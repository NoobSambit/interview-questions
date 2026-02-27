# DocBuilder (Non‑Negotiable) — Backend & FastAPI

These answers are written strictly from what exists in the repo: `Documents/docbuilder/`.

Relevant code I am referencing:
1. FastAPI app + middleware: `Documents/docbuilder/backend/main.py`
2. API routes: `Documents/docbuilder/backend/app/api/endpoints.py`
3. Auth dependency: `Documents/docbuilder/backend/app/core/auth.py`
4. Data models (Pydantic): `Documents/docbuilder/backend/app/models.py`
5. Firestore init: `Documents/docbuilder/backend/app/db/firestore.py`

---

## 1) Why FastAPI over Express?

### Short intro
In this project, the backend is doing a lot of “Python-first” work: generating DOCX/PPTX, using LangChain utilities, and doing web scraping + RAG logic. So using FastAPI (Python) fits the actual code I wrote.

### Step-by-step explanation
1. The backend is written in Python and starts from `FastAPI(title="AI Doc Builder API")` in `Documents/docbuilder/backend/main.py`.
2. The main output feature is exporting, and I implemented it with Python libraries: DOCX uses `python-docx` and PPTX uses `python-pptx` (both in `Documents/docbuilder/backend/app/services/export_service.py`).
3. RAG is implemented in Python too, using LangChain + FAISS + HuggingFace embeddings inside `Documents/docbuilder/backend/app/core/rag.py`.
4. Request/response schemas are strongly structured using Pydantic models in `Documents/docbuilder/backend/app/models.py`.
5. FastAPI gives me clean async endpoints (`async def ...`), automatic OpenAPI docs, and dependency-injection style auth (`Depends(get_current_user)`).

### Design reasoning (why I did it)
I optimized for “one language end-to-end” in the backend. If I used Express, I would still need Python somewhere for doc/ppt generation, or I would have to re-implement those parts in Node.

### Possible improvement (1–2 lines)
If this grew bigger, I’d separate export into a background worker so the API stays responsive under heavy exports.

### Hard Terms Explained Simply
- `OpenAPI docs` = auto-generated API documentation page.
- `Dependency injection (Depends)` = FastAPI passes common things (like the user) into routes automatically.

---

## 2) What is async/await?

### Short intro
`async/await` is a way to write code that can “pause while waiting” (like waiting for DB/network) without blocking the whole server.

### Step-by-step explanation
1. In my API, most endpoints are `async def ...` (example in `Documents/docbuilder/backend/app/api/endpoints.py`).
2. When an endpoint does I/O (database calls, HTTP calls, etc.), async lets the server handle other requests during the wait.
3. In my code, this shows up as async route handlers and an async auth dependency (`get_current_user()` in `Documents/docbuilder/backend/app/core/auth.py`).
4. Even if some libraries inside are synchronous (like Firestore client in many setups), FastAPI’s async structure still keeps the code consistent and future-ready.

### Design reasoning (why I did it)
FastAPI is built around async patterns. Using async endpoints is the “native style” and makes concurrency easier to scale later.

### Possible improvement (1–2 lines)
For heavy CPU tasks (like large exports), I’d move the work off the request thread (job queue) instead of relying only on async.

### Hard Terms Explained Simply
- `I/O` = waiting for network, database, or file operations.
- `Blocking` = the server can’t do anything else while waiting.

---

## 3) What is event loop?

### Short intro
The event loop is the engine that runs async code. It schedules “what runs next” while other tasks are waiting on I/O.

### Step-by-step explanation
1. FastAPI runs on an ASGI server like `uvicorn` (see `Documents/docbuilder/backend/main.py` start command in the `__main__` block).
2. `uvicorn` uses an event loop to run async endpoints.
3. When one request is waiting (for example waiting on a network call), the event loop can run another request.

### Design reasoning (why I did it)
This is how FastAPI achieves good concurrency without spawning a new thread per request.

### Possible improvement (1–2 lines)
If I need more CPU parallelism, I’d run multiple worker processes and keep exports off the main request path.

### Hard Terms Explained Simply
- `Event loop` = a scheduler that keeps async tasks moving forward.
- `Concurrency` = handling multiple requests “at the same time” in a practical way.

---

## 4) What is ASGI?

### Short intro
ASGI is the “server interface standard” that FastAPI follows. It’s like a contract between FastAPI and the server (uvicorn) about how requests are handled.

### Step-by-step explanation
1. In deployment configs, the server command is `uvicorn main:app ...` (see `Documents/docbuilder/render.yaml` and `Documents/docbuilder/backend/railway.toml`).
2. `uvicorn` is an ASGI server. It can run async Python web apps.
3. FastAPI exposes an ASGI app object (`app = FastAPI(...)` in `Documents/docbuilder/backend/main.py`).

### Design reasoning (why I did it)
ASGI is the modern async standard, so it fits my use of `async def` endpoints.

### Possible improvement (1–2 lines)
If I needed WebSockets or streaming updates heavily, ASGI is already the right base; I’d just add those routes.

### Hard Terms Explained Simply
- `ASGI` = the standard way an async Python web app talks to its server.
- `Uvicorn` = the program that runs my FastAPI app.

---

## 5) How does FastAPI handle concurrency?

### Short intro
FastAPI handles many requests by running async route handlers on the event loop, so waiting time (network/DB) doesn’t freeze the whole server.

### Step-by-step explanation
1. Requests come in through `uvicorn`.
2. FastAPI routes are async, so while one request is waiting, the event loop can switch to another request.
3. In my project, typical “wait points” are auth verification (`Documents/docbuilder/backend/app/core/auth.py`), Firestore reads/writes (`Documents/docbuilder/backend/app/api/endpoints.py`), and optional RAG web fetches (`Documents/docbuilder/backend/app/core/rag.py`).

### Design reasoning (why I did it)
This is a good default for a web API that calls external services. It keeps the backend responsive under normal load.

### Possible improvement (1–2 lines)
I’d add timeouts, retries, and a background queue for slow tasks so one user can’t tie up the server.

### Hard Terms Explained Simply
- `Responsive` = the server still answers other users quickly.
- `Timeout` = stop waiting after a limit instead of hanging forever.

---

## 6) What is Pydantic?

### Short intro
Pydantic is how I define “the shape of data” in the backend. FastAPI uses it to validate request bodies and to format responses.

### Step-by-step explanation
1. In `Documents/docbuilder/backend/app/models.py`, I defined models like `Project`, `Section`, `ProjectCreate`, `ProjectUpdate`, and request bodies like `SuggestOutlineRequest`, `GenerateContentRequest`, `RefineRequest`.
2. When a request comes, FastAPI validates JSON into these models.
3. This reduces bugs because I don’t manually parse every field.
4. I also use Pydantic schemas for structured LLM outputs (`OutlineSchema`, `SectionContentSchema`, `RefinementOutputSchema` in `Documents/docbuilder/backend/app/models.py`), and parse them using `PydanticOutputParser` in `Documents/docbuilder/backend/app/core/llm.py`.

### Design reasoning (why I did it)
It makes the API safer and easier to explain: “this is the expected shape, and validation happens automatically.”

### Possible improvement (1–2 lines)
I’d add stricter validation rules for some fields (like max lengths) to prevent huge payloads.

### Hard Terms Explained Simply
- `Schema` = the expected structure of data.
- `Validation` = checking data is correct before using it.

---

## 7) What is middleware?

### Short intro
Middleware is code that runs “around” requests. It can add headers, handle CORS, logging, auth patterns, etc.

### Step-by-step explanation
1. In `Documents/docbuilder/backend/main.py`, I add CORS middleware with `app.add_middleware(CORSMiddleware, ...)`.
2. This controls which frontend origins can call the backend, and which headers are allowed.
3. It also exposes `Content-Disposition` so the browser can download exported files cleanly (important for DOCX/PPTX export).

### Design reasoning (why I did it)
Without CORS middleware, the browser would block calls from the frontend app to the backend API.

### Possible improvement (1–2 lines)
I’d lock down CORS to exact production domains only, and separate dev vs prod configs more clearly.

### Hard Terms Explained Simply
- `CORS` = browser security rule for cross-site API calls.
- `Middleware` = a wrapper that runs before/after your route handler.
