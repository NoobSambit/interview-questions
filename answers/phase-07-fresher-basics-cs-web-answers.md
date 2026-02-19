# Phase 07 - Fresher Basics (CS + Web + AI) Answers

This file is for quick revision rounds.
Language is simple, but core technical terms are kept.

## A) DSA and Problem Solving

1. **Explain time and space complexity with examples from your projects.**
Answer: Time complexity is how runtime grows with input size; space complexity is extra memory growth. Example: linear scan over sections is `O(n)` time, while hash-map lookups for IDs are near `O(1)` average.

2. **Difference between array and linked list, and practical use cases.**
Answer: Arrays have contiguous memory and fast random access (`O(1)` index). Linked lists have easy insert/delete in middle when node pointer is known, but slower random access (`O(n)`).

3. **Stack vs queue and real backend examples.**
Answer: Stack is LIFO; queue is FIFO. Recursion/call stack uses stack, and background job processing usually uses queue.

4. **Hash map internals and collision handling basics.**
Answer: Hash map stores key-value pairs using hash function to pick bucket/index. Collisions are handled using chaining or open addressing.

5. **When do you choose BFS vs DFS?**
Answer: Use BFS when shortest path in unweighted graph is needed. Use DFS for deep traversal, cycle detection, or topological style exploration.

6. **Explain recursion and tail recursion with one example.**
Answer: Recursion means function calls itself on smaller subproblem. Tail recursion is when recursive call is last operation, so compiler/interpreter can optimize in some languages.

7. **What is dynamic programming and when does it apply?**
Answer: DP is solving overlapping subproblems with memoization/tabulation. Use it when brute force repeats same states many times.

8. **Difference between greedy and DP approaches.**
Answer: Greedy picks local best at each step and hopes global optimality. DP systematically explores states to guarantee optimal result when recurrence exists.

9. **Binary search prerequisites and common mistakes.**
Answer: Prerequisite: data must be sorted or monotonic predicate must exist. Common mistakes are wrong loop bounds and overflow/incorrect mid update.

10. **How do you design test cases for algorithmic edge conditions?**
Answer: Include empty input, single element, duplicates, max/min values, sorted/reverse-sorted cases, and invalid input. Also test boundary transitions.

11. **What is amortized complexity with an example?**
Answer: Amortized complexity is average cost per operation over sequence. Example: dynamic array append is mostly `O(1)` amortized even though occasional resize is expensive.

12. **Explain stable vs unstable sorting.**
Answer: Stable sort keeps relative order of equal keys; unstable may change it. Stability matters when sorting by multiple keys in passes.

13. **Quick sort vs merge sort tradeoffs.**
Answer: Quick sort is usually fast in practice with low constant factors but worst-case `O(n^2)` without care. Merge sort guarantees `O(n log n)` and is stable but uses extra memory.

14. **How do you optimize brute-force solutions step-by-step?**
Answer: First find repeated work, then add memoization/cache, then improve data structures, then prune search space. Measure after each step.

15. **How do you explain your thought process in a coding interview?**
Answer: State assumptions, discuss brute force first, explain tradeoff, then implement improved approach. Keep interviewer updated about edge cases and complexity.

## B) OOP and Software Engineering Basics

16. **Explain abstraction, encapsulation, inheritance, polymorphism.**
Answer: Abstraction hides unnecessary details, encapsulation bundles data+methods with access control, inheritance reuses base behavior, and polymorphism allows one interface with many implementations.

17. **Composition vs inheritance: when to use each.**
Answer: Prefer composition for flexibility and lower coupling. Use inheritance only when true “is-a” relationship is stable and behavior sharing is clear.

18. **What is SOLID and which principles have you applied?**
Answer: SOLID is five design principles: single responsibility, open-closed, Liskov substitution, interface segregation, dependency inversion. Adapter-based design is an example of open-closed and dependency inversion thinking.

19. **What is coupling and cohesion in code design?**
Answer: Coupling is dependency between modules; lower is better. Cohesion is how focused a module is on one job; higher is better.

20. **What are design patterns and where did you use adapter pattern?**
Answer: Design patterns are reusable solution templates for recurring problems. Adapter pattern is used when one interface hides multiple backend/provider implementations.

21. **What is clean architecture in practical terms for a fresher project?**
Answer: Keep business logic separate from framework details and external services. This makes testing easier and future changes safer.

22. **How do you design APIs for maintainability?**
Answer: Use clear resource naming, predictable status codes, consistent response shapes, and versioning strategy. Validate input strictly and avoid hidden side effects.

23. **What is idempotency and why does it matter for backend endpoints?**
Answer: Idempotency means repeated same request gives same final state. It prevents duplicate side effects in retries and network glitches.

24. **What is optimistic vs pessimistic concurrency control?**
Answer: Optimistic control allows parallel edits and checks version at commit; pessimistic locks early to block conflicts. Optimistic is common for web apps with lighter contention.

25. **How do you write useful code comments without clutter?**
Answer: Comment why, not what. Add comments around non-obvious logic, invariants, and important tradeoffs.

## C) DBMS and SQL Fundamentals

26. **Explain primary key, foreign key, unique key, composite key.**
Answer: Primary key uniquely identifies each row. Foreign key references another table’s key, unique key enforces uniqueness, and composite key uses multiple columns together.

27. **Difference between SQL and NoSQL; when to choose each.**
Answer: SQL is relational with strong schema and joins; NoSQL is flexible schema and document/key-value styles. Choose based on data shape, query patterns, and consistency needs.

28. **ACID properties and why transactions matter.**
Answer: ACID means Atomicity, Consistency, Isolation, Durability. Transactions prevent partial writes and keep data correct under failures/concurrency.

29. **What is normalization? Explain 1NF, 2NF, 3NF quickly.**
Answer: Normalization reduces redundancy and anomalies. 1NF: atomic columns, 2NF: no partial dependency on composite key, 3NF: no transitive dependency.

30. **What is denormalization and when is it useful?**
Answer: Denormalization duplicates some data for faster reads. It is useful in read-heavy systems where join cost is high.

31. **What are indexes and how do they improve query performance?**
Answer: Index is a data structure (often B-tree) that speeds up search/sort/filter. It avoids full table/collection scan for indexed queries.

32. **Why can too many indexes hurt performance?**
Answer: Every insert/update/delete must update indexes too, increasing write cost and storage. So index only what is truly queried often.

33. **Difference between clustered and non-clustered index (conceptual).**
Answer: Clustered index defines physical order of data rows (usually one). Non-clustered stores separate index structure pointing to row locations.

34. **What is query execution plan and why inspect it?**
Answer: Execution plan shows how DB will run query (scan vs index, join order, cost). It helps optimize slow queries scientifically.

35. **Explain `JOIN` types with examples.**
Answer: `INNER JOIN` returns matching rows in both tables. `LEFT JOIN` keeps all left rows and matches right when available; `RIGHT` is opposite; `FULL` keeps all rows from both sides.

36. **Difference between `WHERE` and `HAVING`.**
Answer: `WHERE` filters rows before aggregation. `HAVING` filters groups after aggregation.

37. **`COUNT(*)` vs `COUNT(column)` differences.**
Answer: `COUNT(*)` counts all rows. `COUNT(column)` counts only non-null values in that column.

38. **How do you prevent duplicate records in concurrent writes?**
Answer: Use unique constraints/indexes plus idempotency keys. Handle duplicate-key errors gracefully in application code.

39. **What is pagination and keyset vs offset pagination?**
Answer: Pagination splits large result set into pages. Offset pagination is simple but can be slow on deep pages; keyset/cursor pagination scales better and is more stable.

40. **How would you model users, projects, and sections in relational schema?**
Answer: `users` table, `projects` table with `owner_user_id` FK, and `sections` table with `project_id` FK plus `position`, `content`, `version`. Add indexes on owner/project/position.

## D) Operating Systems Basics

41. **Process vs thread.**
Answer: Process has separate memory space; thread shares process memory with other threads. Threads are lighter but require synchronization.

42. **What is context switching and why is it costly?**
Answer: Context switch is saving/loading execution state between tasks. It is costly due to CPU overhead and cache disruption.

43. **What is deadlock and its four necessary conditions?**
Answer: Deadlock is permanent waiting cycle. Four conditions: mutual exclusion, hold-and-wait, no preemption, circular wait.

44. **What is starvation vs deadlock?**
Answer: Starvation means a task waits too long because resources keep going to others. Deadlock means tasks wait forever in cycle.

45. **What is virtual memory and paging?**
Answer: Virtual memory gives each process a large logical address space. Paging maps virtual pages to physical frames using page tables.

46. **What is CPU scheduling and common algorithms?**
Answer: CPU scheduling decides which process/thread runs next. Common algorithms: FCFS, SJF, Round Robin, Priority scheduling.

47. **What is race condition and how do you avoid it?**
Answer: Race condition happens when result depends on timing of concurrent operations. Avoid with atomic operations, locks, transactions, and proper isolation.

48. **What is mutex vs semaphore?**
Answer: Mutex is a mutual exclusion lock for one owner at a time. Semaphore is a counter-based sync primitive allowing limited concurrent access.

49. **What is producer-consumer problem?**
Answer: Producer creates items, consumer processes items, and shared buffer must be synchronized. Semaphores/locks/condition variables solve it.

50. **How does OS understanding help in backend performance debugging?**
Answer: It helps explain CPU saturation, context-switch overhead, memory pressure, IO wait, and thread contention. So you can diagnose root cause faster.

## E) Computer Networks and HTTP Basics

51. **OSI vs TCP/IP model basics.**
Answer: OSI has 7 conceptual layers; TCP/IP model is practical 4-layer stack used on internet. They map roughly but are not one-to-one.

52. **What happens when you type a URL in browser?**
Answer: Browser resolves DNS, opens TCP/TLS connection, sends HTTP request, receives response, then renders HTML/CSS/JS. Additional asset requests follow.

53. **TCP vs UDP.**
Answer: TCP is connection-oriented, reliable, ordered delivery. UDP is connectionless, faster, no delivery guarantee.

54. **What is DNS and DNS caching?**
Answer: DNS translates domain names to IP addresses. Caching stores recent lookups to reduce latency and resolver load.

55. **What is TLS handshake at high level?**
Answer: Client and server negotiate cryptographic parameters, authenticate certificate, and derive session keys. Then application data is encrypted.

56. **HTTP methods and idempotency (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`).**
Answer: `GET` is read and idempotent; `PUT` and `DELETE` are usually idempotent; `POST` usually is not; `PATCH` can be non-idempotent depending on implementation.

57. **Common HTTP status codes and when to use 400 vs 401 vs 403 vs 404 vs 500.**
Answer: `400` bad input, `401` unauthenticated, `403` authenticated but forbidden, `404` resource not found, `500` server-side unexpected error.

58. **What is statelessness in REST APIs?**
Answer: Each request contains all required context; server does not rely on per-client session memory for correctness. It improves scalability.

59. **What is CORS and why does it exist?**
Answer: CORS is browser security policy controlling cross-origin requests. It protects users from unauthorized cross-site API access.

60. **Cookies vs localStorage vs sessionStorage.**
Answer: Cookies are sent with HTTP requests and useful for session patterns. `localStorage` is persistent client-side key-value storage; `sessionStorage` lasts per tab session.

61. **What is JWT and what are its parts?**
Answer: JWT is a signed token with three parts: header, payload, signature. It carries claims and can be verified by server secret/public key.

62. **How do refresh tokens differ from access tokens?**
Answer: Access tokens are short-lived and used for API calls. Refresh tokens are long-lived and used only to obtain new access tokens securely.

## F) JavaScript / TypeScript / React / Next.js

63. **Difference between `var`, `let`, `const`.**
Answer: `var` is function-scoped and hoisted with loose behavior. `let` and `const` are block-scoped; `const` cannot be reassigned.

64. **Explain closure with practical frontend example.**
Answer: Closure is when inner function remembers outer lexical variables. Example: event handler uses state/config captured from component scope.

65. **What is event loop, microtask queue, macrotask queue?**
Answer: Event loop executes call stack and then queued tasks. Microtasks (Promise callbacks) run before macrotasks (`setTimeout`, IO callbacks) after current turn.

66. **Promise vs async/await.**
Answer: `async/await` is syntax over Promises for clearer sequential code. Underneath, both are asynchronous promise-based flow.

67. **What is hoisting and TDZ?**
Answer: Hoisting is declaration processing before execution. TDZ (temporal dead zone) is period where `let`/`const` exists but cannot be accessed before declaration line.

68. **Shallow copy vs deep copy in JS objects.**
Answer: Shallow copy duplicates top-level only; nested objects are still shared references. Deep copy recursively duplicates nested structures.

69. **Why TypeScript over plain JavaScript in larger codebases?**
Answer: TypeScript catches many errors at compile time, improves refactoring confidence, and documents contracts clearly. It is very useful in multi-module apps.

70. **`interface` vs `type` in TypeScript.**
Answer: Both describe shapes; `interface` is great for object contracts and extension, while `type` is flexible for unions/intersections and aliases.

71. **What are React hooks and hook rules?**
Answer: Hooks are functions like `useState`, `useEffect` for state/lifecycle logic in function components. Rules: call hooks at top level and only inside React functions.

72. **`useEffect` dependency pitfalls and stale closures.**
Answer: Missing dependencies can cause stale values; extra unstable dependencies can cause unwanted reruns. Use dependency array carefully and memoize callbacks when needed.

73. **Controlled vs uncontrolled components.**
Answer: Controlled components keep input state in React state. Uncontrolled components let DOM keep state and use refs to read values.

74. **Why key props matter in list rendering.**
Answer: Keys help React track item identity across re-renders. Stable keys prevent incorrect DOM reuse and state bugs.

75. **What is hydration in Next.js?**
Answer: Hydration is attaching React event listeners and client logic to server-rendered HTML. It turns static markup into interactive UI.

76. **SSR vs SSG vs CSR basics.**
Answer: SSR renders on each request, SSG prebuilds at build time, CSR renders mainly on client after JS loads. Each has tradeoffs in SEO, speed, and freshness.

77. **App Router vs Pages Router differences.**
Answer: App Router uses nested layouts/server components and new data-fetching patterns. Pages Router is older file-based route system with page-level patterns.

## G) Node.js and Backend API Basics

78. **Why Node.js is non-blocking and where it still blocks.**
Answer: Node uses event loop and async IO for non-blocking operations. CPU-heavy synchronous code and blocking libs can still block the single main thread.

79. **What is middleware in backend frameworks?**
Answer: Middleware is request/response processing layer before or after endpoint logic. It is used for auth, logging, validation, rate limiting, etc.

80. **How do you validate API payloads safely?**
Answer: Use schema validators (Zod/Pydantic/Joi), reject invalid shapes early, and sanitize risky fields. Never trust client input directly.

81. **What is rate limiting and common strategies?**
Answer: Rate limiting controls request volume to protect system and dependencies. Common strategies are fixed window, sliding window, and token bucket.

82. **What is input sanitization and output escaping?**
Answer: Sanitization removes unsafe content from input, and escaping ensures output is rendered safely in target context (HTML/SQL/JSON). Both are security basics.

83. **What is API versioning and why needed?**
Answer: Versioning allows API evolution without breaking existing clients. It supports backward compatibility during migrations.

84. **What is connection pooling and why important?**
Answer: Pooling reuses DB/network connections instead of creating new each request. It reduces latency and resource overhead.

85. **What is N+1 query problem conceptually?**
Answer: N+1 happens when one query fetches parent rows and then one extra query runs per parent. It causes unnecessary DB load and slow responses.

86. **How do you structure backend logs for debugging?**
Answer: Use structured logs with request id, user id, endpoint, status, latency, and error code. This makes tracing and aggregation easier.

87. **What are retries, backoff, and circuit breakers?**
Answer: Retry repeats failed request, backoff increases delay between retries, and circuit breaker stops repeated calls to failing dependency for a cool-down period.

## H) Python and FastAPI Basics

88. **Python list vs tuple.**
Answer: List is mutable; tuple is immutable. Tuples are good for fixed data records and can be hashable when elements are hashable.

89. **What are Python decorators?**
Answer: Decorators wrap functions/classes to add behavior without changing original function code directly. In web frameworks, they are often used for routing and middleware-like behavior.

90. **Sync vs async functions in FastAPI.**
Answer: `async def` is good for IO-bound work and concurrent handling. `def` is fine for CPU/light operations but heavy blocking work should move to workers.

91. **What is Pydantic and why useful?**
Answer: Pydantic validates and parses request/response data with typed models. It gives cleaner contracts and automatic error reporting.

92. **Dependency injection in FastAPI (`Depends`) basics.**
Answer: `Depends` provides reusable components like auth/user/db resolution per request. It keeps handlers cleaner and testable.

93. **How do you handle exceptions globally in FastAPI?**
Answer: Use exception handlers/middleware for common error formats and logging. Return consistent status codes and safe error messages.

94. **How do you write unit tests for FastAPI routes?**
Answer: Use `TestClient`, mock dependencies/services, call routes, and assert status/response behavior. Cover both success and failure paths.

95. **How do you manage virtual environments and dependency pinning?**
Answer: Use isolated virtual environment per project and lock dependency versions in `requirements.txt`/lockfile. This ensures reproducible setup across machines.

## I) AI / LLM / RAG Basics

96. **What is token context window?**
Answer: It is maximum token length model can process in one request (prompt + output). Exceeding it causes truncation/errors or forced reduction.

97. **What is temperature and top-p in generation?**
Answer: Temperature controls randomness intensity; top-p samples from cumulative probability mass. Lower values usually give more deterministic output.

98. **Why do LLMs hallucinate?**
Answer: Models predict likely next tokens, not guaranteed truth. If prompt/context lacks facts, model may produce plausible but incorrect statements.

99. **What is prompt engineering in practical terms?**
Answer: It is designing clear instructions, context, constraints, and output format so model behaves reliably for a task.

100. **What is RAG and when should you use it?**
Answer: RAG retrieves external context and injects it into prompt before generation. Use it when freshness/factual grounding is important.

101. **Embedding model vs generation model.**
Answer: Embedding model converts text to vectors for similarity search. Generation model produces natural language output.

102. **Vector similarity search basics (cosine similarity idea).**
Answer: Text vectors closer in direction are semantically similar. Cosine similarity measures angle closeness between vectors.

103. **Chunking tradeoffs in RAG.**
Answer: Small chunks improve precision but may lose context. Large chunks keep context but may reduce retrieval focus.

104. **Why citations/sources matter in enterprise AI apps?**
Answer: Citations improve trust, auditability, and compliance. Users can verify claims and detect weak output quickly.

105. **Guardrails against prompt injection in RAG systems.**
Answer: Treat retrieved text as untrusted data, enforce strict system instructions, and filter suspicious instruction-like content before prompt assembly.

106. **How do you evaluate LLM output quality?**
Answer: Use rubric metrics: relevance, correctness, completeness, clarity, format compliance, and safety. Combine human review and automated checks.

107. **Difference between fine-tuning and RAG.**
Answer: Fine-tuning changes model parameters using training data. RAG keeps model fixed and adds external context at inference time.

## J) DevOps, Testing, and Git Basics

108. **What is CI/CD and why it matters even for freshers?**
Answer: CI/CD automates build, test, and deployment pipelines. It reduces manual errors and gives faster feedback loops.

109. **What is semantic versioning?**
Answer: Semantic versioning is `MAJOR.MINOR.PATCH`. MAJOR for breaking changes, MINOR for backward-compatible features, PATCH for bug fixes.

110. **What is environment parity across dev/staging/prod?**
Answer: Parity means environments behave similarly in config/runtime so bugs don’t appear only after deployment. It improves release confidence.

111. **What should be in `.env` and what should never be committed?**
Answer: Keep secrets, API keys, DB URIs, and environment-specific configs in `.env`. Never commit real secrets to Git.

112. **Unit test vs integration test vs end-to-end test.**
Answer: Unit test checks small isolated logic; integration test checks modules working together; end-to-end test checks full user workflow.

113. **What makes a test flaky?**
Answer: Non-determinism from time, network, shared state, race conditions, or weak assertions. Flaky tests reduce trust in CI.

114. **How do you write meaningful assertions?**
Answer: Assert behavior and invariants, not only superficial fields. Include negative/error path assertions too.

115. **Git rebase vs merge basics.**
Answer: Merge preserves branch history with merge commit. Rebase rewrites commits on top of new base for linear history.

116. **What should a good PR description include?**
Answer: Problem statement, scope, approach, tradeoffs, test evidence, and rollback/impact notes. Keep it clear for reviewers.

117. **How do you debug production issues systematically?**
Answer: Reproduce with logs/metrics, isolate failing component, validate assumptions, apply minimal safe fix, and add postmortem action items to prevent repeat.
