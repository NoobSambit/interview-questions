# Phase 05 - DocBuilder Deep Dive Questions (Intermediate)

## A) LLM Engineering and Prompting

1. Why did you choose Groq Llama 3.3 70B for this workload?
2. How do temperature and retry settings affect consistency and latency?
3. How do you design prompts for deterministic structured output?
4. What failure modes happen with Pydantic parser even after good prompting?
5. How do you recover when parser throws due to malformed LLM output?
6. How do you evaluate output quality beyond "looks good"?
7. How would you add automatic rubric scoring for generated sections?
8. How would you detect repetitive content across sections automatically?
9. How would you make outline generation domain-aware (e.g., legal vs marketing)?
10. How would you support multilingual generation with current architecture?
11. What caching strategies are safe for LLM outputs?
12. How do you prevent prompt injection from user-provided section text?

## B) RAG Quality and Reliability

13. How do you evaluate retrieval relevance quality today?
14. How do you handle low-quality or SEO-spam search results?
15. Why is search query formulation critical for RAG quality?
16. What are drawbacks of pulling top 5 search results only?
17. How do you avoid stale or contradictory web sources in context?
18. How would you add source trust scoring?
19. How would you support citation rendering per sentence/claim?
20. How do you prevent context window overflow with long retrieved content?
21. How would you improve chunking for table-heavy or structured web pages?
22. Why is CPU embedding acceptable or not for scale?
23. How would you shift from on-the-fly FAISS build to persistent vector index?
24. What security risks come from fetching arbitrary URLs?
25. How would you sandbox or filter external content fetches?
26. How do you protect against prompt injection hidden in retrieved pages?

## C) Backend Concurrency and Performance

27. Your generate endpoint is async but performs blocking work. What are implications?
28. Which calls should move to background workers first?
29. How would you redesign generation for queue-based async processing?
30. How would you implement task status polling/websocket updates?
31. How do you avoid race conditions when two tabs edit same section?
32. How would you add optimistic concurrency control using section version?
33. How do you handle partial failure when generation succeeds but DB update fails?
34. How would you design retries with idempotency keys for generate/refine?
35. What latency budget would you define for generate and refine endpoints?
36. How would you profile bottlenecks in end-to-end request path?
37. How would you scale this on Railway with bursty traffic?
38. What memory risks exist for large project outlines and histories?

## D) Security and Auth Depth

39. Current auth allows `mock_token` in code path. Why is this risky in production?
40. How would you enforce environment-based guardrails for test-only auth logic?
41. How do you validate Firebase token claims (issuer/audience/expiry)?
42. What access-control bugs are common in per-resource ownership checks?
43. How do you protect export endpoint from unauthorized document download?
44. What is your strategy for secret management across local/dev/prod?
45. How would you design audit logging for sensitive actions?
46. What CORS misconfigurations could accidentally expose APIs?
47. How do you mitigate stored XSS from rich text content?
48. Which security tests would you automate in CI?

## E) Data Modeling and Firestore

49. Why embed sections inside project doc instead of subcollection design?
50. What are Firestore document size risks for long refinement histories?
51. How would you redesign schema for large enterprise projects?
52. How do you query efficiently for dashboard listing and sorting?
53. How would you add soft-delete and restore flows?
54. How would you handle multi-user collaboration edits?
55. How do you enforce data validation both API-side and DB-side?
56. What migration challenges arise when Pydantic model changes?

## F) Export System Deep Dive

57. How do you preserve nested formatting in DOCX robustly?
58. How do you map markdown/HTML lists to slides cleanly in PPTX?
59. How do you prevent slide overflow for very long section content?
60. How would you support custom brand templates uploaded by enterprise customers?
61. How do you test visual correctness of exported files automatically?
62. How would you benchmark export speed for 100-section projects?
63. How would you stream large export generation without memory spikes?
64. How do you ensure exports are deterministic across platforms?

## G) Scenario and Debugging Questions

65. Users report refine endpoint produces broken HTML. How do you debug and fix?
66. RAG suddenly returns irrelevant contexts for all topics. What could have changed?
67. Generation latency jumps from 6s to 25s overnight. What is your investigation plan?
68. Export works for DOCX but PPTX fails for one theme only. How do you isolate root cause?
69. Intermittent 401 appears only on production auth routes. What do you check first?
70. Firestore quotas are hit unexpectedly. Which operations likely caused it?
71. Frontend reorder appears successful but order reverts after refresh. Where is the bug likely?
72. Users report duplicate sections after suggest-outline. What checks should be tightened?

## H) Testing and Engineering Maturity

73. Which DocBuilder flow deserves end-to-end tests first and why?
74. How would you mock Groq, Google Search, and Firestore in CI?
75. How do you test refinement behavior with deterministic assertions?
76. How would you create golden test fixtures for generated content quality?
77. What observability events would you emit for each generation/refinement/export request?
78. How do you define SLOs for this platform as a fresher-friendly production app?
79. What is your rollout strategy for risky prompt or retrieval changes?
80. If you had 2 weeks to harden DocBuilder for internships/placements demo, what exact tasks would you prioritize?
