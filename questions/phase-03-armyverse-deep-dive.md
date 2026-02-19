# Phase 03 - ArmyVerse Deep Dive Questions (Intermediate)

## A) Architecture Tradeoffs and Scale

1. If DAU grows from 1k to 100k, what breaks first in ArmyVerse?
2. Which module is your current scalability bottleneck and why?
3. How would you split ArmyVerse into microservices, if forced?
4. Which APIs should remain synchronous and which should become async jobs?
5. How would you design idempotency for reward-claim endpoints?
6. How would you avoid race conditions when awarding XP + inventory + leaderboard updates?
7. What transaction boundaries are needed in MongoDB for game rewards?
8. What eventual consistency issues can happen between quest progress and UI?
9. How would you redesign for multi-region low latency?
10. What observability signals would you add first (metrics/logs/traces)?

## B) Security Deep Dive

11. Explain AES-256-GCM internals: key, IV, auth tag, and tamper detection.
12. Why derive key material from env secret via SHA-256?
13. What are risks if `SPOTIFY_USER_SECRET_KEY` is weak or rotated incorrectly?
14. How would you design key rotation for encrypted BYO secrets?
15. What if an attacker gets DB dump but not app secrets?
16. What if attacker gets both DB dump and app env vars?
17. How is CSRF/state handled in Spotify callback flow?
18. How does signed state differ from PKCE and why use both in different flows?
19. What replay attacks are still possible and how to prevent them?
20. How would you harden token storage and refresh workflows?
21. Which headers/security policies would you enforce at platform level?
22. What are top 5 security risks in this codebase today?

## C) Spotify BYO + OAuth Internals

23. Walk through BYO flow: client credentials route -> callback -> token persistence.
24. Why allow optional client secret in BYO flow?
25. How does PKCE code verifier/challenge generation work exactly?
26. Why encrypt pending BYO state in DB before callback?
27. How do you detect and recover from invalid/expired user bearer token on export?
28. Why return `401` in user mode instead of silently falling back?
29. When is fallback to owner mode safe vs unsafe from product perspective?
30. How do you validate the acting Spotify account for export mode correctness?
31. How do you handle Spotify refresh-token rotation?
32. How would you support token revocation webhooks if Spotify offered them?
33. How do you test BYO flow without leaking real credentials?
34. What edge cases happen if redirect URI mismatches between app and Spotify dashboard?

## D) Last.fm, Rate Limiting, and Quest Verification

35. Explain your token bucket implementation and math.
36. Why choose 5 req/sec and what real API limit did you optimize for?
37. How does local in-memory rate limiter behave across multiple serverless instances?
38. What changes for distributed rate limiting in production?
39. How is Last.fm pagination handled for streaming verification?
40. How do you avoid false negatives in fuzzy track matching?
41. How do album quests verify "all tracks played" reliably?
42. What data quality issues can occur with Last.fm scrobbles?
43. How does caching window affect accuracy vs performance?
44. How would you detect suspicious quest spoofing patterns?

## E) Game Economy and Fairness

45. Resume mentions pity-based RNG; current docs mention random rarity pipeline. Explain the evolution.
46. Why keep pity fields in state if current reward rarity is random?
47. How do you ensure reward economy is not inflationary?
48. How did you choose dust values and mastery rewards?
49. What anti-abuse checks do you have for repeated reward triggers?
50. How do you audit inventory grants and detect anomalies?
51. How do you guarantee only one reward claim per completion event?
52. How would you simulate economy balance over 30 days of active users?
53. What telemetry would prove the game feels fair to users?
54. How do you prevent leaderboard manipulation by scripted clients?

## F) Data Pipelines and Cron Reliability

55. Explain your cron architecture for trending/quests and failure handling.
56. What happens if cron job misses one day?
57. How do you make cron runs idempotent?
58. How do you avoid duplicate quest generation for same period key?
59. What retry strategy would you use for flaky external APIs?
60. How would you implement dead-letter handling for repeated failures?
61. How do you monitor freshness of scraped data?
62. How do you maintain scraper robustness when HTML source changes?

## G) API Design and Reliability Scenarios

63. A user reports "reward claimed but card missing". How do you debug end-to-end?
64. A user reports "playlist exported to wrong account". How do you root-cause quickly?
65. Last.fm API is down for 2 hours. How should quest UX behave?
66. Mongo latency spikes suddenly. Which features degrade first?
67. One API endpoint starts returning 500 only in production. What is your triage checklist?
68. If an endpoint is hot and expensive, where do you cache first?
69. How do you version API contracts without breaking frontend pages?
70. How do you implement graceful degradation for external dependency failure?

## H) Testing and Quality Depth

71. Which ArmyVerse flows need integration tests but currently have unit tests only?
72. What would your highest-value end-to-end test case be?
73. How would you test auth matrix: JWT vs Firebase vs unauthenticated?
74. How would you test race conditions around reward claims?
75. How do you validate cryptography helpers safely in tests?
76. How do you load-test playlist export endpoints?
77. How do you prevent flaky tests for time-dependent quest logic?
78. Which invariants should never be violated in game state?
79. How do you use feature flags for risky changes?
80. What would your "definition of done" be for a production-critical ArmyVerse feature?
