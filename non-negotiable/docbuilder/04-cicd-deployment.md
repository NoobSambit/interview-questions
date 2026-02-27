# DocBuilder (Non‑Negotiable) — CI/CD & Deployment

These answers are written strictly from what exists in the repo: `Documents/docbuilder/`.

Relevant code/config I am referencing:
1. GitHub Actions workflow: `Documents/docbuilder/.github/workflows/ci.yml`
2. Render config: `Documents/docbuilder/render.yaml`
3. Railway config: `Documents/docbuilder/backend/railway.toml`
4. Backend server entry: `Documents/docbuilder/backend/main.py`
5. Env handling: `Documents/docbuilder/backend/app/db/firestore.py`, `Documents/docbuilder/backend/app/core/llm.py`, `Documents/docbuilder/backend/app/core/rag.py`
6. Tests folder: `Documents/docbuilder/backend/tests/test_main.py`, `Documents/docbuilder/backend/tests/test_export.py`, `Documents/docbuilder/backend/tests/test_refinement.py`

---

## 1) Explain your CI/CD pipeline.

### Short intro
I have a GitHub Actions pipeline that runs backend tests on every push and PR to `main`. Frontend CI steps exist but the build/test steps are currently commented out.

### Step-by-step explanation
1. The workflow triggers on `push` and `pull_request` to `main` (see `Documents/docbuilder/.github/workflows/ci.yml`).
2. The `backend-test` job checks out code, sets up Python 3.9, installs `backend/requirements.txt`, exports dummy env vars (`GOOGLE_API_KEY`, `LLM_PROVIDER=mock`, `FIREBASE_CREDENTIALS`), and runs `pytest tests/` inside `backend`.
3. The `frontend-test` job checks out code, sets up Node 18, installs frontend deps, and the build/test steps are currently commented out.

### Design reasoning (why I did it)
For a project like this, backend stability is the most important because it handles auth, data, LLM calls, and export generation. So I ensured backend tests run automatically first.

### Possible improvement (1–2 lines)
I would enable frontend build + tests in CI, and add lint/typecheck steps for both backend and frontend.

### Hard Terms Explained Simply
- `CI` = automatically run tests on code changes.
- `CD` = automatically deploy when code is merged (not fully shown in this repo).

---

## 2) What is stateless backend?

### Short intro
A stateless backend means the server doesn’t keep user session state in memory between requests. Each request has everything needed (like the auth token), and data is stored in the database.

### Step-by-step explanation
1. Auth is bearer-token based: each request includes `Authorization: Bearer <token>`, and the backend verifies it in `Documents/docbuilder/backend/app/core/auth.py`.
2. Project data is stored in Firestore, and the backend fetches/updates documents in the `projects` collection (see `Documents/docbuilder/backend/app/api/endpoints.py`).
3. Exports are generated on-demand and streamed back. The server doesn’t store “export sessions”; it just generates and returns the file (see `StreamingResponse` in `Documents/docbuilder/backend/app/api/endpoints.py`).

Important implementation detail:
1. There is also a local `.langchain_cache.db` file used for caching LLM calls (`Documents/docbuilder/backend/app/core/llm.py`).
2. That is “local instance state” and may not persist across deployments/instances, so I treat it as an optimization, not a required state.

### Design reasoning (why I did it)
Stateless designs are easier to deploy and scale because any server instance can handle any request.

### Possible improvement (1–2 lines)
If I want shared caching, I’d move cache to Redis so it’s consistent across instances.

### Hard Terms Explained Simply
- `Stateless` = server does not remember you between requests.
- `Bearer token` = a token you send with every request to prove identity.

---

## 3) How did you deploy to Railway?

### Short intro
The repo includes a Railway deployment config for the backend. It runs uvicorn with the FastAPI app.

### Step-by-step explanation
1. Railway config is in `Documents/docbuilder/backend/railway.toml`.
2. It installs dependencies using `pip install -r requirements.txt` (Nixpacks build).
3. It starts the server with `uvicorn main:app --host 0.0.0.0 --port $PORT`.
4. That matches the app entrypoint in `Documents/docbuilder/backend/main.py` where `app = FastAPI(...)` is defined.

Reality check from code/config:
1. The repo also contains a Render config at `Documents/docbuilder/render.yaml` with a similar uvicorn start command.
2. So the backend can be deployed in more than one way, depending on the platform.

### Design reasoning (why I did it)
Using uvicorn is the standard way to serve FastAPI in production. Railway/Render configs keep deployment reproducible.

### Possible improvement (1–2 lines)
I’d add a clear “single source of truth” deployment doc and remove unused provider configs to reduce confusion.

### Hard Terms Explained Simply
- `Uvicorn` = the server process that runs FastAPI.
- `Nixpacks` = a build system Railway uses to build your app automatically.

---

## 4) How did you manage environment variables?

### Short intro
I use environment variables for all secrets and provider configuration, and load local dev vars using `dotenv`.

### Step-by-step explanation
1. The backend loads `.env` in multiple places using `load_dotenv()` (see `Documents/docbuilder/backend/main.py`, `Documents/docbuilder/backend/app/core/llm.py`, `Documents/docbuilder/backend/app/core/rag.py`, `Documents/docbuilder/backend/app/db/firestore.py`).
2. Firestore credentials support `GOOGLE_SERVICE_ACCOUNT_JSON` (JSON string) or `GOOGLE_APPLICATION_CREDENTIALS` (file path), implemented in `Documents/docbuilder/backend/app/db/firestore.py`.
3. RAG uses `GOOGLE_API_KEY` and `GOOGLE_CSE_ID` in `Documents/docbuilder/backend/app/core/rag.py`.
4. LLM provider selection uses `LLM_PROVIDER`. Code truth: only `LLM_PROVIDER=groq` uses a real adapter; anything else falls back to `MockLLMAdapter` (see `Documents/docbuilder/backend/app/core/llm.py`).

### Design reasoning (why I did it)
Secrets should not be hardcoded. Env vars are the standard way to configure deployments safely.

### Possible improvement (1–2 lines)
I would centralize configuration into one module so env loading isn’t repeated in multiple files.

### Hard Terms Explained Simply
- `Env var` = a setting provided by the operating system for the app.
- `Secret` = keys/passwords that must not be committed to Git.

---

## 5) What tests did you write?

### Short intro
I wrote backend tests for core API flows (health, projects, generation) and for export generation. The CI workflow runs `pytest` on `backend/tests/`.

### Step-by-step explanation
1. API tests: `Documents/docbuilder/backend/tests/test_main.py` uses FastAPI `TestClient` and covers unauthorized access, creating/listing projects with mocked Firestore, outline suggestion (Mock LLM), and section generation (Mock LLM).
2. Export tests: `Documents/docbuilder/backend/tests/test_export.py` calls `ExportService.generate_docx(...)` and `ExportService.generate_pptx(...)` and checks the returned `BytesIO` is non-empty.
3. Refinement tests: `Documents/docbuilder/backend/tests/test_refinement.py` tests refine, comments, and reactions using mocks.

Practical note from repo reality:
1. Some tests rely on a `mock_token` bypass in auth (`Documents/docbuilder/backend/app/core/auth.py`).
2. That’s helpful for test speed, but for production I would guard or remove such bypasses.

### Design reasoning (why I did it)
I focused on tests that protect the “main user path”: create project, generate content, refine content, export.

### Possible improvement (1–2 lines)
I’d add integration tests that run with a Firestore emulator and verify real auth behavior (no mock token).

### Hard Terms Explained Simply
- `Mock` = fake object used in tests so we don’t call real services.
- `Integration test` = test that checks multiple parts work together, not just one function.
