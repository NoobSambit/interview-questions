# DocBuilder - Important Long-Form Interview Questions

These are explain-style questions for 3-8 minute answers.
Focus on generation pipeline, RAG quality, backend reliability, and product reasoning.

## A) Project Story and Architecture

1. Give a full 5-minute architecture walkthrough of DocBuilder from user input to generated/exported output.
2. Explain the core problem DocBuilder solves and why this problem matters in real-world teams.
3. Walk through the end-to-end flow: project creation -> outline generation -> section generation -> refine -> export.
4. Why did you choose this architecture as a fresher project instead of splitting everything into many services?
5. Which component is the biggest technical differentiator in DocBuilder and why?
6. If traffic suddenly grows 10x, which part of the current architecture will fail first and why?
7. What are the top 3 engineering risks in this system and your mitigation strategy for each?
8. If you had to explain DocBuilder in 90 seconds to a hiring manager, what would you emphasize?

## B) LLM and Prompt Engineering

9. Explain your LLM selection logic and tradeoffs: quality, latency, and cost.
10. Walk through prompt design for structured output and why parsing safety matters.
11. How do you reduce hallucinations while keeping content useful and readable?
12. What failure modes happen when model output is malformed, and how do you recover gracefully?
13. How do you evaluate generation quality beyond "looks good to me"?
14. If model behavior changes after an update, how do you detect regression quickly?
15. How would you add domain-specific generation style (legal, marketing, technical) cleanly?
16. What is your fallback strategy when model provider is slow or unavailable?

## C) RAG and Retrieval Quality

17. Explain your full RAG pipeline step-by-step and where relevance quality can degrade.
18. How do you choose external sources and reduce low-trust/noisy content impact?
19. How do you control context size so retrieval helps instead of confusing the model?
20. Explain chunking and embedding choices, and why they are acceptable for current scale.
21. If retrieval quality drops suddenly, what is your debugging sequence?
22. How would you implement citation-quality answers (sentence-level source mapping)?
23. How do you handle stale references or contradictory sources in retrieved context?
24. What security issues come from retrieving arbitrary web content, and how would you harden this?

## D) Backend Reliability and Concurrency

25. Explain how your async API flows handle long-running generation operations.
26. Which operations should move to background jobs first, and why?
27. How would you redesign this for queue-based processing with status tracking?
28. Two tabs edit same section at once. Explain the race condition and your conflict-resolution approach.
29. Generation succeeds but DB write fails. How do you prevent user-visible inconsistency?
30. How do idempotency keys help in generate/refine/export APIs?
31. If latency jumps from 6s to 25s overnight, what exact triage plan do you follow?
32. What are your top observability signals for backend health and user experience?

## E) Data Modeling and Firestore

33. Explain your Firestore data model and why you chose it.
34. What limits can hurt you as project documents grow large, and how do you redesign schema?
35. How do you keep project listing and filtering efficient as user count grows?
36. What migration strategy would you follow when your data model evolves?
37. How would you support safe soft-delete and restore of projects?
38. What access-control mistakes are common in document ownership models, and how do you avoid them?

## F) Export and Document Fidelity

39. Explain your export architecture for DOCX/PPTX and key formatting challenges.
40. How do you preserve headings, lists, and hierarchy consistently across formats?
41. What do you do when content is too long for slides and formatting starts breaking?
42. How would you support custom branding templates for enterprise users?
43. How would you test export correctness beyond "file opens successfully"?
44. If DOCX works but PPTX fails on one project, how do you isolate the root cause?

## G) Security and Production Hardening

45. Explain your auth and authorization flow for DocBuilder APIs.
46. What are the risks of any mock/test auth path leaking into production?
47. How do you secure secrets, provider keys, and environment configuration?
48. Which 5 production security controls would you implement first?
49. How do you protect against prompt injection and content injection attacks?
50. How do you audit sensitive actions like export/download/share?

## H) Reflection and Growth

51. What did this project teach you about AI product engineering beyond simple API calls?
52. Which technical decision would you change first if rebuilding from scratch?
53. Where did you trade correctness for speed, and was it the right call?
54. If you get 2 weeks before interviews, what exact hardening roadmap would you execute?
55. If an interviewer says "This is a good demo but not production-ready," how do you answer with specifics?
