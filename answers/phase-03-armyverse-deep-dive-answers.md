# Phase 03 - ArmyVerse Deep Dive Answers

This phase is for deep technical rounds.
Answers are detailed but kept in simple interview language.

## Reference Legend

- `P3-1`: `Documents/ARMYVERSE/docs/architecture/overview.md:21`
- `P3-2`: `Documents/ARMYVERSE/vercel.json:4`
- `P3-3`: `Documents/ARMYVERSE/app/api/game/quiz/complete/route.ts:82`
- `P3-4`: `Documents/ARMYVERSE/app/api/game/quests/verify-streaming/route.ts:40`
- `P3-5`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:236`
- `P3-6`: `Documents/ARMYVERSE/lib/game/rewards.ts:66`
- `P3-7`: `Documents/ARMYVERSE/lib/models/UserQuestProgress.ts:27`
- `P3-8`: `Documents/ARMYVERSE/lib/models/MasteryLevelRewardLedger.ts:12`
- `P3-9`: `Documents/ARMYVERSE/lib/utils/secrets.ts:8`
- `P3-10`: `Documents/ARMYVERSE/lib/utils/secrets.ts:15`
- `P3-11`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:60`
- `P3-12`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:36`
- `P3-13`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:127`
- `P3-14`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:140`
- `P3-15`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:148`
- `P3-16`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:176`
- `P3-17`: `Documents/ARMYVERSE/lib/spotify/userTokens.ts:190`
- `P3-18`: `Documents/ARMYVERSE/lib/lastfm/client.ts:25`
- `P3-19`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:25`
- `P3-20`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:198`
- `P3-21`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:36`
- `P3-22`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:358`
- `P3-23`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:168`
- `P3-24`: `Documents/ARMYVERSE/lib/game/quests.ts:24`
- `P3-25`: `Documents/ARMYVERSE/lib/game/streamingQuestSelection.ts:74`
- `P3-26`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:41`
- `P3-27`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:80`
- `P3-28`: `Documents/ARMYVERSE/lib/game/dropTable.ts:37`
- `P3-29`: `Documents/ARMYVERSE/lib/models/UserGameState.ts:41`
- `P3-30`: `Documents/ARMYVERSE/lib/game/rewards.ts:31`
- `P3-31`: `Documents/ARMYVERSE/lib/models/InventoryGrantAudit.ts:3`
- `P3-32`: `Documents/ARMYVERSE/app/api/game/quiz/complete/route.ts:243`
- `P3-33`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:116`
- `P3-34`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:151`
- `P3-35`: `Documents/ARMYVERSE/lib/models/BoraRushRun.ts:19`
- `P3-36`: `Documents/ARMYVERSE/lib/models/BoraRushDailyLimit.ts:22`
- `P3-37`: `Documents/ARMYVERSE/app/api/game/leaderboard/route.ts:47`
- `P3-38`: `Documents/ARMYVERSE/app/api/game/leaderboard/route.ts:72`
- `P3-39`: `Documents/ARMYVERSE/app/api/cron/daily-quests/route.ts:20`
- `P3-40`: `Documents/ARMYVERSE/app/api/cron/weekly-quests/route.ts:19`
- `P3-41`: `Documents/ARMYVERSE/lib/game/streamingQuestSelection.ts:279`
- `P3-42`: `Documents/ARMYVERSE/lib/game/quizQuestGeneration.ts:9`
- `P3-43`: `Documents/ARMYVERSE/app/api/trending/homepage-cache/cron/route.ts:13`
- `P3-44`: `Documents/ARMYVERSE/app/api/trending/homepage-cache/route.ts:45`
- `P3-45`: `Documents/ARMYVERSE/vercel.json:12`
- `P3-46`: `Documents/ARMYVERSE/app/api/game/quests/route.ts:72`
- `P3-47`: `Documents/ARMYVERSE/app/api/game/quests/route.ts:116`
- `P3-48`: `Documents/ARMYVERSE/lib/client/api.ts:22`
- `P3-49`: `Documents/ARMYVERSE/hooks/useSpotifyAuth.ts:23`
- `P3-50`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:212`
- `P3-51`: `Documents/ARMYVERSE/lib/auth/gameHandoff.ts:22`
- `P3-52`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:245`
- `P3-53`: `Documents/ARMYVERSE/lib/game/mastery.ts:145`
- `P3-54`: `Documents/ARMYVERSE/test/lib/game/streamingVerification.test.ts:33`
- `P3-55`: `Documents/ARMYVERSE/test/lib/game/mastery.test.ts:25`
- `P3-56`: `Documents/ARMYVERSE/test/lib/game/borarushCaps.test.ts:3`
- `P3-57`: `Documents/ARMYVERSE/package.json:15`
- `P3-58`: `Documents/ARMYVERSE/docs/features/spotify-per-user-export.md:14`
- `P3-59`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:144`
- `P3-60`: `Documents/ARMYVERSE/lib/spotify/userTokens.ts:23`
- `P3-61`: `Documents/ARMYVERSE/lib/game/streamingQuestSelection.ts:201`
- `P3-62`: `Documents/ARMYVERSE/app/api/game/quiz/start/route.ts:49`
- `P3-63`: `Documents/ARMYVERSE/app/api/game/quiz/complete/route.ts:57`
- `P3-64`: `Documents/ARMYVERSE/app/api/game/quiz/complete/route.ts:116`
- `P3-65`: `Documents/ARMYVERSE/app/api/game/quiz/complete/route.ts:169`
- `P3-66`: `Documents/ARMYVERSE/lib/models/LeaderboardEntry.ts:47`
- `P3-67`: `Documents/ARMYVERSE/docs/features/game-system.md:71`
- `P3-68`: `Documents/ARMYVERSE/lib/game/dropTable.ts:55`
- `P3-69`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:148`
- `P3-70`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:304`
- `P3-71`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:158`
- `P3-72`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:97`
- `P3-73`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:49`
- `P3-74`: `Documents/ARMYVERSE/lib/utils/secrets.ts:25`
- `P3-75`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:110`
- `P3-76`: `Documents/ARMYVERSE/lib/game/quests.ts:53`
- `P3-77`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:349`
- `P3-78`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:46`
- `P3-79`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:67`
- `P3-80`: `Documents/ARMYVERSE/app/api/game/quests/claim/route.ts:84`
- `P3-81`: `Documents/ARMYVERSE/app/api/game/leaderboard/route.ts:17`
- `P3-82`: `Documents/ARMYVERSE/app/api/game/leaderboard/route.ts:143`
- `P3-83`: `Documents/ARMYVERSE/app/api/game/quests/verify-streaming/route.ts:30`
- `P3-84`: `Documents/ARMYVERSE/lib/lastfm/client.ts:91`
- `P3-85`: `Documents/ARMYVERSE/lib/lastfm/client.ts:95`
- `P3-86`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:215`
- `P3-87`: `Documents/ARMYVERSE/lib/game/streamingVerification.ts:424`
- `P3-88`: `Documents/ARMYVERSE/lib/game/streamingQuestSelection.ts:225`
- `P3-89`: `Documents/ARMYVERSE/lib/game/streamingQuestSelection.ts:320`
- `P3-90`: `Documents/ARMYVERSE/app/api/trending/homepage-cache/route.ts:136`
- `P3-91`: `Documents/ARMYVERSE/app/api/trending/homepage-cache/route.ts:129`
- `P3-92`: `Documents/ARMYVERSE/docs/architecture/overview.md:286`
- `P3-93`: `Documents/ARMYVERSE/docs/architecture/overview.md:291`
- `P3-94`: `Documents/ARMYVERSE/lib/auth/jwt.ts:10`
- `P3-95`: `Documents/ARMYVERSE/lib/auth/verify.ts:76`
- `P3-96`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:117`
- `P3-97`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:39`
- `P3-98`: `Documents/ARMYVERSE/lib/models/User.ts:203`
- `P3-99`: `Documents/ARMYVERSE/lib/models/User.ts:228`
- `P3-100`: `Documents/ARMYVERSE/lib/models/User.ts:315`
- `P3-101`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:80`
- `P3-102`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:95`
- `P3-103`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:152`
- `P3-104`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:169`
- `P3-105`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:184`
- `P3-106`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:211`
- `P3-107`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:286`
- `P3-108`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:266`
- `P3-109`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:25`
- `P3-110`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:178`
- `P3-111`: `Documents/ARMYVERSE/app/api/game/borarush/complete/route.ts:201`

## A) Architecture Tradeoffs and Scale

1. **If DAU grows from 1k to 100k, what breaks first in ArmyVerse?**
Answer: First pain will come from database-heavy endpoints and external APIs, not static frontend pages. Quest verification, playlist export, and leaderboard/rank computation will become hot because they do more writes and external calls per request. Serverless time limits can also hit routes that chain many operations.
Refs: `P3-1`, `P3-2`, `P3-4`, `P3-37`

2. **Which module is your current scalability bottleneck and why?**
Answer: Streaming quest verification is a likely bottleneck. It does pagination, normalization, and matching across many tracks and can run for daily and weekly paths together. At high traffic, this becomes expensive in both CPU and external API usage.
Refs: `P3-19`, `P3-20`, `P3-87`

3. **How would you split ArmyVerse into microservices, if forced?**
Answer: I would split by bounded context: Auth, Game, Playlist/Spotify, and Content/Blog/Trending. Each service would own its own write models and expose APIs/events. I would keep ranking and quest generation async to reduce coupling.
Refs: `P3-1`, `P3-24`, `P3-39`, `P3-40`

4. **Which APIs should remain synchronous and which should become async jobs?**
Answer: Keep user-facing actions synchronous when users expect immediate feedback: login, quiz start/complete, and export request acceptance. Move heavy compute to async: rank recomputation, scraping, and bulk quest regeneration. Return job status or cached result for async flows.
Refs: `P3-62`, `P3-64`, `P3-81`, `P3-44`

5. **How would you design idempotency for reward-claim endpoints?**
Answer: Use unique business keys and atomic upserts. For quests, unique `(userId, code, periodKey)` plus `claimed` check protects repeated claims. For mastery, unique ledger key `(userId, kind, key, level)` prevents duplicate reward for same milestone.
Refs: `P3-7`, `P3-26`, `P3-8`, `P3-53`

6. **How would you avoid race conditions when awarding XP + inventory + leaderboard updates?**
Answer: Keep each write atomic where possible (`$inc`, `upsert`), and make each operation idempotent. Use unique indexes for “one-time” entities and treat retries as safe no-op. If you need strict all-or-nothing, wrap critical grouped writes in DB transaction/session.
Refs: `P3-6`, `P3-66`, `P3-7`, `P3-8`

7. **What transaction boundaries are needed in MongoDB for game rewards?**
Answer: Minimum boundary is “award event” record + user balance/item update, so reward cannot half-apply. If no multi-document transaction is used, add compensating logic and idempotency keys. For high-value rewards, transaction is preferred.
Refs: `P3-6`, `P3-31`, `P3-32`

8. **What eventual consistency issues can happen between quest progress and UI?**
Answer: UI can show stale quest progress right after verify/claim because APIs and refresh calls are separate. Also rank and progress can lag if background update is delayed or request retries. This is normal eventual consistency and needs clear UX messaging.
Refs: `P3-46`, `P3-47`, `P3-81`

9. **How would you redesign for multi-region low latency?**
Answer: Separate read and write paths: read from regional replicas/cache, write to a primary with event fan-out. Put heavy external integrations behind regional workers and queue. Keep auth/session checks globally available with short-lived tokens.
Refs: `P3-1`, `P3-92`, `P3-93`, `P3-94`

10. **What observability signals would you add first (metrics/logs/traces)?**
Answer: First: p95 latency and error rate per endpoint, plus external API failure ratio. Then add token-refresh failures, fallback mode usage, and quest verification duration/page count. Finally trace request IDs end-to-end for debug speed.
Refs: `P3-15`, `P3-16`, `P3-20`, `P3-70`

## B) Security Deep Dive

11. **Explain AES-256-GCM internals: key, IV, auth tag, and tamper detection.**
Answer: AES-256-GCM uses a 256-bit key for encryption plus integrity in one mode. A random IV (nonce) is generated per encryption, and an auth tag is produced. During decrypt, if ciphertext/IV/tag is modified, auth check fails and decryption throws.
Refs: `P3-10`, `P3-74`

12. **Why derive key material from env secret via SHA-256?**
Answer: The env string can be any length, but AES-256 needs a fixed 32-byte key. Hashing with SHA-256 gives a consistent 32-byte output and avoids key-size mismatch issues. It is a deterministic key-derivation shortcut.
Refs: `P3-9`

13. **What are risks if `SPOTIFY_USER_SECRET_KEY` is weak or rotated incorrectly?**
Answer: Weak key means encrypted BYO credentials become easier to brute-force offline. Bad rotation can make existing encrypted values undecryptable, breaking BYO login/export for all users. Rotation must support old+new decrypt during migration.
Refs: `P3-9`, `P3-98`, `P3-99`

14. **How would you design key rotation for encrypted BYO secrets?**
Answer: Add key version metadata to encrypted blobs. On read, try key by version; on successful decrypt, re-encrypt with latest key and update record. Keep dual-key support until migration completion.
Refs: `P3-98`, `P3-99`, `P3-102`

15. **What if an attacker gets DB dump but not app secrets?**
Answer: BYO client secret and refresh token stay protected because fields are encrypted at rest. Attacker still gets metadata and unencrypted non-secret fields, so privacy impact exists but credential takeover is harder. This is why encryption-at-rest in app layer helps.
Refs: `P3-10`, `P3-98`, `P3-99`

16. **What if attacker gets both DB dump and app env vars?**
Answer: Then app-layer encryption is compromised and BYO secrets can be decrypted. At that point incident response must revoke/rotate keys and force reconnection. Defense should include secret manager, strict access control, and audit alerts.
Refs: `P3-9`, `P3-98`, `P3-99`

17. **How is CSRF/state handled in Spotify callback flow?**
Answer: Two state paths exist. Standard flow uses signed state and timing-safe signature comparison. BYO flow uses server-side pending state lookup; callback only succeeds when state matches pending record.
Refs: `P3-13`, `P3-96`, `P3-97`

18. **How does signed state differ from PKCE and why use both in different flows?**
Answer: Signed state mainly protects request integrity and CSRF-like mix-up. PKCE protects authorization code exchange especially for public clients without client secret. BYO without secret uses PKCE, while standard confidential flow relies on client secret + signed state.
Refs: `P3-11`, `P3-12`, `P3-13`

19. **What replay attacks are still possible and how to prevent them?**
Answer: Reusing old callback state is a replay risk if state lifetime is too long. Standard flow already checks max age; BYO should also enforce pending-state expiry and one-time usage cleanup. Nonces and strict TTL reduce replay window.
Refs: `P3-14`, `P3-75`, `P3-99`

20. **How would you harden token storage and refresh workflows?**
Answer: Encrypt all long-lived refresh tokens, keep access tokens short-lived, and rotate refresh tokens when provider returns new one. Add failure counters and “needsReauth” states instead of silent fallback. Log suspicious refresh patterns.
Refs: `P3-17`, `P3-60`, `P3-103`

21. **Which headers/security policies would you enforce at platform level?**
Answer: I would enforce CSP, HSTS, X-Content-Type-Options, Referrer-Policy, and strict CORS allowlists. For APIs, rate-limit sensitive endpoints and sanitize all rich content. Also remove secret-like values from logs.
Refs: `P3-92`, `P3-93`, `P3-47`

22. **What are top 5 security risks in this codebase today?**
Answer: Main risks: environment-secret leakage, BYO key rotation mistakes, replay edge cases in callback/pending state, fallback misuse causing wrong-account export, and automation abuse on rewards. Most are manageable with stronger guardrails and monitoring.
Refs: `P3-9`, `P3-58`, `P3-97`, `P3-33`, `P3-34`

## C) Spotify BYO + OAuth Internals

23. **Walk through BYO flow: client credentials route -> callback -> token persistence.**
Answer: User sends clientId(+optional secret) to BYO route, server stores encrypted pending data and optional PKCE verifier, then returns authorize URL. Callback finds pending state, exchanges code, fetches profile, stores encrypted refresh token + metadata in `integrations.spotifyByo`, and clears pending state.
Refs: `P3-11`, `P3-12`, `P3-101`, `P3-102`

24. **Why allow optional client secret in BYO flow?**
Answer: Some Spotify apps are public/mobile style and cannot safely use client secret in client path. So flow supports both confidential (with secret) and PKCE (without secret). This improves compatibility without blocking users.
Refs: `P3-11`, `P3-72`

25. **How does PKCE code verifier/challenge generation work exactly?**
Answer: Server creates random verifier bytes, base64url encodes them, hashes verifier with SHA-256, then sends hash as `code_challenge` (S256). Later callback sends stored verifier to token endpoint. If verifier mismatch, exchange fails.
Refs: `P3-11`, `P3-72`

26. **Why encrypt pending BYO state in DB before callback?**
Answer: Pending object contains sensitive app credentials and PKCE verifier. Even though it is temporary, storing plaintext is risky. Encryption protects those values if DB is exposed.
Refs: `P3-101`, `P3-10`, `P3-99`

27. **How do you detect and recover from invalid/expired user bearer token on export?**
Answer: Export route validates bearer via Spotify `/me`. If invalid, route returns `401` with token-expired code so client can refresh and retry. Client status hook fetches fresh token and retries export flow.
Refs: `P3-69`, `P3-16`, `P3-49`, `P3-58`

28. **Why return `401` in user mode instead of silently falling back?**
Answer: Silent fallback can create playlist in wrong account, which is product and trust issue. Returning `401` gives client a chance to refresh user token first. Fallback should be explicit user choice.
Refs: `P3-16`, `P3-58`

29. **When is fallback to owner mode safe vs unsafe from product perspective?**
Answer: Safe when user knows they are exporting to ArmyVerse shared owner account and accepts it. Unsafe when user expects personal-account ownership. So UI should clearly show mode and warn on fallback use.
Refs: `P3-58`, `P3-70`

30. **How do you validate the acting Spotify account for export mode correctness?**
Answer: In user mode, API calls `/v1/me` with bearer and uses returned user id as target user id. In owner mode, it refreshes owner token and resolves owner id from env or `/me`. This makes target account explicit and verifiable.
Refs: `P3-69`, `P3-71`

31. **How do you handle Spotify refresh-token rotation?**
Answer: On refresh response, if Spotify sends new `refresh_token`, code stores the new encrypted token; else keeps existing token. This supports rolling refresh-token rotation without breaking user sessions.
Refs: `P3-17`, `P3-60`

32. **How would you support token revocation webhooks if Spotify offered them?**
Answer: I would add webhook endpoint to mark connection `needsReauth` immediately and clear cached access token. Then UI can show reconnect state before user hits export. This removes guesswork and reduces failed calls.
Refs: `P3-103`, `P3-104`, `P3-105`

33. **How do you test BYO flow without leaking real credentials?**
Answer: Use mock/staging Spotify apps and test users, never production credentials in tests. Mock token endpoint and `/me` responses for automated tests. Store test secrets in env manager, not code or logs.
Refs: `P3-57`, `P3-60`, `P3-102`

34. **What edge cases happen if redirect URI mismatches between app and Spotify dashboard?**
Answer: Token exchange fails with invalid redirect or no code, so callback redirects with error state. This usually appears as repeated auth failure loops. Fix is exact URI parity in env and dashboard.
Refs: `P3-73`, `P3-96`, `P3-12`

## D) Last.fm, Rate Limiting, and Quest Verification

35. **Explain your token bucket implementation and math.**
Answer: Bucket has max 5 tokens, refills at 5 tokens per second. Each API call acquires one token; if empty, it waits `((1 - tokens)/rate)` seconds and retries. This smooths burst traffic while keeping average throughput controlled.
Refs: `P3-18`

36. **Why choose 5 req/sec and what real API limit did you optimize for?**
Answer: 5 req/sec is a conservative app-level cap to reduce 429 risk and keep stable latency in shared environments. It is not exact provider hard limit but a safe operating throttle. Good for fairness and predictable behavior.
Refs: `P3-18`, `P3-84`, `P3-85`

37. **How does local in-memory rate limiter behave across multiple serverless instances?**
Answer: Each instance has its own memory and tokens, so global limit is not enforced across instances. Effective aggregate rate can exceed intended cap during scale-out. For true global control, limiter must move to shared storage.
Refs: `P3-18`

38. **What changes for distributed rate limiting in production?**
Answer: Move token bucket state to Redis or dedicated limiter service with atomic increments. Key by API target and maybe user/account. Add jitter/backoff policies and centralized monitoring.
Refs: `P3-18`, `P3-92`, `P3-93`

39. **How is Last.fm pagination handled for streaming verification?**
Answer: Verification loops pages until total pages, no-data, or maxPages cap reached. Daily and weekly use different caps to control cost. It tracks fetched pages and logs capped runs.
Refs: `P3-20`, `P3-19`

40. **How do you avoid false negatives in fuzzy track matching?**
Answer: It normalizes track and artist names by lowercasing, removing bracket text, feat tokens, punctuation, and extra spaces. Matching uses normalized exact title plus inclusive artist comparison. This handles common metadata variations.
Refs: `P3-21`

41. **How do album quests verify "all tracks played" reliably?**
Answer: For each target album, it checks every track at least once and stores per-track state in `trackProgress`. If all tracks are seen, full album credit; else partial progress. Previous progress is merged with `max` so it never regresses.
Refs: `P3-22`, `P3-77`

42. **What data quality issues can occur with Last.fm scrobbles?**
Answer: Missing artist fields, now-playing entries, naming inconsistencies, and delayed scrobbles can happen. Code filters now-playing and normalizes names, but source noise still exists. So some edge mismatches are expected.
Refs: `P3-86`, `P3-21`

43. **How does caching window affect accuracy vs performance?**
Answer: Longer cache improves speed and lower API calls, but can miss newest plays. Current code disables cache (`useCache=false`) for accuracy during debugging, which increases API load. In production, short TTL is better balance.
Refs: `P3-23`

44. **How would you detect suspicious quest spoofing patterns?**
Answer: Track impossible jumps in progress, repeated identical claim timings, and mismatch between verification logs and claim patterns. Add anomaly flags and manual review for outliers. Also correlate with account age/device/token behavior.
Refs: `P3-4`, `P3-31`, `P3-52`

## E) Game Economy and Fairness

45. **Resume mentions pity-based RNG; current docs mention random rarity pipeline. Explain the evolution.**
Answer: Early design included rarity/pity progression. Current productionized path simplified to random catalog drop and stores `rarity: random` for audit consistency. Pity fields remain in schema from earlier design and future flexibility.
Refs: `P3-67`, `P3-28`, `P3-29`

46. **Why keep pity fields in state if current reward rarity is random?**
Answer: Backward compatibility and migration safety. Existing state documents keep old fields so reads do not break and future rarity reintroduction stays easy. It is technical debt but controlled.
Refs: `P3-29`, `P3-68`

47. **How do you ensure reward economy is not inflationary?**
Answer: Use caps and sinks: duplicate cards convert to fixed dust, crafting consumes dust, and some flows require minimum XP. Daily caps in BoraRush also prevent extreme farming. Monitoring supply-demand ratios is still needed.
Refs: `P3-30`, `P3-32`, `P3-34`, `P3-109`

48. **How did you choose dust values and mastery rewards?**
Answer: Rewards are milestone-based and progressively increase with level difficulty. Dust conversion is fixed and easy to reason about for users. It is balance-tuned by gameplay feel and should be revalidated with telemetry.
Refs: `P3-30`, `P3-53`, `P3-55`

49. **What anti-abuse checks do you have for repeated reward triggers?**
Answer: Multiple checks exist: unique runId in BoraRush, daily cap counters, quest `claimed` check, and duplicate inventory handling. So repeated same event usually becomes no-op or capped reward. This blocks basic replay abuse.
Refs: `P3-33`, `P3-34`, `P3-35`, `P3-36`, `P3-26`, `P3-32`

50. **How do you audit inventory grants and detect anomalies?**
Answer: Every grant path can write `InventoryGrantAudit` with source, seed, reason, and timestamps. This creates traceability for reward debugging and abuse detection. You can run anomaly queries on user/time/source combinations.
Refs: `P3-31`, `P3-28`, `P3-111`

51. **How do you guarantee only one reward claim per completion event?**
Answer: Use uniqueness at model level and explicit claimed checks at API level. For quests, once `claimed` is set, second claim returns conflict. For mastery level rewards, unique ledger index blocks duplicate award for same milestone.
Refs: `P3-26`, `P3-27`, `P3-8`

52. **How would you simulate economy balance over 30 days of active users?**
Answer: Build a simulator with user cohorts, daily activity probabilities, XP earnings, duplicate rates, and dust spend behavior. Run Monte Carlo over 30 days and inspect dust inflation, reward velocity, and progression fairness. Then tune constants and rerun.
Refs: `P3-30`, `P3-53`, `P3-67`

53. **What telemetry would prove the game feels fair to users?**
Answer: Look at retention by reward path, complaint ratio about duplicates, time-to-first-reward, and progression spread by cohort. Also monitor “near-miss frustration” signals like many low-XP no-card outcomes. Fairness is both math and perception.
Refs: `P3-67`, `P3-68`, `P3-70`

54. **How do you prevent leaderboard manipulation by scripted clients?**
Answer: Server computes score from verified events, not client-provided score. Sensitive award flows require auth tokens and run checks, and leaderboard entries are keyed by period+user with controlled updates. Add rate limits and bot heuristics for stronger protection.
Refs: `P3-6`, `P3-51`, `P3-66`, `P3-37`

## F) Data Pipelines and Cron Reliability

55. **Explain your cron architecture for trending/quests and failure handling.**
Answer: Quest crons are secured endpoints that generate daily/weekly quests and can run in force mode. Trending cache has dedicated cron endpoint that validates secret and calls cache builder. Failures return non-200 and are logged for retry.
Refs: `P3-39`, `P3-40`, `P3-43`, `P3-44`, `P3-45`

56. **What happens if cron job misses one day?**
Answer: That period may keep old active quests until next successful run, because deactivation and creation happen during cron execution. Users can still play existing quests, but freshness may lag. Manual force run should recover quickly.
Refs: `P3-41`, `P3-42`, `P3-61`

57. **How do you make cron runs idempotent?**
Answer: Quests use deterministic date-based codes and existing checks, so rerun same period does not endlessly duplicate standard records. For manual force, suffix is explicit and controlled. Old quests are deactivated using keep list.
Refs: `P3-88`, `P3-89`, `P3-41`, `P3-42`

58. **How do you avoid duplicate quest generation for same period key?**
Answer: Code first queries existing by generated code and only inserts if missing. For refresh cases it updates existing album quest instead of adding many duplicates. Also daily/weekly dedupe logic selects latest per goal type.
Refs: `P3-88`, `P3-89`, `P3-24`

59. **What retry strategy would you use for flaky external APIs?**
Answer: Use bounded retries with exponential backoff and jitter, and classify retryable status codes. Keep timeout budgets and circuit-breaker threshold to fail fast after repeated errors. Log per-attempt metadata.
Refs: `P3-84`, `P3-85`, `P3-92`

60. **How would you implement dead-letter handling for repeated failures?**
Answer: After max retries, push failed job payload to DLQ collection/queue with error context and attempt count. Add replay tooling for safe reprocessing after fix. Alerting should fire when DLQ grows.
Refs: `P3-39`, `P3-40`, `P3-43`

61. **How do you monitor freshness of scraped data?**
Answer: Track latest snapshot date/version and compare against expected cron schedule. For homepage cache, each build increments version and stores lastUpdated. Alert if freshness SLA is violated.
Refs: `P3-91`, `P3-90`, `P3-45`

62. **How do you maintain scraper robustness when HTML source changes?**
Answer: Keep parser modular, use fallback extractors, and retry transient errors. Also add smoke tests against sample pages and track extraction counts; sudden drops indicate selector breakage. Prefer graceful partial save over total failure.
Refs: `P3-25`, `P3-84`, `P3-85`

## G) API Design and Reliability Scenarios

63. **A user reports "reward claimed but card missing". How do you debug end-to-end?**
Answer: Check quest progress/claimed record, then inventory insert logs, then grant audit entries, then duplicate-to-dust branch. Validate whether reward path was ticket-based and if card got converted due to duplicate. Correlate timestamps and userId across these collections.
Refs: `P3-26`, `P3-79`, `P3-31`, `P3-32`

64. **A user reports "playlist exported to wrong account". How do you root-cause quickly?**
Answer: Inspect export response `mode` and `usedFallback`, then check whether valid user bearer was sent and accepted. Verify `/me` user id at export time and UI token-refresh behavior. Most cases come from expired user token and owner fallback usage.
Refs: `P3-69`, `P3-70`, `P3-16`, `P3-49`

65. **Last.fm API is down for 2 hours. How should quest UX behave?**
Answer: UI should show verification unavailable message, keep current progress visible, and allow retry later. Do not mark quest failed or reset progress. Once API recovers, re-run verification and continue normally.
Refs: `P3-83`, `P3-84`, `P3-85`

66. **Mongo latency spikes suddenly. Which features degrade first?**
Answer: Write-heavy and multi-query routes degrade first: quiz complete, quest verify/claim, leaderboard endpoints. Read-only cached routes degrade later if cache exists. Export to Spotify may still work partly because external calls dominate there.
Refs: `P3-3`, `P3-4`, `P3-37`, `P3-44`

67. **One API endpoint starts returning 500 only in production. What is your triage checklist?**
Answer: Check env var presence, runtime mismatch (`nodejs` vs edge), external dependency status, and recent deploy diff. Reproduce with production payload and inspect structured logs around failing branch. Then add temporary guard and rollback if needed.
Refs: `P3-2`, `P3-39`, `P3-40`, `P3-43`

68. **If an endpoint is hot and expensive, where do you cache first?**
Answer: First cache external API derived data and repeated read responses, not mutation results. Next cache token/status checks with short TTL where safe. Keep cache keys scoped by user and period.
Refs: `P3-44`, `P3-23`, `P3-59`

69. **How do you version API contracts without breaking frontend pages?**
Answer: Add backward-compatible fields first, deprecate later, and keep stable core response keys. For breaking changes, expose `/v2` route or version header and migrate client gradually. Avoid silent shape changes for active screens.
Refs: `P3-48`, `P3-49`, `P3-46`

70. **How do you implement graceful degradation for external dependency failure?**
Answer: Return explicit typed errors (`401`, `503`, retriable messages), preserve local state, and let user retry. Use owner fallback only when behavior is expected and transparent. Never silently continue with wrong semantics.
Refs: `P3-16`, `P3-70`, `P3-103`

## H) Testing and Quality Depth

71. **Which ArmyVerse flows need integration tests but currently have unit tests only?**
Answer: High-value missing integration coverage: Spotify BYO callback->status->export, quest verify->claim reward pipeline, and quiz complete->inventory->leaderboard updates. Current tests mostly focus on core game logic units. End-to-end API+DB tests would catch real regressions faster.
Refs: `P3-54`, `P3-55`, `P3-56`, `P3-57`

72. **What would your highest-value end-to-end test case be?**
Answer: “Quiz completion to visible reward” is highest value: start quiz, complete with valid answers, verify XP, inventory/card behavior, leaderboard update, and response shape. It touches most critical user loop and many writes.
Refs: `P3-62`, `P3-64`, `P3-65`, `P3-32`

73. **How would you test auth matrix: JWT vs Firebase vs unauthenticated?**
Answer: Build table-driven tests for protected routes using three token states: valid Firebase, valid JWT, and missing/invalid. Verify same business result for valid auth types and 401 for unauthenticated. Include user mapping checks for both auth paths.
Refs: `P3-95`, `P3-94`, `P3-48`

74. **How would you test race conditions around reward claims?**
Answer: Fire concurrent claim requests for same quest and assert one success and one conflict/no-op. Repeat for mastery level and BoraRush runId replay. Confirm no double inventory or double dust side effects.
Refs: `P3-26`, `P3-8`, `P3-33`, `P3-35`

75. **How do you validate cryptography helpers safely in tests?**
Answer: Use known plaintext round-trip tests and tamper tests (modify IV/cipher/tag and expect decrypt failure). Never print real secrets and use test-only keys. Add key-rotation simulation tests too.
Refs: `P3-10`, `P3-74`

76. **How do you load-test playlist export endpoints?**
Answer: Use synthetic song payloads and controlled token mocks to stress user and owner modes separately. Measure p95 latency, 4xx/5xx rates, and Spotify API call counts. Include 100-track batching path in test profile.
Refs: `P3-108`, `P3-106`, `P3-107`

77. **How do you prevent flaky tests for time-dependent quest logic?**
Answer: Freeze time with deterministic clock in tests, inject date key helpers, and avoid dependence on wall-clock midnight behavior. Test daily/weekly boundaries explicitly. Keep deterministic seed inputs for selection.
Refs: `P3-24`, `P3-61`, `P3-62`

78. **Which invariants should never be violated in game state?**
Answer: Balances should not become negative, one claim event should not reward twice, and period-scoped progress must map to correct period keys. Leaderboard identity `(periodKey,userId)` must stay unique. Inventory grants should always have auditable source metadata.
Refs: `P3-6`, `P3-7`, `P3-66`, `P3-31`

79. **How do you use feature flags for risky changes?**
Answer: Wrap risky paths (new reward logic, fallback behavior, RAG-like verification changes) behind env or DB-driven flags. Start with internal users, then low percentage rollout, then full release. Keep instant rollback path.
Refs: `P3-39`, `P3-40`, `P3-43`

80. **What would your "definition of done" be for a production-critical ArmyVerse feature?**
Answer: Done means: clear API contract, auth/ownership checks, idempotency for critical writes, happy+failure test coverage, observability hooks, and rollback plan. Also include runbook notes and user-facing error UX before shipping.
Refs: `P3-48`, `P3-95`, `P3-6`, `P3-57`
