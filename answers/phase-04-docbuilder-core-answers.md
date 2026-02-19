# Phase 04 - DocBuilder Core Answers

This phase is for basics to early-intermediate discussion.
Answers are simple to speak, but keep important technical terms.

## Reference Legend

- `D4-1`: `Documents/docbuilder/README.md:21`
- `D4-2`: `Documents/docbuilder/README.md:155`
- `D4-3`: `Documents/docbuilder/backend/main.py:17`
- `D4-4`: `Documents/docbuilder/backend/app/api/endpoints.py:102`
- `D4-5`: `Documents/docbuilder/backend/app/models.py:40`
- `D4-6`: `Documents/docbuilder/backend/app/models.py:22`
- `D4-7`: `Documents/docbuilder/backend/app/models.py:31`
- `D4-8`: `Documents/docbuilder/backend/app/api/endpoints.py:135`
- `D4-9`: `Documents/docbuilder/backend/app/api/endpoints.py:155`
- `D4-10`: `Documents/docbuilder/backend/app/api/endpoints.py:156`
- `D4-11`: `Documents/docbuilder/backend/app/api/endpoints.py:181`
- `D4-12`: `Documents/docbuilder/backend/app/api/endpoints.py:277`
- `D4-13`: `Documents/docbuilder/backend/app/api/endpoints.py:415`
- `D4-14`: `Documents/docbuilder/backend/app/api/endpoints.py:317`
- `D4-15`: `Documents/docbuilder/backend/app/api/endpoints.py:369`
- `D4-16`: `Documents/docbuilder/backend/app/api/endpoints.py:471`
- `D4-17`: `Documents/docbuilder/backend/app/api/endpoints.py:510`
- `D4-18`: `Documents/docbuilder/backend/app/core/llm.py:23`
- `D4-19`: `Documents/docbuilder/backend/app/core/llm.py:36`
- `D4-20`: `Documents/docbuilder/backend/app/core/llm.py:143`
- `D4-21`: `Documents/docbuilder/backend/app/models.py:115`
- `D4-22`: `Documents/docbuilder/backend/app/models.py:119`
- `D4-23`: `Documents/docbuilder/backend/app/core/llm.py:98`
- `D4-24`: `Documents/docbuilder/backend/app/core/llm.py:292`
- `D4-25`: `Documents/docbuilder/backend/app/core/llm.py:302`
- `D4-26`: `Documents/docbuilder/backend/app/core/llm.py:269`
- `D4-27`: `Documents/docbuilder/backend/app/core/rag.py:233`
- `D4-28`: `Documents/docbuilder/backend/app/core/rag.py:54`
- `D4-29`: `Documents/docbuilder/backend/app/core/rag.py:279`
- `D4-30`: `Documents/docbuilder/backend/app/core/rag.py:43`
- `D4-31`: `Documents/docbuilder/backend/app/core/rag.py:49`
- `D4-32`: `Documents/docbuilder/backend/app/core/rag.py:293`
- `D4-33`: `Documents/docbuilder/backend/app/core/rag.py:75`
- `D4-34`: `Documents/docbuilder/backend/app/core/rag.py:167`
- `D4-35`: `Documents/docbuilder/backend/app/core/llm.py:306`
- `D4-36`: `Documents/docbuilder/backend/app/api/endpoints.py:648`
- `D4-37`: `Documents/docbuilder/backend/app/api/endpoints.py:665`
- `D4-38`: `Documents/docbuilder/backend/app/core/llm.py:357`
- `D4-39`: `Documents/docbuilder/backend/app/core/llm.py:423`
- `D4-40`: `Documents/docbuilder/backend/app/core/llm.py:308`
- `D4-41`: `Documents/docbuilder/backend/app/core/llm.py:463`
- `D4-42`: `Documents/docbuilder/backend/app/services/export_service.py:212`
- `D4-43`: `Documents/docbuilder/backend/app/services/export_service.py:413`
- `D4-44`: `Documents/docbuilder/backend/app/services/export_service.py:23`
- `D4-45`: `Documents/docbuilder/backend/app/api/endpoints.py:826`
- `D4-46`: `Documents/docbuilder/backend/app/api/endpoints.py:849`
- `D4-47`: `Documents/docbuilder/backend/app/services/export_service.py:226`
- `D4-48`: `Documents/docbuilder/backend/app/services/export_service.py:122`
- `D4-49`: `Documents/docbuilder/backend/app/services/export_service.py:580`
- `D4-50`: `Documents/docbuilder/frontend/src/context/AuthContext.tsx:17`
- `D4-51`: `Documents/docbuilder/frontend/src/pages/projects/[id].tsx:39`
- `D4-52`: `Documents/docbuilder/frontend/src/pages/projects/[id].tsx:75`
- `D4-53`: `Documents/docbuilder/frontend/src/pages/projects/[id].tsx:145`
- `D4-54`: `Documents/docbuilder/frontend/src/components/SectionList.tsx:91`
- `D4-55`: `Documents/docbuilder/frontend/src/components/SectionUnit.tsx:140`
- `D4-56`: `Documents/docbuilder/frontend/src/components/SectionUnit.tsx:357`
- `D4-57`: `Documents/docbuilder/frontend/src/components/SectionUnit.tsx:412`
- `D4-58`: `Documents/docbuilder/frontend/__tests__/pages.test.tsx:20`
- `D4-59`: `Documents/docbuilder/backend/tests/test_main.py:23`
- `D4-60`: `Documents/docbuilder/backend/tests/test_refinement.py:49`
- `D4-61`: `Documents/docbuilder/backend/tests/test_export.py:24`
- `D4-62`: `Documents/docbuilder/README.md:193`
- `D4-63`: `Documents/docbuilder/README.md:194`
- `D4-64`: `Documents/docbuilder/ENV_SETUP.md:47`
- `D4-65`: `Documents/docbuilder/ENV_SETUP.md:19`
- `D4-66`: `Documents/docbuilder/backend/main.py:22`
- `D4-67`: `Documents/docbuilder/backend/main.py:38`
- `D4-68`: `Documents/docbuilder/backend/app/core/auth.py:20`
- `D4-69`: `Documents/docbuilder/backend/app/core/auth.py:23`
- `D4-70`: `Documents/docbuilder/backend/app/db/firestore.py:11`
- `D4-71`: `Documents/docbuilder/backend/app/db/firestore.py:32`
- `D4-72`: `Documents/docbuilder/backend/requirements.txt:20`
- `D4-73`: `Documents/docbuilder/backend/app/models.py:73`
- `D4-74`: `Documents/docbuilder/backend/app/api/endpoints.py:574`
- `D4-75`: `Documents/docbuilder/backend/app/api/endpoints.py:583`
- `D4-76`: `Documents/docbuilder/backend/app/api/endpoints.py:596`
- `D4-77`: `Documents/docbuilder/backend/app/api/endpoints.py:608`
- `D4-78`: `Documents/docbuilder/backend/app/api/endpoints.py:695`
- `D4-79`: `Documents/docbuilder/backend/app/api/endpoints.py:804`
- `D4-80`: `Documents/docbuilder/backend/app/services/export_service.py:299`
- `D4-81`: `Documents/docbuilder/backend/app/services/export_service.py:317`

## A) Product and Architecture Basics

1. **What user problem does DocBuilder solve?**
Answer: It helps users create structured documents and presentations faster using AI-assisted outline, generation, refinement, and export. So the main problem solved is “high-quality content creation with less manual effort.”
Refs: `D4-1`, `D4-2`

2. **Why split DocBuilder into FastAPI backend and Next.js frontend?**
Answer: The split keeps UI and AI/business logic cleanly separated. Next.js handles product UX, while FastAPI handles auth checks, model orchestration, Firestore writes, and export generation.
Refs: `D4-3`, `D4-62`

3. **What are the main modules in DocBuilder backend?**
Answer: Main backend modules are API endpoints, auth middleware, Firestore adapter, LLM adapter, RAG retriever, and export service. Each module has a clear domain responsibility.
Refs: `D4-3`, `D4-18`, `D4-27`, `D4-42`

4. **How does a user go from "new project" to exported file?**
Answer: User creates project, gets or builds outline, generates/refines section content, then calls export endpoint with `docx` or `pptx`. Backend verifies ownership and streams file with proper headers.
Refs: `D4-4`, `D4-16`, `D4-74`, `D4-79`

5. **Why choose Firestore for this product?**
Answer: Firestore fits fast product iteration with document-style storage and simple per-user ownership queries. It also integrates naturally with Firebase auth ecosystem used by the frontend.
Refs: `D4-8`, `D4-70`, `D4-71`

6. **What data is stored in a project document?**
Answer: Project stores title, doc type, outline sections, slides compatibility field, generation history, owner id, and timestamps. Section objects also include status, content, bullets, comments, and version.
Refs: `D4-5`, `D4-6`, `D4-7`

7. **Why is section-level workflow useful vs generating whole document at once?**
Answer: Section-level flow gives control and iterative quality improvement. Users can regenerate or refine only weak sections instead of rewriting entire document. It is also cheaper and easier to debug.
Refs: `D4-14`, `D4-35`, `D4-74`

8. **What are your key APIs for project lifecycle?**
Answer: Core lifecycle APIs are create/list/get/update/delete project, suggest outline, generate section, refine section, add/delete/reorder section, and export. This covers full content lifecycle end to end.
Refs: `D4-4`, `D4-13`, `D4-16`, `D4-79`

9. **How does frontend authenticate requests to backend?**
Answer: Frontend gets Firebase ID token from current user and sends it in `Authorization: Bearer`. Backend verifies token in dependency middleware before protected endpoint logic.
Refs: `D4-50`, `D4-51`, `D4-69`

10. **What are the biggest current limitations of the architecture?**
Answer: Heavy generation/refinement work is still synchronous in API request path. It can increase latency and make retries harder at higher scale. Queue-based async workers would be better later.
Refs: `D4-74`, `D4-35`, `D4-25`

## B) FastAPI and Endpoint Design

11. **How is router organization done in `endpoints.py`?**
Answer: Endpoints are grouped by domain inside one router: auth/profile, project CRUD, section operations, AI operations, reactions/comments, and export. It is clear but now long, so next step can be file-level split by domain.
Refs: `D4-4`, `D4-13`, `D4-16`, `D4-79`

12. **Which endpoints are CRUD vs AI-specific?**
Answer: CRUD: `/projects`, `/projects/{id}`, section add/delete/reorder/update-content. AI-specific: `/suggest-outline`, `/generate`, `/units/{id}/refine`.
Refs: `D4-4`, `D4-13`, `D4-14`, `D4-16`

13. **How do you enforce ownership checks per project?**
Answer: Endpoint reads project document first, then compares `owner_uid` with authenticated user id. If mismatch, request is rejected before business logic proceeds.
Refs: `D4-9`, `D4-10`, `D4-11`

14. **Why return 404 vs 403 in different conditions?**
Answer: 404 is used when target resource does not exist. 403 is used when resource exists but caller has no permission. This improves API correctness and debugging clarity.
Refs: `D4-9`, `D4-10`

15. **How do you validate request payloads with Pydantic models?**
Answer: Request bodies are typed models like `ProjectCreate`, `GenerateContentRequest`, `RefineRequest`, etc. FastAPI validates shape/types before handler logic, reducing manual checks.
Refs: `D4-5`, `D4-21`, `D4-73`

16. **Why keep `version` in each section object?**
Answer: Section version helps track edits and can support optimistic concurrency later. Every manual edit/comment/refinement increments version.
Refs: `D4-7`, `D4-78`

17. **How is section reorder handled and validated?**
Answer: Backend verifies section count and validates each section id exists before reorder write. If any id is invalid, request fails safely.
Refs: `D4-13`

18. **How is section add/delete implemented safely?**
Answer: Add creates a new UUID section and inserts at requested position or end. Delete checks project ownership and confirms section existed before updating outline.
Refs: `D4-14`, `D4-15`

19. **Why sanitize HTML before saving manual edits?**
Answer: Manual content can include unsafe HTML, so backend sanitizes with allowed tags/attrs using `bleach`. This reduces stored XSS risk.
Refs: `D4-12`

20. **What status transitions can a section have during generation?**
Answer: Section starts as `queued`, moves to `generating` when generation starts, then `done` on success or `failed` on exception. This state model gives clear UI feedback.
Refs: `D4-6`, `D4-74`, `D4-77`

21. **How do you capture generation history for auditability?**
Answer: Generation endpoint builds `GenerationHistoryItem` with timestamp, prompt summary, model meta, response, and hash. Then it appends to `generation_history` in project doc.
Refs: `D4-75`, `D4-76`

22. **How would you paginate list projects endpoint if user has thousands?**
Answer: Add cursor-based pagination with limit and `updated_at` ordering, and return next cursor token. This avoids large full scans and reduces response size.
Refs: `D4-8`, `D4-71`

## C) LLM Integration Basics

23. **Why use adapter pattern (`LLMAdapter`) for model providers?**
Answer: Adapter pattern gives a stable contract (`generate_outline`, `generate_section`, `refine_section`) while allowing provider swap. So mock and real providers can be switched without endpoint rewrite.
Refs: `D4-18`

24. **What does `MockLLMAdapter` help with in development/testing?**
Answer: It gives deterministic outputs without real API cost or network dependency. This is useful for fast local dev and stable tests.
Refs: `D4-19`, `D4-59`

25. **Why use LangChain prompt -> LLM -> parser chain?**
Answer: Chain style makes prompt templating, model call, and structured parse explicit and composable. It reduces glue code and improves readability.
Refs: `D4-20`

26. **Why use Pydantic output parsers for LLM responses?**
Answer: Parser enforces expected schema so backend gets predictable fields. Without this, malformed output can break downstream logic easily.
Refs: `D4-20`, `D4-21`, `D4-22`

27. **How do you generate outline with document-type awareness?**
Answer: Prompt includes different guidance for PPTX vs DOCX: section count, title style, and word-count expectations. So generated structure matches output medium.
Refs: `D4-23`

28. **How do you avoid duplicate sections during suggest-outline?**
Answer: Backend compares new titles against existing titles (case-insensitive) and only appends non-duplicate sections. This prevents repeated outline items.
Refs: `D4-17`

29. **How do you guide LLM to choose bullets vs paragraphs?**
Answer: Prompt has explicit rules based on section type: comparisons/lists/steps use bullets, narrative sections use paragraphs. This rule-based prompt design improves format consistency.
Refs: `D4-26`

30. **Why convert markdown output to HTML before storage?**
Answer: UI editor/render path is HTML-based, so markdown is converted once and stored in final display format. This keeps frontend rendering simple.
Refs: `D4-24`, `D4-56`

31. **What is the fallback when LLM call fails?**
Answer: In real adapter, exceptions are surfaced and endpoint marks section as failed. In local/test mode, mock adapter can be used to keep product flow alive.
Refs: `D4-25`, `D4-77`, `D4-19`

32. **How do you ensure generated text meets target word count approximately?**
Answer: Target word count is passed directly into prompt constraints (`±10%`) and included in structured output. It is guided, not mathematically hard-enforced.
Refs: `D4-26`, `D4-22`

## D) RAG Basics

33. **Explain your RAG pipeline step-by-step.**
Answer: Build query from section/topic, search web, fetch pages, chunk content, embed chunks, run FAISS similarity search, return top context and sources, then inject into generation prompt.
Refs: `D4-27`

34. **Why use Google Custom Search instead of direct crawling only?**
Answer: Search API gives relevant candidate URLs quickly, then scraper fetches only top pages. This is more focused than broad web crawling.
Refs: `D4-28`, `D4-27`

35. **Why use FAISS for similarity search?**
Answer: FAISS is fast and practical for in-memory vector retrieval in request scope. It is good for current project size and prototyping speed.
Refs: `D4-29`

36. **Why use `all-MiniLM-L6-v2` embeddings?**
Answer: It gives a strong quality/speed tradeoff and can run on CPU. That fits cost-sensitive deployment for student/fresher project.
Refs: `D4-30`

37. **How does chunk size/overlap affect retrieval quality?**
Answer: Larger chunk gives more context but can dilute relevance. Overlap helps avoid boundary loss between chunks. Current setup uses 800/100 as a balanced default.
Refs: `D4-31`

38. **How are source URLs returned to the application layer?**
Answer: After similarity search, retriever builds a `sources` array with URL/title from chunk metadata and returns it with context payload.
Refs: `D4-32`

39. **What happens if Google API credentials are missing?**
Answer: Search mode is disabled and retriever falls back to mock context instead of crashing. This keeps generation path usable in development.
Refs: `D4-33`, `D4-34`

40. **Why provide mock fallback context in dev mode?**
Answer: It avoids hard dependency on paid APIs during local development and demo practice. Team can still test complete flow behavior.
Refs: `D4-34`, `D4-19`

41. **How would you explain "RAG reduces hallucination" to a non-AI interviewer?**
Answer: RAG gives the model real source text before it writes, so it relies less on memory guesses. It does not make hallucination zero, but it reduces unsupported claims when retrieval quality is good.
Refs: `D4-27`, `D4-32`

## E) Refinement Engine Basics

42. **What does context-aware refinement mean in your implementation?**
Answer: Refinement prompt includes document title, section position, outline context, adjacent sections, current content, and refinement history. So rewrite is not isolated; it knows document flow.
Refs: `D4-35`, `D4-36`, `D4-37`

43. **Why include previous and next section context in refine prompts?**
Answer: It helps transitions and coherence between neighboring sections. This reduces abrupt tone/logic breaks.
Refs: `D4-36`, `D4-37`

44. **How do you use refinement history with likes/dislikes?**
Answer: History is passed into prompt with reaction markers, so model can prefer previously liked patterns and avoid disliked directions. It is lightweight feedback conditioning.
Refs: `D4-35`, `D4-39`

45. **How do you detect user intent to expand/condense content?**
Answer: Code checks instruction text for keywords like expand/longer/condense/shorter and computes target guidance accordingly. This auto-tunes refinement objective.
Refs: `D4-38`

46. **What does ±30% word-count adjustment mean practically?**
Answer: If user asks to expand, target becomes around 130% of current words; condense means about 70%. Example: 200 words becomes ~260 or ~140.
Refs: `D4-38`

47. **How do you preserve format when user asks for refinements?**
Answer: Prompt explicitly says preserve current format unless user requests change, and includes formatting rules for bullet/paragraph handling. This keeps consistent output style.
Refs: `D4-39`

48. **How do you ensure refine output remains structured and parseable?**
Answer: Refine uses Pydantic output parser with explicit fields (`text`, `bullets`, `diff_summary`). If parser fails, endpoint returns error rather than saving broken shape.
Refs: `D4-40`, `D4-25`

49. **Why convert existing HTML to markdown before refine prompt?**
Answer: Markdown is easier for model reasoning while still preserving structure from HTML. Then output markdown is converted back to HTML for storage.
Refs: `D4-41`, `D4-24`

50. **What are common failure modes in refinement flow?**
Answer: Parser mismatch, malformed model output, or prompt-instruction conflict are common. On error, endpoint returns failure and avoids partial bad writes.
Refs: `D4-25`, `D4-77`

## F) Export Service Basics

51. **How does DOCX export preserve structure and formatting?**
Answer: Export service parses HTML and maps headings, paragraphs, lists, and inline styles into python-docx runs and paragraph styles. It keeps readable structure instead of flat plain text.
Refs: `D4-42`, `D4-48`

52. **How does PPTX export theme selection work?**
Answer: Theme name is passed from API query to export service, then colors/fonts/layout values are loaded from theme dictionary and applied while generating slides.
Refs: `D4-43`, `D4-44`, `D4-79`

53. **Why support 4 themes and what differs between them?**
Answer: Four themes provide style flexibility for business, modern, academic, and creative contexts. They differ in background colors, title/text colors, accent colors, and fonts.
Refs: `D4-44`

54. **How do you sanitize file names before download headers?**
Answer: Export endpoint strips invalid filesystem/header characters from project title using regex. Then it builds safe filename with correct extension.
Refs: `D4-45`

55. **How do you handle content-disposition for UTF-8 filenames?**
Answer: Response sets both `filename` and RFC6266 `filename*` with URL-encoded UTF-8 value for broader browser support. This avoids broken download names.
Refs: `D4-46`

56. **Why exclude summary bullets from DOCX export body?**
Answer: Summary bullets are a UI helper, not core document body. Export intentionally omits them to keep output clean and professional.
Refs: `D4-47`

57. **How do you map HTML elements to docx/pptx formatting?**
Answer: For DOCX, parser handles inline styles and list/heading blocks into runs/paragraph styles. For PPTX, content is parsed into structured items and rendered with dynamic font sizing and bullet levels.
Refs: `D4-48`, `D4-49`

58. **What happens when section content is empty during export?**
Answer: Export still creates section slide/heading structure, but body remains minimal/empty. This avoids crash and keeps file generation successful.
Refs: `D4-42`, `D4-43`

## G) Frontend Flow Basics

59. **How is auth state managed on frontend?**
Answer: Auth context subscribes to Firebase `onAuthStateChanged`, stores `user` and `loading`, and exposes hook to pages/components. This gives centralized auth state.
Refs: `D4-50`

60. **How does project page fetch and refresh data after operations?**
Answer: Project page has `fetchProject()` callback and calls it after generate/add/delete/reorder/save flows. This keeps UI consistent with backend truth.
Refs: `D4-51`, `D4-52`

61. **How is optimistic update used during generation/reorder?**
Answer: During generation it sets section status to `generating` before API returns. For reorder it updates local order and then syncs server, with refresh/revert on failure.
Refs: `D4-53`, `D4-54`

62. **How is drag-and-drop reorder implemented?**
Answer: It uses DnD Kit with sortable context and `arrayMove`. On drag end, it computes new order and calls backend with reordered section IDs.
Refs: `D4-54`

63. **How do manual edit and AI edit workflows coexist in UI?**
Answer: Section UI supports both rich-text manual edit/save and AI refine prompt on same content. User can switch modes without losing current section context.
Refs: `D4-55`, `D4-56`

64. **How do you prevent XSS when rendering generated content?**
Answer: Frontend sanitizes HTML with DOMPurify before `dangerouslySetInnerHTML`. Backend also sanitizes manual update payloads before saving.
Refs: `D4-56`, `D4-12`

65. **How does RAG toggle reach backend generation endpoint?**
Answer: UI checkbox sets `useRag` state, then generate call sends `use_rag` in request body. Backend passes that flag to adapter generation.
Refs: `D4-57`, `D4-74`

66. **What frontend edge cases can break user workflow?**
Answer: Token expiry, stale local ordering, unsaved editor changes, and network failures during optimistic updates are key edge cases. Current flow handles many by refresh + error state, but conflict handling can be stronger.
Refs: `D4-51`, `D4-53`, `D4-54`

## H) Testing and Deployment Basics

67. **What backend tests exist today and what do they cover?**
Answer: Backend tests cover health/basic endpoints, project + generation flow mocks, refinement/comment/like flows, and export file generation sanity. They are mostly unit/integration-with-mocks.
Refs: `D4-59`, `D4-60`, `D4-61`

68. **What frontend tests exist today and what is missing?**
Answer: Frontend currently has smoke tests for login/register rendering. Missing high-value tests are project workflow, reorder, editor save, and export UX states.
Refs: `D4-58`

69. **What is your local setup workflow for backend + frontend?**
Answer: Start backend with Python env and FastAPI server, start frontend with `npm run dev`, and connect via `NEXT_PUBLIC_API_URL`. Configure Firebase + Groq + optional Google CSE env values.
Refs: `D4-64`, `D4-65`

70. **How is deployment split across Railway/Vercel?**
Answer: Backend is deployed on Railway and frontend on Vercel. This separation keeps UI and API deployment independent.
Refs: `D4-62`, `D4-63`

71. **Which env vars are absolutely required for production?**
Answer: Firebase frontend vars, backend Firebase credentials, `LLM_PROVIDER=groq` with `GROQ_API_KEY`, API URL, and CORS allowlist are essential. RAG vars are required only if RAG is enabled.
Refs: `D4-64`, `D4-65`, `D4-66`

72. **If export fails in production, what logs and signals do you inspect first?**
Answer: First inspect export route logs, response status path (docx/pptx branch), and theme/format inputs. Then check file generation function errors and header/stream behavior.
Refs: `D4-79`, `D4-42`, `D4-43`, `D4-67`
