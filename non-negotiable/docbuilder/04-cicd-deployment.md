# DocBuilder (Non-Negotiable) - CI/CD & Deployment

These answers are based on the actual DocBuilder implementation, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) Explain your CI/CD pipeline.

### Short intro
My CI/CD setup is basic but useful. The main thing I automated is backend testing on every push and pull request to the main branch. That helps catch issues early before deployment.

### Step-by-step explanation
1. Whenever code is pushed or a pull request is opened for the main branch, GitHub Actions starts the workflow.
2. The backend job installs Python dependencies and runs the backend test suite.
3. Dummy environment variables are provided so tests can run without real production secrets.
4. There is also a frontend job setup, but some frontend build and test steps are still commented out.
5. So in its current state, the pipeline mainly protects backend quality.

### Design reasoning
I focused first on backend CI because the backend handles the most critical parts of the product: auth, data, AI generation, and export. If that breaks, the product is basically unusable.

### Possible improvement
I would fully enable frontend checks and add linting, type-checking, and deployment gates before release.

### Hard Terms Explained Simply
- `CI` = automatically checking code when changes are pushed.
- `CD` = automatically deploying or preparing deployment after code passes checks.
- `Deployment gate` = a rule that blocks release if required checks fail.

---

## 2) What is stateless backend?

### Short intro
A stateless backend means the server does not keep user-specific session data in its own memory between requests. Every request brings what it needs, and long-term data is stored in external systems like the database.

### Step-by-step explanation
1. In DocBuilder, each request includes an auth token.
2. The backend verifies that token for that request and decides whether the user is allowed.
3. Project data is not stored inside the running server. It is stored in Firestore.
4. Export files are generated when needed and sent back immediately, rather than being stored as ongoing server sessions.
5. This means any backend instance can usually handle any valid request, which makes deployment easier.

### Design reasoning
Stateless design is easier to scale and simpler to reason about in cloud environments. It reduces dependency on one specific server instance.

### Possible improvement
If I want shared caching or job coordination, I would move that state into proper shared infrastructure like Redis or a job store.

### Hard Terms Explained Simply
- `Stateless` = the server does not remember your personal session data between requests.
- `Instance` = one running copy of the backend service.

---

## 3) How did you deploy to Railway?

### Short intro
I prepared the backend to run on Railway using a standard FastAPI production command. The deployment platform installs dependencies, then starts the API server.

### Step-by-step explanation
1. The deployment config tells Railway how to build the backend environment.
2. It installs the Python requirements needed by the app.
3. Then it starts the FastAPI server using Uvicorn.
4. The app listens on the port provided by the platform.
5. The repo also includes another deployment config for Render, which shows the backend was designed to be portable across hosting providers.

### Design reasoning
I kept deployment simple and standard. FastAPI plus Uvicorn is a common production setup, and platform configs make it easier to repeat deployment consistently.

### Possible improvement
I would clearly document which deployment platform is the primary one so there is less confusion between multiple config files.

### Hard Terms Explained Simply
- `Portable` = easy to move and run on different platforms.
- `Uvicorn` = the server that runs the FastAPI application.

---

## 4) How did you manage environment variables?

### Short intro
I used environment variables for configuration and secrets so that sensitive values are not hardcoded in the project.

### Step-by-step explanation
1. Local development can load values from a `.env` file.
2. In deployment, the platform provides those values securely.
3. This includes API keys, search configuration, and cloud credential settings.
4. The backend reads those values to decide how to connect to Firestore, whether web search is available, and which LLM provider should be used.
5. This keeps the same codebase flexible across development, testing, and production environments.

### Design reasoning
Hardcoding secrets is unsafe and hard to manage. Environment variables are the standard way to keep configuration separate from code.

### Possible improvement
I would centralize all config loading in one module so validation and defaults are handled in one place.

### Hard Terms Explained Simply
- `Environment variable` = a value given to the app from outside the code.
- `Hardcoded` = directly written into the code, which is risky for secrets.
- `Credential` = a secret or key used to access a service.

---

## 5) What tests did you write?

### Short intro
I wrote backend tests for the main user flow: basic API behavior, content generation flow, refinement flow, and export generation.

### Step-by-step explanation
1. I tested basic API behavior such as health checks and protected route behavior.
2. I tested project-related flows like creating and listing projects.
3. I tested generation-related flows using mocked AI behavior so the tests stay stable and do not depend on a real external model.
4. I tested refinement-related logic, along with comments and reactions.
5. I also tested export generation to make sure DOCX and PPTX files are produced successfully.
6. These tests protect the most important path a user takes through the product.

### Design reasoning
I focused on the paths that matter most in interviews and in real usage: can a user create content, improve it, and export it successfully?

### Possible improvement
I would add more integration tests using emulators or test environments so the system is verified in a setup closer to production.

### Hard Terms Explained Simply
- `Mocked AI behavior` = fake AI responses used in tests so results stay predictable.
- `Integration test` = a test that checks multiple parts of the system working together.
- `Protected route` = an API endpoint that only logged-in users are allowed to access.
