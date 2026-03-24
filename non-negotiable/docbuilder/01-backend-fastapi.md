# DocBuilder (Non-Negotiable) - Backend & FastAPI

These answers are based on the actual DocBuilder implementation, but written in simple interview language so they are easier to remember and speak.

---

## 1) Why FastAPI over Express?

### Short intro
I chose FastAPI because most of the heavy backend work in DocBuilder is naturally better in Python. The project is not just serving simple APIs. It is also generating Word and PowerPoint files, handling AI flows, and doing RAG-related processing. Python already has strong libraries for all of that, so FastAPI was the practical choice.

### Step-by-step explanation
1. The main job of the backend is more than basic CRUD. It has to create structured documents, call LLM workflows, and sometimes fetch research content from the web.
2. For export, Python has mature libraries for DOCX and PPTX generation. That made document creation much easier and more stable.
3. For AI and RAG, Python also has strong support through tools like LangChain, embeddings, and vector search libraries.
4. FastAPI fits well here because it gives clean API routing, request validation, async support, and automatic API docs.
5. So instead of mixing multiple backend languages, I kept all heavy backend logic in one Python service.

### Design reasoning
I optimized for speed of development and technical fit. If I had used Express, I would either need to rebuild some export logic in Node.js or keep a second Python service anyway. That would increase complexity without giving much benefit.

### Possible improvement
If the product grows, I would keep FastAPI as the main API layer but move slow work like export and heavy generation into background workers.

### Hard Terms Explained Simply
- `CRUD` = create, read, update, delete operations on data.
- `RAG` = first collect useful external information, then give that information to the AI before it answers.
- `API docs` = an automatically generated page that shows what endpoints exist and how to call them.

---

## 2) What is async/await?

### Short intro
`async/await` is a way to write code that can wait for slow tasks without freezing the whole server. It helps the backend stay available while one request is waiting on a database, network call, or external service.

### Step-by-step explanation
1. In a web backend, many tasks are not slow because of computation. They are slow because the server is waiting on something else, like a database response or an API call.
2. With `async/await`, the request can pause at that waiting point.
3. While it is paused, the server can use that time to handle another request.
4. This is useful in DocBuilder because requests may involve auth verification, database operations, and AI-related calls.
5. So async code improves how well the backend handles multiple users at the same time.

### Design reasoning
I used the async style because FastAPI is built around it, and it is a good fit for APIs that spend a lot of time waiting on I/O operations.

### Possible improvement
Async helps with waiting, but it does not solve heavy CPU work. For very slow exports or generation tasks, I would move them to a queue system.

### Hard Terms Explained Simply
- `I/O` = operations where the server waits for outside systems, like a database, file, or network response.
- `Freeze the server` = one slow request stops other requests from being handled efficiently.

---

## 3) What is event loop?

### Short intro
The event loop is the part of the async system that keeps track of waiting tasks and decides what should run next. You can think of it like a smart scheduler for backend work.

### Step-by-step explanation
1. When requests come to an async backend, not every request runs from start to finish without pause.
2. Some requests stop for a moment because they are waiting on I/O.
3. The event loop notices that and gives time to another request that is ready to run.
4. When the first request gets its response, the event loop brings it back and continues execution.
5. This is how one server can handle many in-progress requests efficiently without creating a separate thread for every small wait.

### Design reasoning
This model is useful for DocBuilder because many operations involve waiting on other systems. The event loop helps use server time better.

### Possible improvement
If traffic becomes high, I would combine async event-loop handling with multiple worker processes and background jobs.

### Hard Terms Explained Simply
- `Scheduler` = something that decides which task should run now and which can wait.
- `Thread` = one path of execution in a program. Too many threads can become expensive.

---

## 4) What is ASGI?

### Short intro
ASGI is the standard that lets an async Python web app talk to the server that runs it. In simple words, it is the interface between FastAPI and the web server.

### Step-by-step explanation
1. FastAPI is the application framework where I define routes and backend logic.
2. But FastAPI itself does not directly listen for internet traffic.
3. A server like Uvicorn runs the app and accepts requests.
4. ASGI is the common format both sides understand, so the server knows how to pass requests into the app and return responses back out.
5. This is especially important for async applications because older standards were built more for synchronous behavior.

### Design reasoning
I used FastAPI with an ASGI server because that is the standard modern setup for async Python APIs.

### Possible improvement
If I later add real-time features like live progress updates, this async-friendly setup already supports that direction well.

### Hard Terms Explained Simply
- `ASGI` = the communication standard between an async Python app and the server running it.
- `Uvicorn` = the program that runs the FastAPI app.

---

## 5) How does FastAPI handle concurrency?

### Short intro
FastAPI handles concurrency mainly by using async request handling. That means many requests can be in progress at the same time, especially when they spend time waiting on external operations.

### Step-by-step explanation
1. A request comes into the backend.
2. If that request needs to wait on auth, database access, or a network call, it does not need to block the entire server.
3. The backend can move to another request while the first one is waiting.
4. This makes the server feel more responsive under normal traffic.
5. In DocBuilder, this matters because requests may involve user verification, project reads and writes, and sometimes AI-related or research-related work.

### Design reasoning
This model is a good fit because the backend does many wait-heavy operations. FastAPI gives a clean way to handle that without building complex concurrency logic manually.

### Possible improvement
For very slow requests, I would not rely only on concurrency inside the API. I would move long-running work to background jobs and return job status to the frontend.

### Hard Terms Explained Simply
- `Concurrency` = handling multiple requests in overlapping time.
- `Responsive` = the app still feels quick and usable even when many requests are happening.

---

## 6) What is Pydantic?

### Short intro
Pydantic is the tool I use to define what data should look like. It makes sure incoming requests and structured outputs follow the format I expect.

### Step-by-step explanation
1. In DocBuilder, the backend receives structured data like project creation requests, generation requests, and refine requests.
2. Instead of checking each field manually, I define a model for the expected input.
3. When a request comes in, Pydantic validates that the fields are present and in the right type.
4. That reduces bugs and makes the API easier to trust.
5. I also use the same idea for AI outputs, so the model is pushed to return content in a predictable structure instead of random free-form text.

### Design reasoning
I used Pydantic because it keeps the API clean, reduces manual validation code, and makes both backend logic and AI flows more reliable.

### Possible improvement
I would add stricter validation for things like text length, allowed values, and edge-case handling.

### Hard Terms Explained Simply
- `Validation` = checking that incoming data is correct before using it.
- `Model` = a defined structure that tells the backend what fields should exist.

---

## 7) What is middleware?

### Short intro
Middleware is code that runs before or after the main route logic. It is useful for handling common backend concerns in one place.

### Step-by-step explanation
1. When a request enters the backend, it does not always go directly to the route function.
2. Middleware can first inspect or modify the request or response.
3. In DocBuilder, an important example is CORS handling, because the frontend and backend may run on different origins.
4. Middleware can also be used for logging, security headers, request tracking, and other cross-cutting concerns.
5. This keeps route code focused on business logic instead of repeated setup work.

### Design reasoning
I used middleware because some concerns apply to almost every request, and middleware is the cleanest place to handle them.

### Possible improvement
I would keep production and development middleware settings separate, especially for stricter CORS and security rules.

### Hard Terms Explained Simply
- `CORS` = browser security rule that controls whether one website can call another server.
- `Cross-cutting concerns` = common tasks like logging or security that affect many routes, not just one.
