# Phase 06 - Cross-Project and System Design Answers

This phase compares both projects and tests system-design thinking.
Answers are practical, interview-oriented, and easy to explain.

## Reference Legend

- `X1`: `Documents/ARMYVERSE/docs/architecture/overview.md:21`
- `X2`: `Documents/docbuilder/README.md:155`
- `X3`: `Documents/ARMYVERSE/lib/auth/verify.ts:76`
- `X4`: `Documents/docbuilder/backend/app/core/auth.py:23`
- `X5`: `Documents/ARMYVERSE/lib/utils/secrets.ts:15`
- `X6`: `Documents/docbuilder/backend/app/core/llm.py:77`
- `X7`: `Documents/ARMYVERSE/lib/game/rewards.ts:94`
- `X8`: `Documents/docbuilder/backend/app/models.py:31`
- `X9`: `Documents/ARMYVERSE/vercel.json:10`
- `X10`: `Documents/docbuilder/README.md:193`
- `X11`: `Documents/ARMYVERSE/lib/lastfm/client.ts:25`
- `X12`: `Documents/docbuilder/backend/app/core/rag.py:65`
- `X13`: `Documents/ARMYVERSE/lib/models/InventoryGrantAudit.ts:3`
- `X14`: `Documents/docbuilder/backend/app/models.py:33`
- `X15`: `Documents/ARMYVERSE/lib/models/LeaderboardEntry.ts:47`
- `X16`: `Documents/docbuilder/backend/app/api/endpoints.py:804`
- `X17`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:151`
- `X18`: `Documents/docbuilder/backend/main.py:32`
- `X19`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:80`
- `X20`: `Documents/docbuilder/backend/app/db/firestore.py:32`
- `X21`: `Documents/ARMYVERSE/lib/models/UserQuestProgress.ts:27`
- `X22`: `Documents/docbuilder/backend/app/api/endpoints.py:444`
- `X23`: `Documents/ARMYVERSE/lib/client/api.ts:22`
- `X24`: `Documents/docbuilder/frontend/src/pages/projects/[id].tsx:39`
- `X25`: `Documents/ARMYVERSE/test/lib/game/streamingVerification.test.ts:33`
- `X26`: `Documents/docbuilder/backend/tests/test_refinement.py:49`
- `X27`: `Documents/ARMYVERSE/package.json:15`
- `X28`: `Documents/docbuilder/backend/app/models.py:43`
- `X29`: `Documents/ARMYVERSE/lib/auth/gameHandoff.ts:22`
- `X30`: `Documents/docbuilder/backend/app/core/auth.py:20`
- `X31`: `Documents/ARMYVERSE/docs/features/spotify-per-user-export.md:14`
- `X32`: `Documents/docbuilder/RAG_FEATURE.md:211`
- `X33`: `Documents/ARMYVERSE/app/api/trending/homepage-cache/route.ts:129`
- `X34`: `Documents/docbuilder/backend/app/services/export_service.py:23`
- `X35`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:41`
- `X36`: `Documents/docbuilder/backend/app/api/endpoints.py:596`
- `X37`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:152`
- `X38`: `Documents/docbuilder/backend/app/core/rag.py:279`
- `X39`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:424`
- `X40`: `Documents/docbuilder/backend/app/core/llm.py:357`
- `X41`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:158`
- `X42`: `Documents/docbuilder/backend/app/api/endpoints.py:695`
- `X43`: `Documents/ARMYVERSE/app/api/game/leaderboard/route.ts:17`
- `X44`: `Documents/docbuilder/backend/app/api/endpoints.py:128`
- `X45`: `Documents/ARMYVERSE/lib/models/BoraRushRun.ts:19`
- `X46`: `Documents/docbuilder/backend/app/services/export_service.py:413`
- `X47`: `Documents/ARMYVERSE/lib/config.ts:2`
- `X48`: `Documents/docbuilder/backend/main.py:38`
- `X49`: `Documents/ARMYVERSE/lib/models/User.ts:203`
- `X50`: `Documents/docbuilder/backend/app/models.py:73`

## A) Comparing ArmyVerse vs DocBuilder

1. **Compare the architectural patterns of ArmyVerse and DocBuilder.**
Answer: ArmyVerse is Next.js full-stack with route handlers and product modules inside one app. DocBuilder is more service-split: FastAPI backend + Next.js frontend. Both are monorepo-like, but DocBuilder has clearer backend boundary.
Refs: `X1`, `X2`

2. **Why is ArmyVerse Next.js-centric while DocBuilder is API-service-centric?**
Answer: ArmyVerse optimizes feature velocity in one runtime for community/product features. DocBuilder has AI orchestration and export-heavy backend logic, so dedicated API service is practical.
Refs: `X1`, `X6`, `X16`

3. **Which project demonstrates stronger backend engineering and why?**
Answer: ArmyVerse shows stronger transactional/stateful backend complexity because of game economy, quests, anti-abuse, and leaderboard consistency. DocBuilder backend is solid but less state-heavy compared to game mechanics.
Refs: `X7`, `X17`, `X21`, `X43`

4. **Which project demonstrates stronger AI engineering and why?**
Answer: DocBuilder is stronger in AI engineering because it has adapter-based LLM design, structured parsing, RAG retrieval pipeline, and refinement context logic. ArmyVerse uses AI mainly for specific playlist use cases.
Refs: `X6`, `X12`, `X38`, `X40`

5. **Which project had harder state management challenges?**
Answer: ArmyVerse, because it manages progression, claims, rewards, streaks, and leaderboards with anti-duplication guarantees. DocBuilder state is complex too, but mostly document editing lifecycle.
Refs: `X7`, `X13`, `X15`, `X8`

6. **Which project has higher external dependency risk?**
Answer: Both are dependency-heavy, but DocBuilder RAG path depends on search + fetch + embedding + LLM chain in one request. ArmyVerse also depends on Spotify/Last.fm, but many features still work without all external APIs.
Refs: `X11`, `X12`, `X32`, `X37`

7. **How do auth strategies differ between these two projects?**
Answer: ArmyVerse supports dual auth paths (Firebase + JWT fallback), while DocBuilder is mainly Firebase token verification in backend dependency. DocBuilder currently has a mock token path that must be restricted to non-prod.
Refs: `X3`, `X4`, `X30`

8. **What common reusable modules could be extracted across both projects?**
Answer: Common modules could include auth middleware patterns, API error envelope, audit logging utilities, rate limiting wrappers, and feature-flag helpers. Both projects already have repeated route-guard patterns.
Refs: `X3`, `X4`, `X23`, `X24`

9. **Which project is easier to scale to 10x traffic and why?**
Answer: ArmyVerse read-heavy pages may scale easier initially with CDN/serverless, but write-heavy game paths need hardening. DocBuilder can scale well after async queue extraction for generate/refine/export.
Refs: `X9`, `X39`, `X32`, `X16`

10. **Which project has higher correctness sensitivity and why?**
Answer: ArmyVerse has higher correctness sensitivity because reward/claim/leaderboard bugs directly affect fairness and trust. DocBuilder correctness is important, but errors are usually reversible content issues.
Refs: `X35`, `X15`, `X45`, `X42`

## B) End-to-End System Design Scenarios

11. **Design a common notification system usable by both projects.**
Answer: Build shared `notifications` service with event producers from both apps and channel adapters (in-app/email/push). Use event schema with `userId`, `type`, `payload`, `priority`, and dedupe key.
Refs: `X13`, `X14`

12. **Design centralized audit logging for both products.**
Answer: Emit structured audit events from critical endpoints into one append-only store. Include actor, action, resource, result, and trace id for forensics.
Refs: `X13`, `X36`, `X42`

13. **Design a common rate-limiter service for external API calls.**
Answer: Use Redis-based distributed token bucket keyed by provider and tenant/user. Both ArmyVerse Last.fm/Spotify and DocBuilder RAG fetch/search should pass through it.
Refs: `X11`, `X12`, `X41`

14. **Design a shared observability dashboard for error/latency metrics.**
Answer: Collect request metrics per endpoint with p50/p95 latency, error class, and dependency failure rates. Add separate boards for user-path SLOs and external API health.
Refs: `X23`, `X24`, `X32`

15. **Design feature-flag rollout infrastructure for safe deployments.**
Answer: Use centralized flag service with environment overrides and percentage rollout. Start internal, then canary, then full release with rollback toggle.
Refs: `X37`, `X40`

16. **Design a shared secrets management approach for local/dev/prod.**
Answer: Keep local `.env`, but use cloud secret manager for dev/prod and short-lived access credentials. Add rotation schedule and secret versioning.
Refs: `X19`, `X47`, `X48`

17. **Design a unified identity provider strategy replacing mixed auth paths.**
Answer: Adopt one primary IdP and issue signed service tokens with clear claim contract for both apps. Keep compatibility bridge during migration.
Refs: `X3`, `X4`, `X49`

18. **Design multi-tenant support for both products.**
Answer: Add `tenantId` to every resource, partition indexes by tenant, and enforce tenant isolation in middleware. Rate limits and quotas should also be tenant-scoped.
Refs: `X15`, `X28`, `X44`

19. **Design a backup and disaster recovery plan for both systems.**
Answer: Schedule database snapshots, export restore drills, and keep RPO/RTO targets. Store critical audit logs separately and verify rollback automation monthly.
Refs: `X20`, `X33`

20. **Design a cross-project API gateway with auth and quota controls.**
Answer: Put gateway in front with JWT verification, route mapping, rate limits, and per-client quota policies. Add standardized error envelope and request tracing headers.
Refs: `X23`, `X18`

## C) Reliability and Operations

21. **How would you define SLIs/SLOs for each product?**
Answer: ArmyVerse: quest claim success, leaderboard freshness, export success mode accuracy. DocBuilder: generate/refine latency, export success, and RAG relevance success. Set practical SLOs first, tighten later.
Refs: `X35`, `X43`, `X32`, `X16`

22. **What should be your on-call runbook for critical incidents?**
Answer: Include severity classification, quick health checks, dependency checks, rollback actions, communication template, and postmortem checklist. Keep command snippets per module.
Refs: `X9`, `X10`

23. **How do you classify P0 vs P1 vs P2 incidents in these apps?**
Answer: P0: auth bypass, data corruption, reward duplication, major outage. P1: core workflow broken for many users. P2: partial degradation or non-critical feature issues.
Refs: `X30`, `X35`, `X45`

24. **How do you reduce MTTR when external APIs fail?**
Answer: Detect failure fast, degrade gracefully, and surface explicit user-safe fallback states. Keep runbooks for token refresh, search fallback, and retry logic.
Refs: `X41`, `X37`, `X73`

25. **What health checks should each service expose?**
Answer: Basic process health, DB connectivity, auth verification sanity, and dependency heartbeat summaries. Avoid deep expensive checks in health endpoint.
Refs: `X48`, `X20`

26. **How would you detect silent data corruption?**
Answer: Use invariant checks (unique keys, monotonic counters, hash comparisons) and periodic audit jobs. Alert on violations like duplicate claims or impossible balance states.
Refs: `X21`, `X15`, `X36`

27. **How do you plan rollback strategy for bad deployments?**
Answer: Keep immutable build artifacts, feature flags, and one-command rollback path. Prefer backward-compatible DB changes before app rollout.
Refs: `X9`, `X10`

28. **How do you implement circuit breakers for flaky dependencies?**
Answer: Track failure ratio and open circuit after threshold, then return cached/degraded response quickly. Retry only after cool-down window.
Refs: `X11`, `X12`, `X32`

29. **How do you implement graceful degradation in user-facing UI?**
Answer: Show mode/status explicitly, keep partial functionality alive, and allow retry when dependency recovers. Never hide ownership/security-impacting fallback behavior.
Refs: `X31`, `X37`, `X24`

30. **How do you protect against thundering herd on cache misses?**
Answer: Use request coalescing, single-flight locks, and stale-while-revalidate strategies. Add jitter to TTL expiry.
Refs: `X33`, `X38`

## D) Data and Caching Strategy

31. **Where should caching be applied in ArmyVerse and DocBuilder respectively?**
Answer: ArmyVerse: leaderboard reads, trending snapshots, status checks. DocBuilder: repeated generation prompts, retrieval results, and read-heavy project views.
Refs: `X33`, `X43`, `X82`, `X24`

32. **What data deserves strong consistency vs eventual consistency?**
Answer: Strong consistency for claims, balances, inventory, and auth-critical data. Eventual consistency is fine for rankings, analytics, and cached trend views.
Refs: `X7`, `X21`, `X15`, `X33`

33. **How would you choose Redis vs DB caching per use case?**
Answer: Use Redis for short-lived hot keys and cross-instance coordination. Use DB materialized/cache collections for durable snapshots.
Refs: `X33`, `X20`

34. **How would you design cache invalidation for generated content updates?**
Answer: Invalidate by project/section key on save/refine/generate completion and bump version-based cache keys. Keep tiny TTL safety net.
Refs: `X8`, `X42`, `X36`

35. **How would you design deduplication for repeated generation requests?**
Answer: Add request fingerprint (projectId+sectionId+promptHash+version) and idempotency store. Return existing result if same in-flight/completed key exists.
Refs: `X36`, `X8`

36. **How would you prevent stale leaderboard or stale project views?**
Answer: Use refresh triggers after write operations and include updated timestamps/versions in responses. Client should re-fetch on important mutations.
Refs: `X43`, `X24`, `X42`

37. **How would you shard data if user base grows massively?**
Answer: Partition by tenant/user hash for high-cardinality collections and keep hot counters isolated. Read models like leaderboard can be period-sharded.
Refs: `X15`, `X28`

38. **How would you manage archival of old project/game data?**
Answer: Move inactive records to cold storage with restore path. Keep only operationally relevant subset in hot database.
Refs: `X14`, `X45`

## E) Security Across Systems

39. **What is your threat model for both products?**
Answer: Main threats: auth bypass, token theft, resource-ownership bypass, prompt injection/data poisoning, and abuse automation. Threat model must cover API, DB, third-party integrations, and frontend rendering.
Refs: `X3`, `X30`, `X12`, `X41`

40. **Which PII/security-sensitive data exists in each project?**
Answer: ArmyVerse stores user identifiers, auth links, and encrypted Spotify BYO secrets. DocBuilder stores user identity plus generated/refined document content that may contain confidential business info.
Refs: `X49`, `X5`, `X50`

41. **How do you apply least privilege across APIs and database roles?**
Answer: Enforce ownership checks in all resource routes and use scoped service credentials with minimal permissions. Separate read/write permissions where possible.
Refs: `X35`, `X50`, `X20`

42. **How do you secure third-party tokens and credentials at rest and in transit?**
Answer: Encrypt sensitive credentials at rest and always transmit through HTTPS/TLS. Keep secrets in env manager and rotate periodically.
Refs: `X5`, `X19`, `X48`

43. **How do you do secure logging without leaking secrets?**
Answer: Log metadata, not raw tokens/content secrets. Add redaction middleware for auth headers and sensitive fields.
Refs: `X23`, `X48`

44. **How do you implement abuse detection and anti-automation protections?**
Answer: Combine rate limits, idempotency keys, anomaly checks, and auth-token integrity verification. Track repeated suspicious patterns per user/IP/device.
Refs: `X17`, `X29`, `X45`

45. **How do you enforce secure dependency and vulnerability scanning?**
Answer: Run dependency scanners in CI for JS and Python and block known critical CVEs. Keep lockfiles updated and patch on schedule.
Refs: `X27`, `X10`

46. **How would you pass a basic security audit with these projects?**
Answer: Remove prod mock-token paths, document data flows, prove encryption practices, demonstrate authz checks, and add audit logs/tests for critical actions.
Refs: `X30`, `X5`, `X35`, `X26`

## F) Engineering Process and Teamwork

47. **How do you break these products into tickets for a small team?**
Answer: Split by vertical slice: API change, UI change, data model, tests, observability, docs. Keep tickets small and independently shippable.
Refs: `X24`, `X16`

48. **How do you estimate and prioritize technical debt?**
Answer: Prioritize debt by user impact, incident risk, and development drag. Security/auth and consistency debt come first.
Refs: `X30`, `X35`, `X42`

49. **What code review checklist would you enforce?**
Answer: Checklist: authz checks, input validation, idempotency, error handling, test coverage, logging quality, and docs update. Also verify backward compatibility.
Refs: `X3`, `X22`, `X26`, `X25`

50. **How do you ensure docs stay updated when code changes?**
Answer: Make doc update part of PR definition of done and include doc link in PR template. Reject major behavior changes without doc diff.
Refs: `X1`, `X2`, `X10`

51. **How would you set up branch strategy and release cadence?**
Answer: Use trunk + short-lived feature branches, automated checks, and scheduled release windows. For risky changes, use feature flags and phased rollout.
Refs: `X27`, `X10`

52. **How do you decide when to refactor vs patch quickly?**
Answer: If issue is critical and isolated, patch quickly with rollback safety. If root cause is structural and recurring, schedule refactor with tests.
Refs: `X42`, `X43`

53. **How do you communicate tradeoffs to non-technical stakeholders?**
Answer: Explain options using impact, risk, cost, and timeline, not internal jargon. Show what users gain or lose with each option.
Refs: `X31`, `X32`

54. **How do you mentor a junior teammate on these codebases?**
Answer: Start with domain map, request flow walkthrough, and one small end-to-end ticket. Pair on debugging and enforce test-first critical changes.
Refs: `X1`, `X2`, `X25`, `X26`

## G) Placement/Interview-Style Open Design Prompts

55. **Design a "generation credit" billing model for DocBuilder.**
Answer: Add `credits` ledger with event types (`generate`, `refine`, `export`) and per-action cost. Deduct at request start with idempotency key and refund on hard failure.
Refs: `X36`, `X40`, `X16`

56. **Design a robust anti-cheat framework for ArmyVerse quests and leaderboards.**
Answer: Use signed event tokens, per-user caps, behavioral anomaly scoring, and audit trails. Separate “verified score” from “pending score” until checks pass.
Refs: `X29`, `X17`, `X13`, `X15`

57. **Design offline-capable draft editing for DocBuilder frontend.**
Answer: Store local drafts per section in IndexedDB with version metadata and sync queue. On reconnect, push changes with optimistic concurrency and conflict resolution UI.
Refs: `X8`, `X24`, `X42`

58. **Design an experimentation system for ArmyVerse recommendation quality.**
Answer: Add experiment assignment service and log exposure/outcome events by variant. Measure retention, playlist export success, and satisfaction signals.
Refs: `X41`, `X37`

59. **Design a moderation pipeline for blog and comment content.**
Answer: Add pre-save moderation checks, risk scoring, and asynchronous review queue for flagged content. Keep appeal and audit trail flows.
Refs: `X1`, `X13`

60. **Design a production-grade queue architecture for async generation and export.**
Answer: API accepts request and enqueues job, worker handles heavy generation/export, result store tracks status and artifact link, frontend polls/subscribes. Add retry policy, DLQ, and idempotency.
Refs: `X16`, `X46`, `X36`
