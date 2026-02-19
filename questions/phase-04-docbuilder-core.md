# Phase 04 - DocBuilder Core Questions (Basics to Early Intermediate)

## A) Product and Architecture Basics

1. What user problem does DocBuilder solve?
2. Why split DocBuilder into FastAPI backend and Next.js frontend?
3. What are the main modules in DocBuilder backend?
4. How does a user go from "new project" to exported file?
5. Why choose Firestore for this product?
6. What data is stored in a project document?
7. Why is section-level workflow useful vs generating whole document at once?
8. What are your key APIs for project lifecycle?
9. How does frontend authenticate requests to backend?
10. What are the biggest current limitations of the architecture?

## B) FastAPI and Endpoint Design

11. How is router organization done in `endpoints.py`?
12. Which endpoints are CRUD vs AI-specific?
13. How do you enforce ownership checks per project?
14. Why return 404 vs 403 in different conditions?
15. How do you validate request payloads with Pydantic models?
16. Why keep `version` in each section object?
17. How is section reorder handled and validated?
18. How is section add/delete implemented safely?
19. Why sanitize HTML before saving manual edits?
20. What status transitions can a section have during generation?
21. How do you capture generation history for auditability?
22. How would you paginate list projects endpoint if user has thousands?

## C) LLM Integration Basics

23. Why use adapter pattern (`LLMAdapter`) for model providers?
24. What does `MockLLMAdapter` help with in development/testing?
25. Why use LangChain prompt -> LLM -> parser chain?
26. Why use Pydantic output parsers for LLM responses?
27. How do you generate outline with document-type awareness?
28. How do you avoid duplicate sections during suggest-outline?
29. How do you guide LLM to choose bullets vs paragraphs?
30. Why convert markdown output to HTML before storage?
31. What is the fallback when LLM call fails?
32. How do you ensure generated text meets target word count approximately?

## D) RAG Basics

33. Explain your RAG pipeline step-by-step.
34. Why use Google Custom Search instead of direct crawling only?
35. Why use FAISS for similarity search?
36. Why use `all-MiniLM-L6-v2` embeddings?
37. How does chunk size/overlap affect retrieval quality?
38. How are source URLs returned to the application layer?
39. What happens if Google API credentials are missing?
40. Why provide mock fallback context in dev mode?
41. How would you explain "RAG reduces hallucination" to a non-AI interviewer?

## E) Refinement Engine Basics

42. What does context-aware refinement mean in your implementation?
43. Why include previous and next section context in refine prompts?
44. How do you use refinement history with likes/dislikes?
45. How do you detect user intent to expand/condense content?
46. What does ±30% word-count adjustment mean practically?
47. How do you preserve format when user asks for refinements?
48. How do you ensure refine output remains structured and parseable?
49. Why convert existing HTML to markdown before refine prompt?
50. What are common failure modes in refinement flow?

## F) Export Service Basics

51. How does DOCX export preserve structure and formatting?
52. How does PPTX export theme selection work?
53. Why support 4 themes and what differs between them?
54. How do you sanitize file names before download headers?
55. How do you handle content-disposition for UTF-8 filenames?
56. Why exclude summary bullets from DOCX export body?
57. How do you map HTML elements to docx/pptx formatting?
58. What happens when section content is empty during export?

## G) Frontend Flow Basics

59. How is auth state managed on frontend?
60. How does project page fetch and refresh data after operations?
61. How is optimistic update used during generation/reorder?
62. How is drag-and-drop reorder implemented?
63. How do manual edit and AI edit workflows coexist in UI?
64. How do you prevent XSS when rendering generated content?
65. How does RAG toggle reach backend generation endpoint?
66. What frontend edge cases can break user workflow?

## H) Testing and Deployment Basics

67. What backend tests exist today and what do they cover?
68. What frontend tests exist today and what is missing?
69. What is your local setup workflow for backend + frontend?
70. How is deployment split across Railway/Vercel?
71. Which env vars are absolutely required for production?
72. If export fails in production, what logs and signals do you inspect first?
