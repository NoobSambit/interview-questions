# DocBuilder (Non‑Negotiable) — Export & Processing

These answers are written strictly from what exists in the repo: `Documents/docbuilder/`.

Relevant code I am referencing:
1. Export service (DOCX + PPTX): `Documents/docbuilder/backend/app/services/export_service.py`
2. Export endpoint: `Documents/docbuilder/backend/app/api/endpoints.py`
3. Data model fields used in export: `Documents/docbuilder/backend/app/models.py`
4. LLM generation converts markdown to HTML: `Documents/docbuilder/backend/app/core/llm.py`

---

## 1) How did you generate DOCX and PPT?

### Short intro
I generate files on the backend using Python libraries. DOCX is built using `python-docx` and PPTX is built using `python-pptx`. The API streams the generated file back to the browser as a download.

### Step-by-step explanation
1. The frontend hits the export API: `GET /projects/{project_id}/export?format=docx` or `...format=pptx` (implemented in `Documents/docbuilder/backend/app/api/endpoints.py`).
2. The API fetches the project from Firestore and checks ownership (`owner_uid` must match the authenticated user).
3. It then calls `ExportService.generate_docx(project_data)` for DOCX or `ExportService.generate_pptx(project_data, theme_name=theme)` for PPTX (both in `Documents/docbuilder/backend/app/services/export_service.py`).
4. Each generator returns a `BytesIO` file stream.
5. The API responds with `StreamingResponse(...)` and sets `Content-Disposition` so the browser downloads it as `<title>.docx` or `<title>.pptx`.

### Design reasoning (why I did it)
Export is sensitive logic (formatting, fonts, slide layout). Doing it on the backend avoids browser limitations and keeps output consistent.

### Possible improvement (1–2 lines)
For large projects, export can take time; I’d turn export into an async job and notify the user when it’s ready.

### Hard Terms Explained Simply
- `StreamingResponse` = sends the file to the browser without saving it permanently on the server.
- `Content-Disposition` = header that tells the browser “download this file”.

---

## 2) How did you convert HTML to slides?

### Short intro
In my project, the “source content” is stored as HTML in each section. For PPT, I parse that HTML into structured items (headings, bullets, paragraphs) and then render them into a text box on the slide.

### Step-by-step explanation
1. Content is stored as HTML. Section generation converts markdown to HTML using `markdown2.markdown(...)` in `Documents/docbuilder/backend/app/core/llm.py`, and user edits are sanitized with `bleach.clean(...)` in `Documents/docbuilder/backend/app/api/endpoints.py`.
2. PPT generator reads `section.get('content', '')`.
3. It calls `ExportService.parse_html_to_structured_content(content)` in `Documents/docbuilder/backend/app/services/export_service.py`.
4. The parser uses BeautifulSoup to read HTML, extracts structured items (heading/bullet/paragraph), and caps output with a `max_chars` limit so slides don’t overflow too much.
5. The PPT generator renders each structured item as a PowerPoint paragraph, using indentation for bullets (`p.level`), larger/bold text for headings, and normal spacing for paragraphs.
6. It also preserves basic inline formatting by turning bold/italic HTML into separate runs using `ExportService.parse_html_with_formatting()`.

### Design reasoning (why I did it)
HTML is a good intermediate format because it can represent headings, lists, and bold/italic. That maps decently to Word and PowerPoint formatting.

### Possible improvement (1–2 lines)
I’d add smarter slide splitting (auto-create multiple slides when content is too long) instead of truncating or squeezing text.

### Hard Terms Explained Simply
- `Run` = a piece of text with its own style (bold/italic/color).
- `Indent level` = how deep a bullet point is nested.

---

## 3) How did you manage themes?

### Short intro
Themes are a backend feature. I keep a theme dictionary for PPT that controls colors and fonts. The generator uses those values while building slides.

### Step-by-step explanation
1. Themes are defined in `ExportService.THEMES` in `Documents/docbuilder/backend/app/services/export_service.py`.
2. A theme includes background color, title/text/accent colors, and the font name for title and body.
3. The API exposes available themes via `GET /themes` (see `Documents/docbuilder/backend/app/api/endpoints.py`).
4. When exporting PPTX, the API receives a `theme` query param (default `professional`) and `generate_pptx()` uses it to paint background, title bars, and text colors.

### Design reasoning (why I did it)
Users want different “looks” without changing content. A theme layer is a simple way to separate styling from text.

### Possible improvement (1–2 lines)
I’d support custom theme uploads (user-defined colors/fonts) and validate contrast for readability.

### Hard Terms Explained Simply
- `Accent color` = the highlight color used for bars/lines.
- `Contrast` = how readable text is against background.

---

## 4) How did you optimize inference speed?

### Short intro
The main speed improvement I implemented is caching LLM calls using a local SQLite cache, so repeated prompts can return faster. I also keep RAG optional so users only pay the cost when needed.

### Step-by-step explanation
1. I enabled LangChain caching in `Documents/docbuilder/backend/app/core/llm.py` using `set_llm_cache(SQLiteCache(database_path=".langchain_cache.db"))`, so repeated prompts can reuse cached results.
2. I keep RAG optional (`use_rag` defaults to false), so normal generation can skip web search + scraping.
3. In RAG, fetched page text is capped (example: `return text[:5000]` in `Documents/docbuilder/backend/app/core/rag.py`), which keeps context building faster.

Important “implementation truth” note:
1. This cache is stored in a local file `.langchain_cache.db`.
2. In many cloud environments, local disk is ephemeral or per-instance. So this cache helps mostly per instance, not globally.

### Design reasoning (why I did it)
As a student project, caching is the simplest way to get “feels faster” improvements without building complex infra.

### Possible improvement (1–2 lines)
I would move cache to a shared store (like Redis) so it works across deployments/instances.

### Hard Terms Explained Simply
- `Inference` = the model generating output for your prompt.
- `Cache` = save results so the next time it’s faster.
