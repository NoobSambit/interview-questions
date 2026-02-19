# Phase 02 - ArmyVerse Core Answers

This file is optimized for **explanation rounds**.
Answers are simple and short, with terminology kept intact.

## Reference Legend

- `A1`: `Documents/ARMYVERSE/README.md:5`
- `A2`: `Documents/ARMYVERSE/README.md:22`
- `A3`: `Documents/ARMYVERSE/README.md:95`
- `A4`: `Documents/ARMYVERSE/docs/architecture/overview.md:13`
- `A5`: `Documents/ARMYVERSE/docs/architecture/overview.md:23`
- `A6`: `Documents/ARMYVERSE/package.json:101`
- `A7`: `Documents/ARMYVERSE/package.json:99`
- `A8`: `Documents/ARMYVERSE/lib/auth/verify.ts:76`
- `A9`: `Documents/ARMYVERSE/lib/auth/jwt.ts:10`
- `A10`: `Documents/ARMYVERSE/lib/auth/jwt.ts:56`
- `A11`: `Documents/ARMYVERSE/lib/auth/jwt.ts:70`
- `A12`: `Documents/ARMYVERSE/lib/utils/secrets.ts:15`
- `A13`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:47`
- `A14`: `Documents/ARMYVERSE/app/api/spotify/client-credentials/route.ts:97`
- `A15`: `Documents/ARMYVERSE/app/api/spotify/callback/route.ts:39`
- `A16`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:144`
- `A17`: `Documents/ARMYVERSE/app/api/spotify/status/route.ts:212`
- `A18`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:142`
- `A19`: `Documents/ARMYVERSE/app/api/playlist/export/route.ts:177`
- `A20`: `Documents/ARMYVERSE/lib/lastfm/client.ts:25`
- `A21`: `Documents/ARMYVERSE/lib/lastfm/client.ts:29`
- `A22`: `Documents/ARMYVERSE/lib/rateLimit.ts:3`
- `A23`: `Documents/ARMYVERSE/lib/game/dropTable.ts:59`
- `A24`: `Documents/ARMYVERSE/lib/game/borarush.ts:29`
- `A25`: `Documents/ARMYVERSE/lib/game/mastery.ts:208`
- `A26`: `Documents/ARMYVERSE/lib/game/quests.ts:4`
- `A27`: `Documents/ARMYVERSE/lib/game/quests.ts:11`
- `A28`: `Documents/ARMYVERSE/lib/game/rewards.ts:94`
- `A29`: `Documents/ARMYVERSE/lib/game/rewards.ts:120`
- `A30`: `Documents/ARMYVERSE/lib/models/UserGameState.ts:40`
- `A31`: `Documents/ARMYVERSE/lib/models/QuizSession.ts:17`
- `A32`: `Documents/ARMYVERSE/lib/models/MasteryLevelRewardLedger.ts:12`
- `A33`: `Documents/ARMYVERSE/lib/models/User.ts:190`
- `A34`: `Documents/ARMYVERSE/lib/models/User.ts:203`
- `A35`: `Documents/ARMYVERSE/lib/models/User.ts:228`
- `A36`: `Documents/ARMYVERSE/vercel.json:10`
- `A37`: `Documents/ARMYVERSE/app/api/cron/daily-quests/route.ts:20`
- `A38`: `Documents/ARMYVERSE/app/api/cron/weekly-quests/route.ts:19`
- `A39`: `Documents/ARMYVERSE/lib/config.ts:2`
- `A40`: `Documents/ARMYVERSE/docs/setup/environment-variables.md:219`
- `A41`: `Documents/ARMYVERSE/scripts/import-questions.js:50`
- `A42`: `Documents/ARMYVERSE/scripts/scrape-fandom-galleries.js:5`
- `A43`: `Documents/ARMYVERSE/lib/cloudinary.ts:7`
- `A44`: `Documents/ARMYVERSE/app/api/upload/route.ts:23`
- `A45`: `Documents/ARMYVERSE/lib/models/Blog.ts:23`
- `A46`: `Documents/ARMYVERSE/lib/models/Blog.ts:106`
- `A47`: `Documents/ARMYVERSE/lib/utils/blog.ts:1`
- `A48`: `Documents/ARMYVERSE/hooks/useSpotifyAuth.ts:23`
- `A49`: `Documents/ARMYVERSE/lib/client/api.ts:22`
- `A50`: `Documents/ARMYVERSE/docs/features/spotify-per-user-export.md:12`
- `A51`: `Documents/ARMYVERSE/docs/features/spotify-per-user-export.md:14`
- `A52`: `Documents/ARMYVERSE/docs/features/game-system.md:21`
- `A53`: `Documents/ARMYVERSE/docs/features/game-system.md:146`
- `A54`: `Documents/ARMYVERSE/docs/features/game-system.md:152`
- `A55`: `Documents/ARMYVERSE/package.json:15`
- `A56`: `Documents/ARMYVERSE/docs/architecture/overview.md:203`

## A) Product and Scope Understanding

1. **What problem does ArmyVerse solve for BTS fans?**
Answer: It puts multiple fan needs in one place: music discovery, playlist creation, analytics, games, and community content. So users do not need to jump between many tools.
Refs: `A1`, `A2`, `A3`

2. **Which 3 modules are most important in ArmyVerse today?**
Answer: I would say playlist + Spotify integration, game system (quiz/quests/leaderboard), and community/blog features. These are the most active value modules.
Refs: `A2`, `A3`, `A45`

3. **Who is the primary user persona for ArmyVerse?**
Answer: A BTS fan who wants both utility and engagement: playlists, stats, and game/community interaction. So the persona is not only content consumer, but active participant.
Refs: `A1`, `A3`

4. **How would you describe ArmyVerse architecture in under 2 minutes?**
Answer: It is a Next.js 14 App Router product with API routes in the same codebase. MongoDB + Mongoose stores core data, and external APIs like Spotify/Last.fm/Firebase are integrated. Auth, game, playlist, trending, and blog domains are separated by route groups.
Refs: `A4`, `A5`, `A6`, `A7`

5. **Why did you choose a single Next.js app instead of separate frontend/backend repos?**
Answer: Speed and cohesion. For this product stage, a single repo made feature shipping faster and reduced integration overhead. It also simplified deployment for a student-led product.
Refs: `A4`, `A6`

6. **Which features are read-heavy vs write-heavy in ArmyVerse?**
Answer: Read-heavy: stats dashboards, blogs, profile views, trending pages. Write-heavy: quiz completion, quest progress, rewards, comments, and playlist export actions.
Refs: `A3`, `A28`, `A45`, `A46`

7. **What are the critical user journeys in the app?**
Answer: Sign in -> connect Spotify/Last.fm -> generate or create playlist -> export -> play quiz/quests -> collect rewards and track leaderboard. Another key journey is write/read blog content.
Refs: `A8`, `A16`, `A18`, `A52`

8. **How do you prioritize features in a fan-platform product?**
Answer: First I prioritize retention loops, then reliability, then UI polish. So core loops like auth, playlist save, quests, and rewards come before cosmetic work.
Refs: `A2`, `A3`, `A29`

9. **What tradeoff did you make between feature richness and maintainability?**
Answer: I accepted high feature count, but grouped logic by domain and kept helpers/models modular. It is still a tradeoff, so testing and documentation need continuous improvement.
Refs: `A5`, `A55`

10. **If users drop off after signup, what data would you inspect first?**
Answer: I would inspect conversion funnel: signup success, Spotify connect success, first playlist generation, first export, first game completion. I would also inspect token error rates and 401 patterns.
Refs: `A16`, `A19`, `A48`, `A49`

## B) Next.js and API Layer

11. **Why did you use Next.js 14 App Router?**
Answer: It gave a clean full-stack setup with App Router pages plus API route handlers in one project. For rapid feature delivery, this was efficient.
Refs: `A6`, `A4`

12. **What is the difference between server components and client components in your app?**
Answer: Client components handle interactive UI and hooks. Server side route handlers and backend logic handle secure operations, DB calls, and integrations.
Refs: `A4`, `A48`, `A18`

13. **How do Next.js route handlers work in your project?**
Answer: Domain APIs are under `app/api/**/route.ts`. Each handler validates auth/input, runs business logic, then returns JSON responses. This is consistent across modules like auth/game/spotify.
Refs: `A5`, `A18`, `A37`

14. **How are API routes organized by domain in ArmyVerse?**
Answer: They are grouped by functional domain: auth, game, playlist, spotify, blogs, user, trending, cron. This keeps ownership clear and easier to maintain.
Refs: `A5`, `Documents/ARMYVERSE/app/api`

15. **Why use `force-dynamic` in some API routes?**
Answer: Routes like Spotify status/export need fresh token and user state on every request. `force-dynamic` avoids stale cache behavior for these sensitive flows.
Refs: `A16`, `A18`

16. **How do you manage runtime differences (`edge` vs `nodejs`)?**
Answer: For token-heavy or external API logic, I explicitly keep `nodejs` runtime where needed. This avoids runtime mismatch issues and keeps behavior predictable.
Refs: `A18`, `A36`

17. **What are common pitfalls in Next.js API route error handling?**
Answer: Main pitfalls are silent token failures, weak status-code mapping, and leaking internal errors to clients. I handle these with explicit 401/403/5xx and safer response messages.
Refs: `A19`, `A51`, `A49`

18. **How do you standardize API response shapes across modules?**
Answer: I keep response objects explicit (`success`, `error`, payload fields) and use a client-side `ApiError` wrapper to normalize failure handling. This gives consistent UX.
Refs: `A49`, `A18`

19. **How do you handle retries and partial failures on the frontend?**
Answer: Spotify flow checks status, refreshes token, and retries in controlled manner. If user token is invalid, API can return 401 so frontend can recover instead of silently failing.
Refs: `A48`, `A50`, `A51`

20. **How do you protect private API routes from unauthorized access?**
Answer: I verify bearer token first, then map to user, then enforce resource ownership checks. No protected action runs before these checks pass.
Refs: `A8`, `A24`, `A37`

## C) Database and Models

21. **Why did you choose MongoDB + Mongoose for ArmyVerse?**
Answer: Because feature domains evolved fast and document-style modeling was practical for that speed. Mongoose gave schema control while still flexible for iterative development.
Refs: `A7`, `A4`

22. **Explain your model organization strategy in `lib/models`.**
Answer: Models are split by domain entity: user, game, content, analytics, integration support. This keeps schemas reusable and route handlers cleaner.
Refs: `Documents/ARMYVERSE/lib/models`, `A30`, `A45`

23. **How do you avoid schema drift in fast-moving projects?**
Answer: I keep schema defaults, validation, and indexes inside model definitions, and use migration/backfill scripts when behavior changes. I also avoid hidden schema assumptions in handlers.
Refs: `A45`, `A31`, `A41`

24. **Which entities are core to gameplay state?**
Answer: Core gameplay state lives in `UserGameState`, quest progress models, quiz session, mastery progress, and leaderboard entries. These models drive progression and rewards.
Refs: `A30`, `A26`, `A28`, `A31`

25. **How do you model one-to-many relationships in your design?**
Answer: I mostly use per-user documents with nested/linked arrays and separate ledger-style collections for event history. This gives fast user-centric reads and auditable writes.
Refs: `A30`, `A32`, `A46`

26. **How do you index frequently queried collections?**
Answer: I add indexes on identity keys, lifecycle fields, and query filters. Example: unique userId in game state, TTL on quiz session expiry, and unique composite indexes in ledgers.
Refs: `A30`, `A31`, `A32`

27. **Where do you use TTL or time-based cleanup?**
Answer: Quiz sessions use TTL index via `expiresAt`, so expired sessions auto-clean up. I also use time-based keys and cron schedules for daily/weekly systems.
Refs: `A31`, `A26`, `A27`, `A36`

28. **How do you prevent duplicate writes in reward flows?**
Answer: I use upsert + atomic increments for balances and unique ledger constraints for one-time rewards. If duplicate insert happens, I handle duplicate-key case safely.
Refs: `A28`, `A29`, `A32`

29. **How do you handle migrations when schema fields evolve?**
Answer: I use scripts and controlled backfills, then keep route logic backward-safe during transition. I avoid hard cutovers without compatibility window.
Refs: `A41`, `Documents/ARMYVERSE/scripts/migrate-existing-users.js`

30. **How would you backfill historical data safely?**
Answer: I run idempotent scripts in batches, with dry-run first and logs for audit. I avoid destructive full rewrites unless clearly needed.
Refs: `A41`, `Documents/ARMYVERSE/docs/scripts/seed-inventory-sample.md`

## D) Authentication and Security Basics

31. **Why did you support both Firebase auth and custom JWT auth?**
Answer: Because social login users prefer Firebase OAuth, while username/password users need direct JWT auth. Dual mode improved accessibility and product flexibility.
Refs: `A8`, `A9`, `A33`

32. **How does unified auth verification work in your app?**
Answer: `verifyAuth` first tries Firebase token verification, and if not valid it falls back to JWT verification and DB lookup. So protected routes can support both auth types cleanly.
Refs: `A8`, `A24`

33. **How do you extract and verify bearer tokens in route handlers?**
Answer: I read `Authorization: Bearer <token>`, validate token, and only then proceed with business logic. Invalid or missing token returns unauthorized response.
Refs: `A8`, `A18`

34. **What are security risks of storing JWT in localStorage?**
Answer: Main risk is token theft through XSS. So strict content sanitization, CSP, and short token lifetime are important safeguards.
Refs: `A9`, `A10`, `A45`

35. **Why is `JWT_SECRET` mandatory and what happens if missing?**
Answer: Without it, token signing/verification is insecure or impossible. In this code, missing secret throws fatal startup-level error for safety.
Refs: `A9`

36. **How do you hash passwords and why bcrypt cost factor matters?**
Answer: I hash with bcrypt using salt rounds (`genSalt(10)`). Cost factor balances security and login performance.
Refs: `A10`

37. **How do you validate signup/signin payloads?**
Answer: I validate username/password format and business rules before writing user records. Invalid formats and reserved values are blocked early.
Refs: `A11`

38. **How do you enforce username/password quality rules?**
Answer: Username length and allowed pattern are enforced; reserved names are blocked. Password length and character requirements are also enforced.
Refs: `A11`

39. **How do you handle auth fallback when Firebase Admin is not configured?**
Answer: If Firebase Admin env is missing, Firebase verification is skipped and JWT path still works. This is useful for controlled environments but must be configured carefully in production.
Refs: `A8`, `A24`

40. **What extra hardening would you add if this app scaled to enterprise usage?**
Answer: I would add strict CSP, better token storage strategy, secret rotation, centralized audit logs, rate limiting per endpoint, and stronger security testing in CI.
Refs: `A12`, `A39`, `A40`

## E) Spotify and Playlist Features

41. **What playlist generation inputs do you collect from users?**
Answer: Inputs include mood/type/length, seed tracks, and contextual constraints. The AI flow then shapes the playlist from those parameters.
Refs: `A2`, `A50`

42. **How does AI playlist generation flow work end-to-end?**
Answer: User gives constraints, model generates candidate tracks, system validates/matches tracks, and user can export result to Spotify. It is generation plus post-processing, not raw model output only.
Refs: `A2`, `A4`, `A18`

43. **How do you resolve generated songs to real Spotify track IDs?**
Answer: If `spotifyId` is present I use it directly; otherwise I run Spotify search with track + artist query and map the top valid match.
Refs: `A18`

44. **Why keep both manual playlist and AI playlist flows?**
Answer: Because some users want full control, others want quick discovery. Supporting both increases usability and retention.
Refs: `A2`, `A50`

45. **How does export-to-Spotify work at high level?**
Answer: Export endpoint decides mode (user token or owner fallback), creates playlist in target account, then adds tracks in batches. It returns URL + stats + mode info.
Refs: `A18`, `A50`

46. **Why does ArmyVerse support owner fallback mode for exports?**
Answer: It prevents hard failure when user token is unavailable, so user still gets output. This improves resilience for real users.
Refs: `A18`, `A50`, `A51`

47. **What is the purpose of BYO Spotify app mode?**
Answer: It lets users connect using their own Spotify app credentials, reducing central app limits and giving flexible user-level control. Secrets are encrypted at rest.
Refs: `A13`, `A15`, `A34`, `A35`

48. **What is PKCE and where is it applied in your flow?**
Answer: PKCE is OAuth protection using code verifier/challenge for public clients. It is used in BYO path when client secret is not provided.
Refs: `A14`, `A15`

49. **Which scopes are required for playlist creation and why?**
Answer: At minimum, `playlist-modify-public`, `playlist-modify-private`, and `user-read-private` are needed for secure playlist operations and identity checks.
Refs: `A13`, `Documents/ARMYVERSE/docs/setup/environment-variables.md:80`

50. **How do you handle expired Spotify tokens in the UX?**
Answer: Status endpoint checks/refreshes token and returns health state. If token is invalid, frontend prompts reconnect or controlled retry.
Refs: `A16`, `A17`, `A48`

## F) Game System Basics

51. **Explain quiz flow from question fetch to reward grant.**
Answer: User starts quiz, answers questions, score is computed, XP is awarded, and card drop is checked by XP gate. Then quest and leaderboard updates run.
Refs: `A52`, `A23`, `A28`

52. **How do you score quiz performance and convert it to XP?**
Answer: Difficulty-weighted question scoring is used, and total XP is then applied to user balance and progression systems. XP flows into level and leaderboard updates.
Refs: `A52`, `A28`, `A53`

53. **Why is card drop XP-gated in quiz flow?**
Answer: XP gate keeps reward economy balanced and discourages low-effort spam attempts. If XP is below threshold, no card drop is granted.
Refs: `A23`, `A52`

54. **What is the role of `UserGameState` in your system?**
Answer: It is the core player state object: XP, dust, streaks, limits, and progression signals. Most reward and limit logic depends on it.
Refs: `A30`

55. **How are daily limits enforced for quiz starts?**
Answer: A UTC date key is used; when date changes, counter resets. Each start increments counter and checks against limit.
Refs: `A22`, `A30`

56. **What is Borarush and how is XP computed from turns?**
Answer: Borarush is a game mode where XP depends on number of turns to finish. Fewer turns gives higher XP based on fixed tier table.
Refs: `A24`, `A52`

57. **How do mastery levels work in broad terms?**
Answer: Mastery tracks XP by member/era, converts XP to levels, then grants milestone and level-up rewards. OT7 has harder progression because divider is larger.
Refs: `A25`, `A53`

58. **How do daily and weekly quest period keys work?**
Answer: Daily key uses UTC date, and weekly key uses ISO-week format. Progress is stored against these keys to keep resets correct.
Refs: `A26`, `A27`, `A54`

59. **How is leaderboard score updated across daily/weekly/all-time?**
Answer: On XP award, all three periods are updated in parallel, and score is incremented with `$inc`. This keeps period consistency.
Refs: `A28`, `A29`, `A54`

60. **How do you avoid leaderboard inconsistencies in concurrent updates?**
Answer: I use atomic update operations (`$inc` + upsert), not read-modify-write score overwrites. This reduces race condition risk.
Refs: `A28`, `A29`

## G) Content and Community Features

61. **Why did you build a Tiptap-based blog editor?**
Answer: It supports rich editing features needed for community content while staying developer-friendly in React/Next setup. It gives better writing UX than plain textarea.
Refs: `A45`, `Documents/ARMYVERSE/README.md:227`

62. **What is stored for a blog post besides title and content?**
Answer: Visibility, soft-delete flags, tags, mood, cover image, author metadata, reactions, comments, read time, and views are stored.
Refs: `A45`, `A46`

63. **How do you sanitize user-generated content before storing/displaying?**
Answer: I use sanitize-first thinking: allowlisted rich text and safe rendering rules. In ArmyVerse architecture, content processing explicitly includes HTML sanitization before storage/render. This reduces XSS risk while keeping rich formatting.
Refs: `A56`, `A45`

64. **How do nested comments or reactions affect data modeling?**
Answer: They increase write complexity and require careful schema design for updates and indexing. I keep comments and reactions structured so read/update remains manageable.
Refs: `A46`

65. **How do you handle image uploads and media hosting?**
Answer: Upload route sends image to Cloudinary and stores secure URL/publicId. Cloudinary config and transformation helpers are centralized.
Refs: `A43`, `A44`

66. **What is your strategy for read-time or metadata computation?**
Answer: Read time is computed from word count using a simple 200 wpm formula. Metadata like trending score is derived from views/reactions and recency.
Refs: `A47`, `A45`

## H) Cron, Integrations, Testing, Deploy

67. **Which tasks run on cron and why are they scheduled?**
Answer: Daily/weekly quest generation and trending cache updates are scheduled so users always see fresh cycles without manual trigger. These are time-based product loops.
Refs: `A36`, `A37`, `A38`

68. **How do you secure cron endpoints from unauthorized triggers?**
Answer: Cron routes check bearer secret and reject unauthorized requests. For local debugging, auth can be explicitly disabled by env flag.
Refs: `A37`, `A38`, `A39`, `A40`

69. **Why does Last.fm integration need caching and throttling?**
Answer: Because external API can fail under bursts and has practical limits. Token bucket throttling plus cache logic improves stability and response time.
Refs: `A20`, `A21`, `Documents/ARMYVERSE/lib/game/streamingVerification.ts:250`

70. **What is your current test strategy and biggest testing gap in ArmyVerse?**
Answer: I have Jest-based unit tests, especially for game/reward logic. Biggest gap is deeper integration and end-to-end coverage across auth + external API + cron flows.
Refs: `A55`, `Documents/ARMYVERSE/test/lib/game/mastery.test.ts`

71. **What deployment setup do you use and what environment variables are critical?**
Answer: Main hosting is Vercel for ArmyVerse, with env-driven integration config. Critical envs include JWT, Firebase Admin, Spotify keys, CRON secret, Last.fm, Mongo URI, and encryption key.
Refs: `A36`, `A39`, `A40`, `A12`

72. **If production breaks after deploy, how do you triage quickly?**
Answer: First check health and logs, then isolate if failure is auth, DB, or external API. I verify env variables, rollback risky change if needed, then patch with smallest safe fix.
Refs: `A18`, `A37`, `A49`

---

## Quick Speaking Tip

In interview, answer each technical question in this order:
1. What it is
2. Why you did it
3. One tradeoff

This sounds clear and mature.
