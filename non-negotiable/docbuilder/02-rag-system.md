# DocBuilder (Non-Negotiable) - RAG System

These answers are based on the actual DocBuilder implementation, but rewritten in simple interview language so they are easier to remember and explain.

---

## 1) Explain your RAG pipeline step-by-step.

### Short intro
In DocBuilder, RAG is an optional feature used while generating a section. If the user turns it on, the backend first gathers outside information from the web, then gives that information to the AI so the answer is more grounded.

### Step-by-step explanation
1. The user starts section generation and chooses whether research support should be used.
2. If RAG is enabled, the backend creates a search query from the topic and section title.
3. It searches the web for relevant pages.
4. It downloads a few of those pages and cleans the raw HTML so only useful text remains.
5. That text is split into smaller chunks because large pages are too broad to search well.
6. Each chunk is converted into an embedding, which is a numeric representation of meaning.
7. The system compares the query with those chunk embeddings and keeps the most relevant chunks.
8. The best chunks are added into the prompt as research context.
9. Then the LLM generates the section using both the user request and the retrieved research.
10. If search is not configured or fails, the app falls back safely so generation still continues.

### Design reasoning
I made RAG optional because not every section needs outside research. That keeps normal generation faster and cheaper, while still allowing more factual support when needed.

### Possible improvement
I would store reusable research data instead of rebuilding it every time, and I would add better filtering so weak websites are less likely to influence the answer.

### Hard Terms Explained Simply
- `RAG` = retrieve information first, then generate the answer using that information.
- `Grounded` = based on real supporting information, not only on the model's memory.
- `Embedding` = a numeric form of text that helps the system compare meaning.

---

## 2) What is vector embedding?

### Short intro
A vector embedding is a way to convert text into numbers so the system can compare meaning using math instead of exact word matching.

### Step-by-step explanation
1. A sentence or paragraph is turned into a long list of numbers.
2. That list captures the meaning of the text in a machine-friendly form.
3. Texts with similar meaning usually end up closer to each other in that numeric space.
4. So when the user asks about a topic, the system can find chunks that are semantically close even if they do not use the exact same words.
5. This is why embeddings are useful for retrieval in RAG.

### Design reasoning
I used embeddings because keyword matching alone is too shallow. It can miss useful content when the wording changes but the meaning is still relevant.

### Possible improvement
I would compare different embedding models and choose the best one based on speed, quality, and cost.

### Hard Terms Explained Simply
- `Vector` = a list of numbers.
- `Semantic` = related to meaning, not just exact words.

---

## 3) What is cosine similarity?

### Short intro
Cosine similarity is a way to measure how close two embeddings are. In simple terms, it tells us how similar two pieces of text are in meaning.

### Step-by-step explanation
1. The query is converted into an embedding.
2. Each text chunk is also converted into an embedding.
3. The system compares the direction of those vectors.
4. If the vectors point in a similar direction, the texts are considered similar.
5. A higher similarity score means the chunk is a better match for the query.

### Design reasoning
I rely on the similarity search tools provided by the vector library instead of writing the math manually. That keeps the code simpler and uses a standard retrieval approach.

### Possible improvement
I would log similarity scores and ignore chunks below a minimum quality threshold.

### Hard Terms Explained Simply
- `Similarity score` = a number that shows how closely two texts match.
- `Threshold` = the minimum score required before we accept something.

---

## 4) What is FAISS?

### Short intro
FAISS is a library that helps search through embeddings quickly. In DocBuilder, it works like a lightweight in-memory vector search engine.

### Step-by-step explanation
1. First, I collect text chunks from the retrieved web pages.
2. Then I create embeddings for those chunks.
3. FAISS builds an index over those embeddings.
4. When I search with the query embedding, FAISS quickly returns the closest matches.
5. Those matches become the context sent to the LLM.

### Design reasoning
I chose FAISS because it is simple, fast, and good enough for a project where the index is created during the request. I did not need a full hosted vector database for this stage.

### Possible improvement
If the product scales, I would move to a persistent vector store so retrieval is faster and reusable across requests.

### Hard Terms Explained Simply
- `Index` = a structure that makes searching faster.
- `Vector search engine` = a tool that searches based on meaning represented by embeddings.

---

## 5) How does similarity search work?

### Short intro
Similarity search means finding the chunks whose meaning is closest to the user's topic.

### Step-by-step explanation
1. Split the collected research text into smaller chunks.
2. Turn each chunk into an embedding.
3. Turn the user query into an embedding too.
4. Compare the query embedding with all chunk embeddings.
5. Keep the top few chunks that are most similar.
6. Use those chunks as context for generation.

### Design reasoning
This approach is much better than sending all collected text to the model. It keeps only the parts that are most likely to help with the specific section being generated.

### Possible improvement
I would add deduplication so very similar chunks do not all take up prompt space.

### Hard Terms Explained Simply
- `Top few chunks` = the best matching pieces of text.
- `Prompt space` = the amount of text we can safely send to the model in one request.

---

## 6) What is chunking?

### Short intro
Chunking means breaking large text into smaller pieces so retrieval becomes more accurate and manageable.

### Step-by-step explanation
1. Web pages are often too large and contain many different ideas.
2. If I treat the whole page as one unit, the meaning becomes too broad.
3. So I split the page into smaller chunks.
4. Each chunk is easier to embed and compare with the query.
5. This improves retrieval quality because the system can match a specific part of a page instead of the whole page.

### Design reasoning
I used chunking because retrieval works best when each piece of text represents a focused idea instead of a very large mixed document.

### Possible improvement
I would try smarter chunking based on headings or section structure, not only raw length.

### Hard Terms Explained Simply
- `Chunk` = a smaller piece of a larger document.
- `Focused idea` = one clear topic instead of many mixed topics.

---

## 7) What is sliding window context?

### Short intro
In this project, sliding window behavior mainly appears through overlapping chunks. It means nearby chunks share a little text so important information is not lost at the boundary.

### Step-by-step explanation
1. When text is split into chunks, a sentence might land right at the end of one chunk.
2. If there is no overlap, that sentence may lose context.
3. So I repeat a small portion of text between neighboring chunks.
4. That way, ideas near the boundary still appear in more than one chunk.
5. This improves the chance that retrieval keeps the right context.

### Design reasoning
I used overlap because it is a simple and practical way to preserve continuity without adding complicated logic.

### Possible improvement
I would tune overlap size based on content type, because some topics need more context continuity than others.

### Hard Terms Explained Simply
- `Boundary` = the point where one chunk ends and the next starts.
- `Continuity` = keeping the meaning connected across chunks.

---

## 8) How did you reduce hallucination?

### Short intro
I reduced hallucination mainly in two ways: by giving the model outside research context through RAG, and by forcing outputs into a structured format. That does not make hallucination impossible, but it reduces it.

### Step-by-step explanation
1. When RAG is enabled, the model gets research context related to the topic instead of relying only on its internal memory.
2. That makes it more likely to stay close to real information.
3. I also use structured outputs, so the model is guided to answer in a predictable format instead of wandering into random text.
4. This reduces both factual drift and formatting problems.
5. Still, if the source quality is poor, the model can produce weak or incorrect content, so RAG lowers hallucination risk but does not fully remove it.

### Design reasoning
For a document generation product, factual quality matters. Pure prompting was not enough, so grounding the model with retrieved context was the practical next step.

### Possible improvement
I would add citations in the output and possibly a second verification pass for topics where factual accuracy matters a lot.

### Hard Terms Explained Simply
- `Hallucination` = when the AI says something confidently that is wrong or unsupported.
- `Structured format` = forcing the answer into a clear shape instead of fully free text.
- `Citation` = showing which source supports a claim.
