# Phase 06 - Cross-Project and System Design Questions

## A) Comparing ArmyVerse vs DocBuilder

1. Compare the architectural patterns of ArmyVerse and DocBuilder.
2. Why is ArmyVerse Next.js-centric while DocBuilder is API-service-centric?
3. Which project demonstrates stronger backend engineering and why?
4. Which project demonstrates stronger AI engineering and why?
5. Which project had harder state management challenges?
6. Which project has higher external dependency risk?
7. How do auth strategies differ between these two projects?
8. What common reusable modules could be extracted across both projects?
9. Which project is easier to scale to 10x traffic and why?
10. Which project has higher correctness sensitivity and why?

## B) End-to-End System Design Scenarios

11. Design a common notification system usable by both projects.
12. Design centralized audit logging for both products.
13. Design a common rate-limiter service for external API calls.
14. Design a shared observability dashboard for error/latency metrics.
15. Design feature-flag rollout infrastructure for safe deployments.
16. Design a shared secrets management approach for local/dev/prod.
17. Design a unified identity provider strategy replacing mixed auth paths.
18. Design multi-tenant support for both products.
19. Design a backup and disaster recovery plan for both systems.
20. Design a cross-project API gateway with auth and quota controls.

## C) Reliability and Operations

21. How would you define SLIs/SLOs for each product?
22. What should be your on-call runbook for critical incidents?
23. How do you classify P0 vs P1 vs P2 incidents in these apps?
24. How do you reduce MTTR when external APIs fail?
25. What health checks should each service expose?
26. How would you detect silent data corruption?
27. How do you plan rollback strategy for bad deployments?
28. How do you implement circuit breakers for flaky dependencies?
29. How do you implement graceful degradation in user-facing UI?
30. How do you protect against thundering herd on cache misses?

## D) Data and Caching Strategy

31. Where should caching be applied in ArmyVerse and DocBuilder respectively?
32. What data deserves strong consistency vs eventual consistency?
33. How would you choose Redis vs DB caching per use case?
34. How would you design cache invalidation for generated content updates?
35. How would you design deduplication for repeated generation requests?
36. How would you prevent stale leaderboard or stale project views?
37. How would you shard data if user base grows massively?
38. How would you manage archival of old project/game data?

## E) Security Across Systems

39. What is your threat model for both products?
40. Which PII/security-sensitive data exists in each project?
41. How do you apply least privilege across APIs and database roles?
42. How do you secure third-party tokens and credentials at rest and in transit?
43. How do you do secure logging without leaking secrets?
44. How do you implement abuse detection and anti-automation protections?
45. How do you enforce secure dependency and vulnerability scanning?
46. How would you pass a basic security audit with these projects?

## F) Engineering Process and Teamwork

47. How do you break these products into tickets for a small team?
48. How do you estimate and prioritize technical debt?
49. What code review checklist would you enforce?
50. How do you ensure docs stay updated when code changes?
51. How would you set up branch strategy and release cadence?
52. How do you decide when to refactor vs patch quickly?
53. How do you communicate tradeoffs to non-technical stakeholders?
54. How do you mentor a junior teammate on these codebases?

## G) Placement/Interview-Style Open Design Prompts

55. Design a "generation credit" billing model for DocBuilder.
56. Design a robust anti-cheat framework for ArmyVerse quests and leaderboards.
57. Design offline-capable draft editing for DocBuilder frontend.
58. Design an experimentation system for ArmyVerse recommendation quality.
59. Design a moderation pipeline for blog and comment content.
60. Design a production-grade queue architecture for async generation and export.
