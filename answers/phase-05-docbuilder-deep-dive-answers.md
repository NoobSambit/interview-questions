# Phase 05 - DocBuilder Deep Dive Answers

This phase is for intermediate/deep technical rounds.
Answers stay simple in wording but include key engineering terminology.

## Reference Legend

- `D5-1`: `Documents/docbuilder/backend/app/core/llm.py:77`
- `D5-2`: `Documents/docbuilder/backend/app/core/llm.py:80`
- `D5-3`: `Documents/docbuilder/backend/app/core/llm.py:81`
- `D5-4`: `Documents/docbuilder/backend/app/core/llm.py:96`
- `D5-5`: `Documents/docbuilder/backend/app/core/llm.py:143`
- `D5-6`: `Documents/docbuilder/backend/app/core/llm.py:147`
- `D5-7`: `Documents/docbuilder/backend/app/core/llm.py:156`
- `D5-8`: `Documents/docbuilder/backend/app/core/llm.py:162`
- `D5-9`: `Documents/docbuilder/backend/app/core/llm.py:277`
- `D5-10`: `Documents/docbuilder/backend/app/core/llm.py:302`
- `D5-11`: `Documents/docbuilder/backend/app/core/llm.py:292`
- `D5-12`: `Documents/docbuilder/backend/app/core/llm.py:308`
- `D5-13`: `Documents/docbuilder/backend/app/core/llm.py:329`
- `D5-14`: `Documents/docbuilder/backend/app/core/llm.py:357`
- `D5-15`: `Documents/docbuilder/backend/app/core/llm.py:463`
- `D5-16`: `Documents/docbuilder/backend/app/core/llm.py:483`
- `D5-17`: `Documents/docbuilder/backend/app/core/llm.py:511`
- `D5-18`: `Documents/docbuilder/backend/app/core/rag.py:42`
- `D5-19`: `Documents/docbuilder/backend/app/core/rag.py:49`
- `D5-20`: `Documents/docbuilder/backend/app/core/rag.py:55`
- `D5-21`: `Documents/docbuilder/backend/app/core/rag.py:58`
- `D5-22`: `Documents/docbuilder/backend/app/core/rag.py:65`
- `D5-23`: `Documents/docbuilder/backend/app/core/rag.py:79`
- `D5-24`: `Documents/docbuilder/backend/app/core/rag.py:98`
- `D5-25`: `Documents/docbuilder/backend/app/core/rag.py:124`
- `D5-26`: `Documents/docbuilder/backend/app/core/rag.py:113`
- `D5-27`: `Documents/docbuilder/backend/app/core/rag.py:144`
- `D5-28`: `Documents/docbuilder/backend/app/core/rag.py:153`
- `D5-29`: `Documents/docbuilder/backend/app/core/rag.py:167`
- `D5-30`: `Documents/docbuilder/backend/app/core/rag.py:182`
- `D5-31`: `Documents/docbuilder/backend/app/core/rag.py:219`
- `D5-32`: `Documents/docbuilder/backend/app/core/rag.py:267`
- `D5-33`: `Documents/docbuilder/backend/app/core/rag.py:279`
- `D5-34`: `Documents/docbuilder/backend/app/core/rag.py:282`
- `D5-35`: `Documents/docbuilder/backend/app/core/rag.py:292`
- `D5-36`: `Documents/docbuilder/backend/app/core/rag.py:307`
- `D5-37`: `Documents/docbuilder/backend/app/api/endpoints.py:528`
- `D5-38`: `Documents/docbuilder/backend/app/api/endpoints.py:551`
- `D5-39`: `Documents/docbuilder/backend/app/api/endpoints.py:574`
- `D5-40`: `Documents/docbuilder/backend/app/api/endpoints.py:596`
- `D5-41`: `Documents/docbuilder/backend/app/api/endpoints.py:608`
- `D5-42`: `Documents/docbuilder/backend/app/api/endpoints.py:614`
- `D5-43`: `Documents/docbuilder/backend/app/api/endpoints.py:695`
- `D5-44`: `Documents/docbuilder/backend/app/api/endpoints.py:300`
- `D5-45`: `Documents/docbuilder/backend/app/models.py:31`
- `D5-46`: `Documents/docbuilder/backend/app/core/auth.py:20`
- `D5-47`: `Documents/docbuilder/backend/app/core/auth.py:23`
- `D5-48`: `Documents/docbuilder/backend/main.py:22`
- `D5-49`: `Documents/docbuilder/backend/main.py:36`
- `D5-50`: `Documents/docbuilder/backend/app/api/endpoints.py:820`
- `D5-51`: `Documents/docbuilder/backend/app/api/endpoints.py:826`
- `D5-52`: `Documents/docbuilder/backend/app/api/endpoints.py:839`
- `D5-53`: `Documents/docbuilder/backend/app/db/firestore.py:32`
- `D5-54`: `Documents/docbuilder/backend/app/models.py:43`
- `D5-55`: `Documents/docbuilder/backend/app/models.py:29`
- `D5-56`: `Documents/docbuilder/backend/app/models.py:45`
- `D5-57`: `Documents/docbuilder/backend/app/models.py:73`
- `D5-58`: `Documents/docbuilder/backend/requirements.txt:20`
- `D5-59`: `Documents/docbuilder/backend/requirements.txt:21`
- `D5-60`: `Documents/docbuilder/backend/requirements.txt:26`
- `D5-61`: `Documents/docbuilder/backend/app/services/export_service.py:23`
- `D5-62`: `Documents/docbuilder/backend/app/services/export_service.py:122`
- `D5-63`: `Documents/docbuilder/backend/app/services/export_service.py:320`
- `D5-64`: `Documents/docbuilder/backend/app/services/export_service.py:267`
- `D5-65`: `Documents/docbuilder/backend/app/services/export_service.py:584`
- `D5-66`: `Documents/docbuilder/backend/tests/test_refinement.py:49`
- `D5-67`: `Documents/docbuilder/backend/tests/test_export.py:24`
- `D5-68`: `Documents/docbuilder/backend/tests/test_main.py:33`
- `D5-69`: `Documents/docbuilder/frontend/src/pages/projects/[id].tsx:247`
- `D5-70`: `Documents/docbuilder/README.md:193`
- `D5-71`: `Documents/docbuilder/RAG_FEATURE.md:210`
- `D5-72`: `Documents/docbuilder/RAG_FEATURE.md:211`
- `D5-73`: `Documents/docbuilder/RAG_FEATURE.md:118`
- `D5-74`: `Documents/docbuilder/RAG_FEATURE.md:301`
- `D5-75`: `Documents/docbuilder/backend/app/models.py:105`
- `D5-76`: `Documents/docbuilder/backend/app/api/endpoints.py:444`
- `D5-77`: `Documents/docbuilder/backend/app/api/endpoints.py:626`
- `D5-78`: `Documents/docbuilder/backend/app/api/endpoints.py:128`
- `D5-79`: `Documents/docbuilder/backend/app/api/endpoints.py:680`
- `D5-80`: `Documents/docbuilder/backend/main.py:38`
- `D5-81`: `Documents/docbuilder/backend/app/api/endpoints.py:317`
- `D5-82`: `Documents/docbuilder/backend/app/core/llm.py:21`
- `D5-83`: `Documents/docbuilder/backend/app/core/rag.py:111`

## A) LLM Engineering and Prompting

1. **Why did you choose Groq Llama 3.3 70B for this workload?**
Answer: It gives strong output quality with fast inference for generation/refinement workflows. The model setup already exists in adapter and is easy to plug into chain-based prompting.
Refs: `D5-1`, `D5-17`

2. **How do temperature and retry settings affect consistency and latency?**
Answer: Higher temperature increases creativity but can reduce output consistency for structured schema responses. Retries improve success rate on transient failures but add extra latency.
Refs: `D5-2`, `D5-3`

3. **How do you design prompts for deterministic structured output?**
Answer: I use strict system instructions plus Pydantic format instructions and a parser-backed chain. This forces the model to target known keys and structure.
Refs: `D5-4`, `D5-5`

4. **What failure modes happen with Pydantic parser even after good prompting?**
Answer: Model can still return invalid JSON shape, missing fields, or mixed prose outside schema. Parser then throws and endpoint should handle the error safely.
Refs: `D5-7`, `D5-10`, `D5-12`

5. **How do you recover when parser throws due to malformed LLM output?**
Answer: Current behavior is fail request and return explicit error; section status moves to `failed` in generation flow. Next hardening step is controlled retry or repair pass before failing.
Refs: `D5-10`, `D5-41`

6. **How do you evaluate output quality beyond "looks good"?**
Answer: Check schema validity, word-count target adherence, section relevance, repetition rate, and factual grounding when RAG is enabled. Use rubric-based evaluation with examples.
Refs: `D5-8`, `D5-14`, `D5-35`

7. **How would you add automatic rubric scoring for generated sections?**
Answer: Add post-generation scorer with dimensions like relevance, clarity, coverage, and format compliance. Persist score in generation history and track drift over time.
Refs: `D5-40`, `D5-56`

8. **How would you detect repetitive content across sections automatically?**
Answer: Compute sentence/embedding similarity between section outputs and flag high-overlap pairs. Also compare against outline intent so each section keeps unique information.
Refs: `D5-18`, `D5-33`

9. **How would you make outline generation domain-aware (e.g., legal vs marketing)?**
Answer: Add domain field in request and inject domain-specific constraints/examples in prompt template. Keep separate rubric templates per domain.
Refs: `D5-4`, `D5-5`, `D5-23`

10. **How would you support multilingual generation with current architecture?**
Answer: Add language option in request model and propagate into outline/generate/refine prompts. Keep language-aware summaries and export fonts where needed.
Refs: `D5-37`, `D5-42`, `D5-57`

11. **What caching strategies are safe for LLM outputs?**
Answer: Safe cache keys must include topic, section title, doc type, prompt version, and language. Current SQLite LLM cache helps repeated identical prompts but should be invalidated on prompt changes.
Refs: `D5-82`, `D5-37`

12. **How do you prevent prompt injection from user-provided section text?**
Answer: Treat user text as data context, not system instruction, and keep strict system priority rules. Add sanitization/guard rules for suspicious instruction patterns before model call.
Refs: `D5-12`, `D5-39`, `D5-15`

## B) RAG Quality and Reliability

13. **How do you evaluate retrieval relevance quality today?**
Answer: Check whether returned chunks actually support section intent and if sources are meaningful for the query. Current system logs query/chunks/sources, so manual QA is possible.
Refs: `D5-35`, `D5-39`

14. **How do you handle low-quality or SEO-spam search results?**
Answer: Add domain allowlist/denylist and quality filters before chunking. Today it fetches top results directly, so source filtering is an important improvement.
Refs: `D5-22`, `D5-25`, `D5-74`

15. **Why is search query formulation critical for RAG quality?**
Answer: If query is vague, retrieval pulls weak pages and model gets weak context. Query builder currently extracts key terms and removes filler words for better precision.
Refs: `D5-23`, `D5-24`, `D5-83`

16. **What are drawbacks of pulling top 5 search results only?**
Answer: It may miss niche but high-quality sources and can overfit to early-ranked pages. For complex topics, dynamic `k` or reranking usually gives better recall.
Refs: `D5-22`, `D5-28`

17. **How do you avoid stale or contradictory web sources in context?**
Answer: Add source timestamp checks and trust scoring before final context assembly. Also deduplicate conflicting claims and prefer authoritative domains.
Refs: `D5-35`, `D5-74`

18. **How would you add source trust scoring?**
Answer: Assign base domain trust, freshness score, and content-quality score, then rank chunks by weighted total. Keep score metadata with each source for transparency.
Refs: `D5-35`, `D5-74`

19. **How would you support citation rendering per sentence/claim?**
Answer: Keep chunk ids while generating and ask model to map claims to source ids. Then store citation map and render inline superscripts in frontend.
Refs: `D5-35`, `D5-39`

20. **How do you prevent context window overflow with long retrieved content?**
Answer: Limit page text length, chunk content, and only pass top relevant chunks. This keeps prompt size bounded and stable.
Refs: `D5-27`, `D5-32`, `D5-34`

21. **How would you improve chunking for table-heavy or structured web pages?**
Answer: Use structure-aware splitters (headings/tables/lists) instead of raw character chunks only. Keep table rows grouped as semantic units.
Refs: `D5-19`, `D5-32`

22. **Why is CPU embedding acceptable or not for scale?**
Answer: It is acceptable for low/moderate traffic and cost-sensitive setup. At higher load, embedding latency grows and GPU/async batch embedding becomes better.
Refs: `D5-18`, `D5-71`

23. **How would you shift from on-the-fly FAISS build to persistent vector index?**
Answer: Move indexing to background ingestion and store vectors in persistent DB/service. Query path then only does retrieval, not full rebuild per request.
Refs: `D5-33`, `D5-74`

24. **What security risks come from fetching arbitrary URLs?**
Answer: Risks include SSRF-like abuse, malicious payloads, and prompt injection hidden in page content. Also network timeout abuse can exhaust resources.
Refs: `D5-25`, `D5-26`, `D5-60`

25. **How would you sandbox or filter external content fetches?**
Answer: Restrict protocols/domains, block private IP ranges, set strict timeout/content-size limits, and sanitize extracted text. Run fetch worker with limited network permissions.
Refs: `D5-25`, `D5-26`, `D5-27`

26. **How do you protect against prompt injection hidden in retrieved pages?**
Answer: Strip suspicious instruction-like patterns from retrieved text and separate “facts” from “instructions” in prompt template. Keep hard system guardrails that user/retrieved content cannot override.
Refs: `D5-39`, `D5-35`

## C) Backend Concurrency and Performance

27. **Your generate endpoint is async but performs blocking work. What are implications?**
Answer: The route is async but model call and heavy processing are still request-bound, so long operations tie up worker capacity. Under burst load, latency and timeout risk increase.
Refs: `D5-37`, `D5-9`, `D5-72`

28. **Which calls should move to background workers first?**
Answer: First move generation with RAG, refinement, and large exports. These are heavier and user can wait via job-status UX.
Refs: `D5-39`, `D5-42`, `D5-43`

29. **How would you redesign generation for queue-based async processing?**
Answer: API should enqueue job and return `jobId`; worker executes LLM/RAG and writes result/status. Frontend polls or subscribes for completion.
Refs: `D5-37`, `D5-38`, `D5-43`

30. **How would you implement task status polling/websocket updates?**
Answer: Add `jobs` store with states (`queued/running/succeeded/failed`) and timestamps. Poll endpoint for simple version; websocket/SSE for real-time updates.
Refs: `D5-38`, `D5-43`

31. **How do you avoid race conditions when two tabs edit same section?**
Answer: Use section `version` in update request and reject stale writes (`409 Conflict`). Current model already has version field, so optimistic concurrency is natural next step.
Refs: `D5-45`, `D5-44`

32. **How would you add optimistic concurrency control using section version?**
Answer: Client sends expected version; server compares with stored version before update. If mismatch, return conflict and ask client to refresh/merge.
Refs: `D5-45`, `D5-44`

33. **How do you handle partial failure when generation succeeds but DB update fails?**
Answer: Keep generation output in temporary job record and retry DB write with idempotency key. If still failing, surface retry action to user instead of dropping content.
Refs: `D5-40`, `D5-41`, `D5-53`

34. **How would you design retries with idempotency keys for generate/refine?**
Answer: Add `requestId` per operation and store last successful result hash per section. Retry with same key returns same saved result, preventing duplicate history noise.
Refs: `D5-40`, `D5-79`

35. **What latency budget would you define for generate and refine endpoints?**
Answer: Practical targets: generate 8-12s normal (20s upper bound), refine 4-8s normal. With RAG enabled, allow higher budget or async mode.
Refs: `D5-71`, `D5-72`

36. **How would you profile bottlenecks in end-to-end request path?**
Answer: Measure per-stage timing: auth, DB read, RAG search/fetch/embed, LLM call, DB write. Add structured logs with request id and stage durations.
Refs: `D5-47`, `D5-39`, `D5-35`, `D5-40`

37. **How would you scale this on Railway with bursty traffic?**
Answer: Add queue workers for heavy jobs, apply rate limits, and use autoscaling + concurrency settings. Keep synchronous API thin and predictable.
Refs: `D5-70`, `D5-37`, `D5-42`

38. **What memory risks exist for large project outlines and histories?**
Answer: Embedded outline + refinement history + generation history can make project document very large. Large docs increase read/write cost and risk size limits.
Refs: `D5-54`, `D5-55`, `D5-56`

## D) Security and Auth Depth

39. **Current auth allows `mock_token` in code path. Why is this risky in production?**
Answer: It is a bypass path that can grant access without real token verification. If accidentally enabled in production, it is a critical auth vulnerability.
Refs: `D5-46`, `D5-68`

40. **How would you enforce environment-based guardrails for test-only auth logic?**
Answer: Wrap mock-token branch behind explicit `ENV=development/test` check and fail hard in production. Add CI policy to block release if mock path is active.
Refs: `D5-46`, `D5-47`

41. **How do you validate Firebase token claims (issuer/audience/expiry)?**
Answer: Use Firebase Admin `verify_id_token` and enforce claim checks server-side. Also reject missing/invalid bearer headers before endpoint logic.
Refs: `D5-47`

42. **What access-control bugs are common in per-resource ownership checks?**
Answer: Common bugs are checking auth but forgetting ownership, or checking ownership after mutation. Current pattern checks owner before updates, which is correct baseline.
Refs: `D5-77`, `D5-50`

43. **How do you protect export endpoint from unauthorized document download?**
Answer: Export route verifies auth and compares `owner_uid` with current user before generating file. Unauthorized callers get `403`.
Refs: `D5-50`, `D5-79`

44. **What is your strategy for secret management across local/dev/prod?**
Answer: Keep secrets in env vars only, never in repo, and use provider secret manager for production. Separate keys by environment and rotate periodically.
Refs: `D5-70`, `D5-48`

45. **How would you design audit logging for sensitive actions?**
Answer: Log user id, resource id, action type, status, and timestamp for auth-sensitive actions like export, delete, refine, and auth failures. Avoid logging raw secrets or full content bodies.
Refs: `D5-79`, `D5-42`, `D5-80`

46. **What CORS misconfigurations could accidentally expose APIs?**
Answer: Overly broad origins or wildcard methods/headers with credentials can increase attack surface. Keep strict allowlist and environment-specific origin control.
Refs: `D5-48`, `D5-49`

47. **How do you mitigate stored XSS from rich text content?**
Answer: Sanitize at write time in backend and sanitize again at render time in frontend. Defense-in-depth is important for user-editable HTML.
Refs: `D5-44`, `D5-12`

48. **Which security tests would you automate in CI?**
Answer: Auth bypass tests, ownership tests, XSS sanitization tests, and export access tests are must-have. Add dependency scanning and secret scanning too.
Refs: `D5-66`, `D5-67`, `D5-68`

## E) Data Modeling and Firestore

49. **Why embed sections inside project doc instead of subcollection design?**
Answer: Embedding keeps single-read project loading simple and fast at small/medium size. It reduces query complexity for student-stage product.
Refs: `D5-54`

50. **What are Firestore document size risks for long refinement histories?**
Answer: Each refinement adds text/history arrays inside section, so document can grow quickly. Large documents increase latency and can hit size limits.
Refs: `D5-55`, `D5-56`

51. **How would you redesign schema for large enterprise projects?**
Answer: Move sections and refinement history to subcollections, keep project doc as metadata index. This improves write contention and document-size safety.
Refs: `D5-54`, `D5-55`

52. **How do you query efficiently for dashboard listing and sorting?**
Answer: Add pagination and sort fields like `updated_at` with cursor-based fetch. Current list endpoint is simple stream and should be improved for large scale.
Refs: `D5-78`

53. **How would you add soft-delete and restore flows?**
Answer: Add `deleted_at` and `is_deleted` flags instead of hard delete. Filter active documents by default and add restore endpoint with ownership check.
Refs: `D5-77`

54. **How would you handle multi-user collaboration edits?**
Answer: Add collaborator roles, per-section locking or version conflict handling, and operation logs. Start with optimistic concurrency and merge workflows.
Refs: `D5-45`, `D5-44`

55. **How do you enforce data validation both API-side and DB-side?**
Answer: API uses Pydantic/FastAPI models for input validation. DB side can add validation rules, but currently strong checks are mostly application-layer.
Refs: `D5-57`, `D5-53`

56. **What migration challenges arise when Pydantic model changes?**
Answer: Old stored docs may miss new required fields or have old shapes. Migration scripts and backward-compatible defaults are needed during rollout.
Refs: `D5-54`, `D5-55`, `D5-56`

## F) Export System Deep Dive

57. **How do you preserve nested formatting in DOCX robustly?**
Answer: Parse HTML recursively and convert inline tags into styled runs while respecting lists/headings. Preserve spacing/line-height for readability.
Refs: `D5-62`

58. **How do you map markdown/HTML lists to slides cleanly in PPTX?**
Answer: Parse content into structured `paragraph/bullet/heading` items, then map bullets to paragraph levels with spacing rules. This keeps slide layout readable.
Refs: `D5-63`, `D5-65`

59. **How do you prevent slide overflow for very long section content?**
Answer: Use dynamic font scaling and content truncation/structuring heuristics. For very long content, prefer notes or splitting into multiple slides in future.
Refs: `D5-64`, `D5-65`

60. **How would you support custom brand templates uploaded by enterprise customers?**
Answer: Add template storage model with locked font/color/layout schema and validation. Export service loads tenant template instead of fixed `THEMES` map.
Refs: `D5-61`, `D5-43`

61. **How do you test visual correctness of exported files automatically?**
Answer: Combine structural assertions (file opens, slide count, text presence) with snapshot/image comparison for key templates. Keep golden files in CI.
Refs: `D5-67`, `D5-61`

62. **How would you benchmark export speed for 100-section projects?**
Answer: Generate synthetic large projects and measure end-to-end export time and memory. Track p50/p95 and identify hotspots in HTML parsing and slide rendering loops.
Refs: `D5-63`, `D5-65`

63. **How would you stream large export generation without memory spikes?**
Answer: Use chunked temp-file approach or incremental writing where library allows, instead of holding all content in memory. Return streamed file handle when ready.
Refs: `D5-79`, `D5-42`, `D5-43`

64. **How do you ensure exports are deterministic across platforms?**
Answer: Keep fixed theme/style rules, stable section ordering, and controlled parser behavior. Avoid non-deterministic timestamps/content in visual body.
Refs: `D5-61`, `D5-76`

## G) Scenario and Debugging Questions

65. **Users report refine endpoint produces broken HTML. How do you debug and fix?**
Answer: Reproduce with failing prompt, inspect parser output before markdown->HTML conversion, and validate HTML sanitization/render path. Add regression test for that prompt pattern.
Refs: `D5-12`, `D5-15`, `D5-16`, `D5-66`

66. **RAG suddenly returns irrelevant contexts for all topics. What could have changed?**
Answer: Check Google CSE config/credentials, query formulation changes, and chunking/index steps. Also verify fallback mode was not silently activated due to credential failure.
Refs: `D5-20`, `D5-23`, `D5-32`, `D5-73`

67. **Generation latency jumps from 6s to 25s overnight. What is your investigation plan?**
Answer: Split latency by stage: auth, DB read, RAG fetch/embedding/search, LLM call, DB write. Compare before/after deployment and provider/API health.
Refs: `D5-39`, `D5-71`, `D5-72`, `D5-40`

68. **Export works for DOCX but PPTX fails for one theme only. How do you isolate root cause?**
Answer: Reproduce with same content across all themes, then diff the failing theme config and slide rendering branch. Add unit test on that theme to lock fix.
Refs: `D5-61`, `D5-43`, `D5-67`

69. **Intermittent 401 appears only on production auth routes. What do you check first?**
Answer: Check token issuer/audience/expiry validation, clock skew, and frontend token refresh behavior. Also verify no environment mismatch between Firebase projects.
Refs: `D5-47`, `D5-46`, `D5-80`

70. **Firestore quotas are hit unexpectedly. Which operations likely caused it?**
Answer: High-frequency full-project updates (outline replacement) and repeated generate/refine writes can spike reads/writes. Large embedded documents increase cost per operation.
Refs: `D5-40`, `D5-43`, `D5-54`

71. **Frontend reorder appears successful but order reverts after refresh. Where is the bug likely?**
Answer: Usually payload mismatch (`section_ids`) or backend reorder validation failure, followed by client refresh to old order. Check API response and saved outline order in DB.
Refs: `D5-69`, `D5-75`, `D5-76`

72. **Users report duplicate sections after suggest-outline. What checks should be tightened?**
Answer: Improve dedupe normalization (trim/case/synonyms) and add server-side unique title guard per project where appropriate. Current check is simple title match and may miss semantic duplicates.
Refs: `D5-37`, `D5-40`

## H) Testing and Engineering Maturity

73. **Which DocBuilder flow deserves end-to-end tests first and why?**
Answer: Highest value E2E is create project -> suggest outline -> generate -> refine -> export. It covers core user value and most backend modules.
Refs: `D5-37`, `D5-42`, `D5-79`

74. **How would you mock Groq, Google Search, and Firestore in CI?**
Answer: Use adapter abstraction for Groq mock, retriever fallback/mock for RAG, and dependency override/mocked Firestore client in tests. Keep deterministic fixtures for repeatability.
Refs: `D5-17`, `D5-29`, `D5-66`, `D5-68`

75. **How do you test refinement behavior with deterministic assertions?**
Answer: Run with MockLLM adapter for stable output, then assert response shape, version increment, and history append behavior. Add focused tests for instruction branches like expand/condense.
Refs: `D5-19`, `D5-43`, `D5-66`

76. **How would you create golden test fixtures for generated content quality?**
Answer: Store approved outputs for fixed prompts/topic/doc type and compare structure + quality metrics, not exact wording only. Version fixtures when prompt template changes intentionally.
Refs: `D5-5`, `D5-8`, `D5-4`

77. **What observability events would you emit for each generation/refinement/export request?**
Answer: Emit request id, user id, project id, stage timings, provider metadata, failure reason, and output size stats. For export include format/theme and file-size metrics.
Refs: `D5-40`, `D5-79`, `D5-80`

78. **How do you define SLOs for this platform as a fresher-friendly production app?**
Answer: Example SLOs: 99% API success for CRUD, 95% generate under 15s, 95% refine under 10s, export success >99%. Keep simpler but measurable objectives first.
Refs: `D5-72`, `D5-37`, `D5-42`

79. **What is your rollout strategy for risky prompt or retrieval changes?**
Answer: Use feature flags and canary rollout by percent/users, monitor quality/error metrics, then expand. Keep quick rollback and previous prompt version available.
Refs: `D5-17`, `D5-74`

80. **If you had 2 weeks to harden DocBuilder for internships/placements demo, what exact tasks would you prioritize?**
Answer: I would prioritize five things: remove mock-token from prod path, add queue for generate/refine, add pagination + doc-size-safe schema plan, tighten RAG source filtering, and expand E2E tests for core flow. This gives best reliability and interview confidence quickly.
Refs: `D5-46`, `D5-37`, `D5-54`, `D5-74`, `D5-66`
