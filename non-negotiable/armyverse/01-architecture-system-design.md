# ArmyVerse (Non-Negotiable) - Architecture & System Design

These answers are based on the implemented ArmyVerse project, but rewritten in simple interview language so they are easier to remember and explain clearly.

---

## 1) Explain ArmyVerse architecture end-to-end.

### Short intro
ArmyVerse is built as one main Next.js application. The frontend pages and the backend APIs live in the same codebase. So from an interview point of view, I explain it as a modular monolith: one app, but split into clean feature areas.

### Step-by-step explanation
1. The user opens a page in the Next.js app, like a game page, playlist page, or trending page.
2. When the UI needs data or wants to perform an action, it calls an API route inside the same project.
3. The API route runs on the server side. It usually starts by checking who the user is.
4. Then it connects to MongoDB and runs the actual business logic.
5. The business rules are not all written directly inside the route. They are split into reusable modules such as game logic, Spotify logic, Last.fm logic, blog logic, and trending logic.
6. The server reads or updates MongoDB using Mongoose models like user, quiz session, quest progress, inventory, and leaderboard.
7. Some routes also talk to external services like Spotify, Last.fm, AI models, or scraping sources.
8. After that, the route returns JSON and the frontend updates the UI.

### Design reasoning
I kept it as one codebase because it made development and deployment much faster for a solo project. At the same time, I separated the logic by domain so it still feels organized and explainable in interviews.

### Possible improvement
If the system grows a lot, I would move slow workflows like scraping, verification, and rank recomputation into background jobs.

### Hard Terms Explained Simply
- `Modular monolith` = one application, but internally split into clean modules.
- `Mongoose model` = the code structure used to read and write MongoDB data safely.

---

## 2) How does frontend communicate with backend?

### Short intro
The frontend talks to the backend through HTTP requests to API routes. Even though both live in one Next.js project, I still keep the frontend-backend boundary clear.

### Step-by-step explanation
1. A user clicks something in the UI, like start quiz, verify streaming, or export playlist.
2. The frontend sends a request to an `/api/...` route.
3. If the endpoint is protected, it sends an auth token in the `Authorization` header.
4. The backend route verifies the token, runs the logic, and reads or writes the database if needed.
5. The backend returns JSON.
6. The frontend reads that response and updates the screen.

### Design reasoning
I used this pattern because it is simple, clean, and easy to debug. Even inside one codebase, it helps to think of the frontend and backend as separate responsibilities.

### Possible improvement
For slow operations, I would return a job ID and let the UI poll or subscribe for status instead of waiting in one long request.

### Hard Terms Explained Simply
- `HTTP request` = a message sent from frontend to backend asking for data or an action.
- `Authorization header` = the place where the login token is sent.

---

## 3) How are your 60+ APIs structured?

### Short intro
The APIs are organized by feature folder. So instead of one huge API file, each feature area has its own route structure.

### Step-by-step explanation
1. In Next.js, each route handler is placed in a feature path inside `app/api`.
2. The folder path becomes the API URL.
3. Inside each route, the usual pattern is:
   validate input, verify user, connect to database, run domain logic, then return JSON.
4. I grouped the routes by major feature areas like auth, user, game, Spotify, playlist, blogs, collections, trending, and cron jobs.
5. This means if I want to explain one system in an interview, I can directly point to that domain and follow the flow from route to logic to database model.

### Design reasoning
Feature-based API organization keeps the code easier to navigate. It also matches how people naturally think about the product: auth APIs, game APIs, music APIs, and so on.

### Possible improvement
I would add a more shared validation and auth wrapper so repeated patterns are even more consistent.

### Hard Terms Explained Simply
- `Route handler` = the server function that runs when an API path is called.
- `Domain` = one feature area, like auth or game.

---

## 4) Why did you use Next.js App Router?

### Short intro
I used App Router because the project has many pages and many APIs, and App Router gives a clear route-based structure for both frontend and backend.

### Step-by-step explanation
1. UI pages are organized under the `app` folder.
2. API endpoints are organized under `app/api`.
3. Shared layouts are handled through the layout system.
4. This creates one consistent routing structure for the entire product.
5. Since ArmyVerse has many different modules, that structure makes the app easier to scale and explain.

### Design reasoning
For a project with game features, blog features, music integrations, and admin-like cron behavior, route structure matters a lot. App Router made that structure predictable.

### Possible improvement
If backend complexity grows a lot more, I could split some systems into a dedicated backend service, but for the current stage App Router is practical and fast.

### Hard Terms Explained Simply
- `App Router` = Next.js folder-based routing system.
- `Layout` = a shared page wrapper used by multiple pages.

---

## 5) How does serverless architecture work in your project?

### Short intro
In deployment, the API routes behave like serverless functions. That means there is no always-running backend server managed by me. Instead, the platform runs the needed code when a request comes in.

### Step-by-step explanation
1. A user or frontend calls an API route.
2. The hosting platform starts that function, or reuses an already warm one.
3. The function runs the route logic.
4. It can connect to MongoDB, call external APIs, and return a response.
5. After that, the function ends and the platform manages the rest.

### Design reasoning
This was a good fit because it made deployment simpler and removed a lot of infrastructure work. For a solo project, that matters a lot.

### Possible improvement
Serverless is not ideal for very heavy or long-running tasks, so I would move those into worker jobs if usage increases.

### Hard Terms Explained Simply
- `Serverless function` = backend code that runs only when needed.
- `Warm instance` = a function environment that is already started, so it responds faster.

---

## 6) Where is business logic handled?

### Short intro
The business logic is mainly kept in reusable modules, not inside the UI and not fully inside the route handlers. The routes mostly act as entry points.

### Step-by-step explanation
1. The route receives the request.
2. It verifies the user and validates input.
3. Then it calls logic in reusable modules such as game, Spotify, Last.fm, or blog helpers.
4. Those modules contain the rules of the product, like how rewards are granted, how quests are checked, or how playlists are exported.
5. The route then returns the result back to the client.

### Design reasoning
If I keep too much logic directly inside route files, the code becomes repetitive and harder to maintain. Reusable modules make the project cleaner and easier to reason about.

### Possible improvement
I would standardize the service layer even more and add more integration tests around those domain modules.

### Hard Terms Explained Simply
- `Business logic` = the actual rules of what the app should do.
- `Service layer` = a reusable middle layer where important logic is kept.

---

## 7) How did you design MongoDB schema?

### Short intro
I designed the schema around the real entities and user flows in the product. Instead of forcing everything into one large user document, I split the data based on how it behaves.

### Step-by-step explanation
1. First, I listed the main product flows: quiz, rewards, inventory, quests, mastery, leaderboard, blogs, and music integrations.
2. Then I created separate collections for data with different lifecycles.
3. Some data is long-lived, like user profiles and inventory.
4. Some data is session-based, like quiz sessions.
5. Some data is period-based, like leaderboard entries for daily or weekly windows.
6. I also kept user integration data under the user scope because that information belongs directly to the user.
7. For debugging and safety, I included audit-like records for some reward-related actions.

### Design reasoning
MongoDB works well here because the product has a mix of nested user data, game progress, content, and integrations. Splitting collections by behavior kept the schema clearer.

### Possible improvement
I would keep reviewing indexes and uniqueness rules based on real production query patterns.

### Hard Terms Explained Simply
- `Schema` = the structure of stored data.
- `Collection` = the MongoDB version of a table.
- `Lifecycle` = how data behaves over time, like short-lived or long-lived.

---

## 8) Why MongoDB over SQL?

### Short intro
I chose MongoDB because ArmyVerse has many document-style use cases: user profiles with nested integrations, game state, content metadata, and evolving feature fields.

### Step-by-step explanation
1. User data contains nested structures like profile information and integration details.
2. Game data also has grouped stats and flexible fields.
3. Blog and playlist related data also fit naturally into document-style storage.
4. Since the project evolved quickly, schema flexibility helped me move faster.
5. Mongoose also made it easier to keep some structure and validation while still using MongoDB.

### Design reasoning
For a fast-moving full-stack project, MongoDB gave me flexibility without the extra friction of frequent schema migrations.

### Possible improvement
If I later need strict reporting or analytics-heavy workflows, I might add SQL-based reporting tables for those specific needs.

### Hard Terms Explained Simply
- `SQL` = a relational database system built around tables and joins.
- `Flexible schema` = easier to add or adjust fields without heavy database migrations.

---

## 9) What indexing strategy did you use?

### Short intro
I used indexing mainly on the fields that are read frequently, like user identity fields, integration lookups, quest progress lookups, and leaderboard queries.

### Step-by-step explanation
1. Users need to be found quickly by identity fields like Firebase UID or username.
2. Integration-related records also need fast lookup by owner and external IDs.
3. Quest progress is often checked using user, quest code, and time period together.
4. Leaderboards need fast access by period and score order.
5. So I focused indexes on the hot query paths that matter most for app performance.

### Design reasoning
Indexes are important because they keep frequent reads fast. Without them, the database would scan too much data as usage grows.

### Possible improvement
I would refine compound indexes further based on production query logs and sorting patterns.

### Hard Terms Explained Simply
- `Index` = a structure that helps the database find data faster.
- `Compound index` = one index built using more than one field.
- `Hot query path` = a query that gets used often and matters a lot for performance.

---

## 10) If 100k users join tomorrow, what will break first?

### Short intro
The first problems would likely appear in the features that do heavy external calls or expensive write-heavy processing. In ArmyVerse, that means verification flows, playlist export flows, and leaderboard recomputation.

### Step-by-step explanation
1. Streaming verification depends on external data sources, so latency and failures increase quickly under load.
2. Playlist export can involve multiple Spotify calls, which means more waiting and more chance of provider limits.
3. Leaderboard rank recomputation inside request flow becomes expensive when the number of users grows a lot.
4. Serverless environments also have time limits, so slow chained operations become risky at scale.
5. So the product would not fail first because of simple page rendering. It would fail first in the heavy backend workflows.

### Design reasoning
For the current scale, I optimized for correct working flows instead of fully distributed infrastructure. That was the right tradeoff for a fresher project.

### Possible improvement
I would move heavy jobs to queues, add caching, and precompute leaderboard ranks on a schedule instead of inside user requests.

### Hard Terms Explained Simply
- `Provider limits` = restrictions set by external services like Spotify.
- `Precompute` = calculate something ahead of time instead of during the user request.
