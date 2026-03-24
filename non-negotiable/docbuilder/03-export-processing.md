# DocBuilder (Non-Negotiable) - Export & Processing

These answers are based on the actual DocBuilder implementation, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) How did you generate DOCX and PPT?

### Short intro
I generate both file types on the backend. The frontend only asks for the export. The backend reads the project data, builds the file in memory, and sends it back as a download.

### Step-by-step explanation
1. The user clicks export from the frontend and chooses either DOCX or PPTX.
2. The backend first checks that the project belongs to the logged-in user.
3. It reads the stored project content, including sections and generated text.
4. If the user wants a Word file, the backend builds a DOCX document.
5. If the user wants slides, the backend builds a PPTX presentation.
6. The file is created in memory and streamed back to the browser.
7. The browser then downloads it with the correct file extension.

### Design reasoning
I kept export on the backend because file generation libraries are stronger there and the output is more consistent. It also avoids browser-side limitations and keeps sensitive formatting logic in one place.

### Possible improvement
For very large exports, I would run the export asynchronously and notify the user when the file is ready.

### Hard Terms Explained Simply
- `In memory` = the file is created temporarily in RAM instead of being saved permanently first.
- `Streamed back` = the file is sent directly to the browser as it is prepared.

---

## 2) How did you convert HTML to slides?

### Short intro
The section content in DocBuilder is stored as HTML. For PowerPoint export, I read that HTML, understand its structure, and then rebuild it as slide text with formatting.

### Step-by-step explanation
1. Generated content is stored in HTML form because HTML can represent headings, paragraphs, lists, bold text, and italic text.
2. During PPT export, the backend reads the HTML for each section.
3. It parses that HTML and identifies the important content pieces, like headings, bullet points, and normal paragraphs.
4. Then it maps those pieces into PowerPoint text boxes and paragraphs.
5. Bullet levels are used to show nested points.
6. Basic formatting like bold and italic is preserved where possible.
7. A length limit is also applied so one slide does not get overloaded with too much text.

### Design reasoning
HTML is a good middle format because it is rich enough to represent document structure, but still easy to parse and convert into Word or PowerPoint styles.

### Possible improvement
I would add smart slide splitting so long sections automatically flow into multiple slides instead of being squeezed or cut down.

### Hard Terms Explained Simply
- `Parse` = read structured text and break it into understandable pieces.
- `Nested points` = bullet points inside another bullet point.
- `Middle format` = an in-between format used before converting to the final output.

---

## 3) How did you manage themes?

### Short intro
I separated content from visual styling. The text stays the same, but the theme controls how the slides look, like colors and fonts.

### Step-by-step explanation
1. The backend keeps a set of predefined presentation themes.
2. Each theme contains things like background color, title color, body text color, accent color, and font choices.
3. When the user exports to PPTX, they can choose a theme.
4. The export logic uses that selected theme while creating slide backgrounds, titles, and text styles.
5. This gives the same content a different visual look without changing the actual information.

### Design reasoning
This approach keeps the system clean. Content generation and visual styling are treated as separate concerns, which makes customization easier.

### Possible improvement
I would let users create their own custom themes and add checks to ensure text stays readable on the chosen background.

### Hard Terms Explained Simply
- `Accent color` = the highlight color used for important visual parts.
- `Separate concerns` = keep different responsibilities independent so the system is easier to manage.

---

## 4) How did you optimize inference speed?

### Short intro
I improved speed mainly by using caching and by making RAG optional. That way, repeated requests can return faster, and users only pay the extra cost of research when they actually need it.

### Step-by-step explanation
1. LLM calls can be expensive in time, especially if the same prompt is repeated.
2. So I enabled a local cache that can reuse earlier results for repeated prompts.
3. I also kept RAG optional, because web search, scraping, chunking, and retrieval all add extra delay.
4. In the research flow, I limit how much text is collected from each page so processing stays under control.
5. Together, these decisions reduce unnecessary latency without changing the core output flow.

### Design reasoning
For a student project, I wanted practical improvements without building heavy infrastructure. Caching and optional RAG gave a good speed gain for relatively low complexity.

### Possible improvement
I would move caching to a shared service like Redis so the speed benefit works across all deployed instances, not just one machine.

### Hard Terms Explained Simply
- `Inference` = the step where the AI model generates an answer.
- `Cache` = saved output that can be reused instead of recomputing everything.
- `Latency` = the time a user waits before getting a response.
