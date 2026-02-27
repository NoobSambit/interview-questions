# ArmyVerse (Non‑Negotiable) — Architecture & System Design

These answers are written strictly from what exists in the repo: `Documents/ARMYVERSE/`.

---

## 1) Explain ArmyVerse architecture end-to-end.

### Short intro
ArmyVerse is a single Next.js 14 app. The UI and the backend API both live in the same codebase. The backend is implemented as Next.js API routes under `app/api/*`. Data is stored in MongoDB using Mongoose models under `lib/models/*`.

### Step-by-step explanation
1. The user opens a page in `app/*` (React UI).
2. When UI needs data or needs to do an action, it calls an API route like `/api/game/quiz/start` or `/api/spotify/status`.
3. The API route runs server-side and usually starts by verifying the user using `verifyAuth()`.
4. The route connects to MongoDB using `connect()`.
5. Business rules are done in reusable modules under `lib/game/*`, `lib/spotify/*`, `lib/lastfm/*`, `lib/blog/*`, etc.
6. Data is read/written through Mongoose models like `User`, `QuizSession`, `InventoryItem`, `UserQuestProgress`, `LeaderboardEntry`.
7. Some routes call external APIs (Spotify, Last.fm, Groq, scraping sources). The route then merges that result into the response.
8. The API returns JSON. The frontend updates UI state.

### Design reasoning (why I did it)
- I kept it as a modular monolith because it is faster to ship as a solo/fresher project.
- Keeping UI + API together reduces integration overhead and makes deployment simpler (one Vercel app).
- I still separated code by domain in `lib/*` so the system stays explainable and maintainable.

### Possible improvement (1–2 lines)
If traffic grows, I would move heavy workflows (verification, large exports, rank recompute) into background jobs and use a queue.

### Hard Terms Explained Simply
- `Modular monolith` = one app, but code is separated into clean modules.
- `Mongoose model` = a typed way to read/write MongoDB collections.

---

## 2) How does frontend communicate with backend?

### Short intro
Frontend calls the backend through HTTP requests to Next.js API routes. It is basically normal REST-style JSON communication.

### Step-by-step explanation
1. UI triggers an action (button click like “Start Quiz”, “Verify Streaming”, “Export”).
2. Frontend calls `/api/...` using `fetch` (or a wrapper).
3. It sends `Authorization: Bearer <token>` for protected endpoints.
4. Backend route verifies token and runs logic.
5. Backend returns JSON with results.
6. UI renders the updated state.

### Design reasoning (why I did it)
- REST-style endpoints are simple to reason about and easy to debug in interviews.
- Next.js API routes keep the frontend-backend boundary clear, even inside one repo.

### Possible improvement (1–2 lines)
For long operations, I would return a `jobId` and let UI poll status instead of waiting in one request.

### Hard Terms Explained Simply
- `REST` = using URLs + HTTP methods to do actions and get data.
- `Authorization header` = the token you send so backend knows who you are.

---

## 3) How are your 60+ APIs structured?

### Short intro
APIs are structured by feature folder under `app/api/*`. Each folder contains a `route.ts` file that implements `GET/POST/...`.

### Step-by-step explanation
1. Each API endpoint is a Next.js route handler file: `app/api/**/route.ts`.
2. The folder path becomes the URL path. Example:
   - `app/api/game/quiz/start/route.ts` -> `/api/game/quiz/start`
3. Inside each route, the common pattern is:
   - validate input
   - `verifyAuth()`
   - `connect()`
   - call domain functions in `lib/*`
   - write/read models
   - return JSON
4. APIs are grouped into main domains:
   - `auth/*`, `user/*`, `game/*`, `spotify/*`, `playlist/*`, `blogs/*`, `collections/*`, `trending/*`, `cron/*`.

### Design reasoning (why I did it)
- Feature-folder structure matches how interviewers think: game APIs, auth APIs, Spotify APIs.
- It keeps navigation easy in the repo.

### Possible improvement (1–2 lines)
I would add a small “shared middleware” layer so auth/validation patterns are even more consistent.

### Hard Terms Explained Simply
- `Route handler` = server-side function that runs when an API URL is called.
- `Domain` = a feature area like Game, Auth, Spotify.

---

## 4) Why did you use Next.js App Router?

### Short intro
I used Next.js App Router because the project has many pages and many API routes, and App Router is the modern Next.js structure.

### Step-by-step explanation
1. UI pages live in `app/*` with nested routes.
2. API endpoints live in `app/api/*` with route handlers.
3. Layouts and shared UI are handled through `app/layout.tsx` and nested layouts.
4. It gives a clean route-based structure for both UI and API.

### Design reasoning (why I did it)
- It reduces boilerplate and keeps routing predictable.
- For a large feature project (game + blog + Spotify), route structure matters.

### Possible improvement (1–2 lines)
If I needed more backend flexibility, I could split a dedicated backend service, but for now App Router is the fastest working setup.

### Hard Terms Explained Simply
- `App Router` = the folder system that controls routes in Next.js.
- `Layout` = shared wrapper UI across pages.

---

## 5) How does serverless architecture work in your project?

### Short intro
On Vercel, each API route runs like a serverless function. You don’t keep a long-running Node server; functions run per request.

### Step-by-step explanation
1. User calls `/api/...`.
2. Vercel spins up the function (or reuses a warm instance).
3. The function executes route code (`GET/POST`).
4. It connects to MongoDB, runs logic, returns response.
5. The function finishes. It can be reused later if it stays warm.

### Design reasoning (why I did it)
- Vercel + Next.js is simple for deployment.
- For a student project, it removes infra tasks like provisioning servers.

### Possible improvement (1–2 lines)
For heavy tasks, serverless timeouts can be an issue; I would move those tasks to background workers.

### Hard Terms Explained Simply
- `Serverless function` = code that runs only when a request comes.
- `Cold start` = first run can be slower because it has to start.

---

## 6) Where is business logic handled?

### Short intro
Business logic is mostly inside `lib/*`, not inside UI components. API routes call these helpers.

### Step-by-step explanation
1. API routes are the entry point for actions.
2. Core rules live in `lib/game/*` (rewards, mastery, quests, leaderboard).
3. Integrations live in `lib/spotify/*`, `lib/lastfm/*`.
4. Models in `lib/models/*` represent the database.
5. Routes glue them together and return responses.

### Design reasoning (why I did it)
- If logic lives only inside routes, it becomes hard to test and reuse.
- Keeping logic in `lib/` keeps code cleaner and more explainable.

### Possible improvement (1–2 lines)
I would standardize “service layer” boundaries even more and add more integration tests.

### Hard Terms Explained Simply
- `Business logic` = the real rules of the app (what should happen).
- `Service layer` = a middle layer that keeps rules reusable.

---

## 7) How did you design MongoDB schema?

### Short intro
I designed schema around real app entities: users, quiz sessions, inventory items, quests, mastery, leaderboards, blogs, and snapshots.

### Step-by-step explanation
1. I listed key entities and flows: quiz -> reward -> inventory -> mastery -> leaderboard, plus blogs and Spotify integrations.
2. I created separate collections where data has different lifecycle:
   - short-lived: `QuizSession`
   - long-lived: `User`, `InventoryItem`, `UserGameState`
   - time-windowed: `LeaderboardEntry` (`daily/weekly/alltime`)
   - scheduled: `QuestDefinition` and `UserQuestProgress`
3. I stored “integration data” inside `User.integrations.*` so it stays user-scoped.
4. For audit-like needs, I added ledger/audit collections (example: inventory grant audit / mastery ledgers) so I can debug issues.

### Design reasoning (why I did it)
- MongoDB fits evolving schemas and nested user integration documents.
- Separate collections help keep queries clear and avoid huge single documents.

### Possible improvement (1–2 lines)
I would review indexes regularly based on real query patterns and add unique constraints for more idempotency guarantees.

### Hard Terms Explained Simply
- `Schema` = shape of stored data.
- `Collection` = a table-like group in MongoDB.

---

## 8) Why MongoDB over SQL?

### Short intro
In ArmyVerse, data is a mix of game state, nested integrations, and content. MongoDB was faster for me to iterate and model.

### Step-by-step explanation
1. User documents include nested `profile` and `integrations` objects.
2. Game data includes lists and nested stats.
3. Content features (blogs, reactions, comments) also fit document-style storage.
4. Using Mongoose makes it easy to evolve fields without strict migrations.

### Design reasoning (why I did it)
- For a fast-changing product, schema flexibility helps.
- Document model maps naturally to “user profile + integrations” use case.

### Possible improvement (1–2 lines)
If I needed strict relational reporting, I would consider SQL for analytics tables, while keeping core app in Mongo.

### Hard Terms Explained Simply
- `SQL` = relational database with tables and joins.
- `Flexible schema` = easier to add fields without heavy migrations.

---

## 9) What indexing strategy did you use?

### Short intro
I rely on indexes for user lookup and integration lookups, and for leaderboard/quest lookups. Some indexes are defined in schema (like `firebaseUid`).

### Step-by-step explanation
1. User lookup fields are indexed, for example `firebaseUid` and unique `username`.
2. Spotify integration has indexed identifiers like `spotifyUserId` and `ownerId`.
3. Quest progress is read by `(userId, code, periodKey)` style filters.
4. Leaderboard is queried by `periodKey` and sorted by score.

### Design reasoning (why I did it)
- These are the “hot query” paths in the product.
- Indexes make reads faster and reduce load under traffic.

### Possible improvement (1–2 lines)
I would add/verify compound indexes for leaderboard sorting and quest progress uniqueness, based on production query profiling.

### Hard Terms Explained Simply
- `Index` = a speed-up structure for database searches.
- `Compound index` = index on multiple fields together.

---

## 10) If 100k users join tomorrow, what will break first?

### Short intro
The first things to break are the endpoints that do heavy external calls and repeated DB writes: streaming verification, playlist export, and leaderboard rank recomputation.

### Step-by-step explanation
1. Streaming quest verification depends on external scrobble data and can be expensive.
2. Playlist export hits Spotify APIs multiple times (create playlist + add tracks + search fallback).
3. Leaderboard currently recomputes ranks lazily in the request path, which becomes slow at scale.
4. Serverless functions have time limits; heavy chains can hit timeout under load.

### Design reasoning (why I did it)
- For a fresher project, I optimized for working product first.
- I still added idempotency patterns in critical paths (claims, run IDs, etc.) to reduce correctness failures.

### Possible improvement (1–2 lines)
Move heavy workflows into background jobs, add caching, and precompute leaderboard ranks periodically.

### Hard Terms Explained Simply
- `Hot endpoint` = an API route that gets called a lot.
- `Timeout` = request took too long and got stopped.
