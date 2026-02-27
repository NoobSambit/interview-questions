# DocBuilder (Non‑Negotiable) — RAG System

These answers are written strictly from what exists in the repo: `Documents/docbuilder/`.

Relevant code I am referencing:
1. RAG retriever: `Documents/docbuilder/backend/app/core/rag.py`
2. LLM adapter + prompting: `Documents/docbuilder/backend/app/core/llm.py`
3. API route that triggers generation (and can enable RAG): `Documents/docbuilder/backend/app/api/endpoints.py`

---

## 1) Explain your RAG pipeline step-by-step.

### Short intro
In DocBuilder, RAG is an optional mode during section generation. When enabled, the backend does a web search, downloads a few pages, chunks them, embeds the chunks, and then feeds the best chunks into the LLM prompt as “web research context”.

### Step-by-step explanation
1. The user triggers section generation via `POST /projects/{project_id}/generate`, and can pass `use_rag` in the request body (see `GenerateContentRequest` in `Documents/docbuilder/backend/app/models.py`).
2. The route calls `adapter.generate_section(..., use_rag=request.use_rag or False)` in `Documents/docbuilder/backend/app/api/endpoints.py`.
3. If `use_rag=True`, the Groq adapter in `Documents/docbuilder/backend/app/core/llm.py` calls the retriever: `retriever.get_relevant_context(section_title, topic, doc_type, top_k=5)`.
4. Inside `Documents/docbuilder/backend/app/core/rag.py`, the retriever builds a search query (`formulate_search_query()`), runs Google Search (only if `GOOGLE_API_KEY` and `GOOGLE_CSE_ID` exist), then downloads the top pages with `requests.get(...)`.
5. It cleans the HTML using BeautifulSoup (it removes things like `script/style/nav/footer/header`) and limits the extracted text (about `5000` characters per page).
6. It chunks the text using `RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=100)`.
7. It embeds chunks using `HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2", device="cpu")`.
8. It builds an in-memory FAISS index and runs `similarity_search(f"{section_title} {topic}", k=top_k)` to pick the most relevant chunks.
9. It returns a combined context string plus source metadata (URL + title).
10. The LLM prompt injects this as `WEB RESEARCH CONTEXT` in `Documents/docbuilder/backend/app/core/llm.py` and tells the model to synthesize instead of copying verbatim.

Important fallback behavior (still part of the real pipeline):
1. If Google credentials are missing, `search_enabled` becomes false and RAG returns a mock `Document(...)` instead of real web pages.
2. If Google search fails, it returns “fallback mock content” so generation doesn’t crash.

### Design reasoning (why I did it)
1. It’s optional (`use_rag`) so normal generation can stay cheap and fast.
2. It keeps the app “more grounded” for factual topics by supplying outside context.
3. Using local HuggingFace embeddings avoids extra paid API calls for embeddings.

### Possible improvement (1–2 lines)
I would persist the vector store per project (instead of building it each time) and add better source filtering to reduce low-quality web pages.

### Hard Terms Explained Simply
- `RAG` = search first, then ask the LLM using the search results as context.
- `Chunking` = splitting big text into smaller pieces so search works better.

---

## 2) What is vector embedding?

### Short intro
An embedding is a way to convert text into numbers so we can “compare meaning” using math.

### Step-by-step explanation
1. In my RAG code, each chunk of text becomes a vector using `HuggingFaceEmbeddings`.
2. Similar meaning chunks get vectors that are close to each other.
3. Then I can search “which chunk is closest to my question/section title”.

Where it happens in my project:
1. `Documents/docbuilder/backend/app/core/rag.py` initializes embeddings with `sentence-transformers/all-MiniLM-L6-v2`.
2. It then builds FAISS index from those embeddings.

### Design reasoning (why I did it)
Keyword search alone misses semantically relevant text. Embeddings help find content that “means the same thing” even if wording differs.

### Possible improvement (1–2 lines)
I would add an option to switch embedding models and benchmark accuracy vs speed.

### Hard Terms Explained Simply
- `Vector` = a list of numbers that represents text.
- `Semantic` = meaning-based (not exact word matching).

---

## 3) What is cosine similarity?

### Short intro
Cosine similarity is one common way to measure how close two vectors are (how similar two texts are).

### Step-by-step explanation
1. Each text chunk has an embedding vector.
2. The query (section title + topic) also becomes a vector.
3. Cosine similarity gives a score for closeness.
4. Higher score means “more related”.

In my code:
1. I don’t manually compute cosine similarity.
2. I call `FAISS.similarity_search(...)` and FAISS handles the similarity scoring internally.
3. This is in `Documents/docbuilder/backend/app/core/rag.py`.

### Design reasoning (why I did it)
I use the standard vector-search approach that LangChain + FAISS provide, instead of writing math code myself.

### Possible improvement (1–2 lines)
I would log similarity scores and add a minimum threshold so irrelevant chunks are filtered out.

### Hard Terms Explained Simply
- `Similarity score` = number that tells “how close these texts are”.
- `Threshold` = a cut-off value, like “only accept score above X”.

---

## 4) What is FAISS?

### Short intro
FAISS is a library for fast similarity search on vectors. In my project it acts like a quick “vector database” in memory.

### Step-by-step explanation
1. I create a list of chunk documents.
2. I embed them into vectors.
3. I build an index: `FAISS.from_documents(chunks, embeddings)`.
4. I query it: `vectorstore.similarity_search(query, k=...)`.
5. It returns the most relevant chunks.

Where it happens:
1. `Documents/docbuilder/backend/app/core/rag.py` imports `FAISS` from `langchain_community.vectorstores`.

### Design reasoning (why I did it)
It’s simple and local. For my project size, I don’t need a hosted vector DB to get RAG working.

### Possible improvement (1–2 lines)
For large scale, I’d move to a persistent vector database so I don’t rebuild the index every request.

### Hard Terms Explained Simply
- `Index` = a data structure that makes searching fast.
- `Vector DB` = storage optimized to search embeddings.

---

## 5) How does similarity search work?

### Short intro
Similarity search means: “find the text chunks whose embeddings are closest to my query embedding.”

### Step-by-step explanation
1. Split pages into chunks.
2. Convert each chunk into an embedding.
3. Convert query into an embedding.
4. Search the index for the closest vectors.
5. Return top K chunks.
6. Use those chunks in the LLM prompt.

In my code:
1. The key call is `vectorstore.similarity_search(f"{section_title} {topic}", k=...)` in `Documents/docbuilder/backend/app/core/rag.py`.

### Design reasoning (why I did it)
It gives context that is actually relevant to the section title, not just random web text.

### Possible improvement (1–2 lines)
I’d add deduplication across sources and limit chunks per domain so one website doesn’t dominate the context.

### Hard Terms Explained Simply
- `Top K` = the best K results (like top 5).
- `Query` = the text you are searching for.

---

## 6) What is chunking?

### Short intro
Chunking is splitting big web pages into smaller pieces so embeddings and retrieval become accurate and fast.

### Step-by-step explanation
1. Web pages can be very long.
2. If I embed the full page as one vector, it becomes too broad and noisy.
3. So I split into chunks of about 800 characters.
4. I also overlap by 100 characters so important sentences near boundaries aren’t lost.

In my code:
1. `RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=100)` in `Documents/docbuilder/backend/app/core/rag.py`.

### Design reasoning (why I did it)
Small chunks make retrieval more precise because the “meaning vector” represents a tighter topic.

### Possible improvement (1–2 lines)
I would experiment with chunking by headings (HTML `<h1>/<h2>`) instead of pure character splitting.

### Hard Terms Explained Simply
- `Overlap` = repeating a small part between chunks so context is continuous.
- `Splitter` = the tool that breaks text into chunks.

---

## 7) What is sliding window context?

### Short intro
In my implementation, “sliding window” basically shows up as overlapping chunks. I don’t have a separate explicit sliding window feature beyond `chunk_overlap`.

### Step-by-step explanation
1. When I chunk text, I do `chunk_overlap=100`.
2. That means chunk N and chunk N+1 share some text.
3. So the retriever can still capture a point even if it sits between two chunks.

Where it happens:
1. Chunk overlap is in `Documents/docbuilder/backend/app/core/rag.py`.

### Design reasoning (why I did it)
It’s a simple way to preserve continuity without complex logic.

### Possible improvement (1–2 lines)
I’d tune overlap based on document type: higher overlap for technical topics, lower overlap for general topics.

### Hard Terms Explained Simply
- `Sliding window` = moving a window over text so each piece shares some context.
- `Overlap` = shared text between pieces.

---

## 8) How did you reduce hallucination?

### Short intro
I reduce hallucination mainly by grounding the LLM with retrieved web context (RAG) and forcing structured outputs using Pydantic parsing.

### Step-by-step explanation
1. Grounding via RAG: when `use_rag=True`, I inject “WEB RESEARCH CONTEXT” into the prompt (see `Documents/docbuilder/backend/app/core/llm.py`). That context is built from web pages (when keys are configured) in `Documents/docbuilder/backend/app/core/rag.py`.
2. Structured outputs: I use `PydanticOutputParser` so the LLM must return a predictable structure based on schemas in `Documents/docbuilder/backend/app/models.py`. This reduces format failures and random output.
3. Safety fallback: if web search fails, the retriever returns mock fallback content instead of crashing. That keeps UX stable, but it does not guarantee factual accuracy.

Important honesty point (from the code reality):
1. RAG reduces hallucination, but it does not “eliminate” it. The model can still make mistakes, especially if the retrieved sources are weak.

### Design reasoning (why I did it)
For a doc builder, factual grounding is important. RAG is the simplest working step-up over “pure prompt” generation.

### Possible improvement (1–2 lines)
I’d add citations into the generated content (link each claim to a source URL) and add a “fact-check pass” for high-risk topics.

### Hard Terms Explained Simply
- `Hallucination` = when an LLM says something confidently but it’s wrong.
- `Grounding` = giving the model real reference text so it stays accurate.
