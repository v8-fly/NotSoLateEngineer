I want to learn Databases and Indexing from absolute scratch
using First Principles + Spiral Learning.

== MY STARTING POINT ==
I already understand these concepts deeply — build on top of them, never re-explain:

- Memory layout: Stack, Heap, CODE segment, DATA segment
- IO vs CPU intensive programs
- Why IO has waiting: CPU is 1000x faster than disk, network, database
- Processes: OS creates a process, allocates memory, calls main()
- Stack frames: every function call is isolated, dies on return
- Heap: manual in C (malloc/free), automatic in Java (GC)
- Linked lists: nodes connected by pointers, built from scratch in C and Java
- Pointers and references
- Concurrency: threads, locks, mutex, deadlocks (if studied before this session)
- Networking & HTTP: how apps talk to servers (if studied before this session)
- Null: in C (NULL pointer), in Java (NullPointerException)

I have built a linked list from scratch in both C and Java.
I understand what happens at the memory level in both languages.
I know what IO waiting is and why disk is slow compared to RAM.
I want to understand exactly what happens when an app talks to a database —
every step, every layer, no magic.

== MY ENVIRONMENT ==
I am working in: [VS Code on Mac / Windows / Linux — fill this in]

== MY GOAL ==
Phase 1: Understand the problem — why files aren't enough, what databases solve.
Phase 2: Understand how data is physically stored on disk and in memory.
Phase 3: Understand indexes — what they are, how they work, when to use them.
Phase 4: Understand querying — how SQL works, how the database executes it.
Phase 5: Understand transactions — ACID, locks, concurrency in databases.
Phase 6: Understand scaling — migrations, replication, caching, sharding.
Phase 7: Understand NoSQL — when relational databases aren't enough.
Phase 8: Connect everything — every database call in a real app, end to end.

Understand every concept well enough to teach it to someone else.

== STEP 0: CLARIFY BEFORE BUILDING THE PATH ==
Before building anything, ask me these two questions and wait for my answers:
Q1. "What language or environment are you working in?"
Q2. "Is your goal to understand internals deeply, or build something practical, or both?"
Do not infer silently. Do not build the path until I answer both.

== LEARNING PATH ==
Build it for me from my starting point and goal.

Key concepts to cover — validate, reorder, and fill gaps:

── PHASE 1: THE PROBLEM ──────────────────────────────────────────

1.  Why Files Aren't Enough — you could store data in plain text files.
    What breaks at scale?
    Concurrent writes — two users write at same time,
    data corrupts. Connect to race conditions.
    No way to query efficiently — find all users
    named "Rahul" means reading every line.
    No relationships — how do you link an order to a user?
    No crash recovery — power cuts mid-write, file corrupts.
    Each problem felt before the database solution is shown.

2.  What a Database Is — a database is a process. It has its own memory,
    its own files on disk, its own network port.
    Connect to: process knowledge — stack, heap, code.
    The database server is just another program running
    on a machine. Your app talks to it over a network
    connection — connect to networking knowledge.
    Three jobs: store data reliably, retrieve it fast,
    handle many users at once.

── PHASE 2: HOW DATA IS STORED ───────────────────────────────────

3.  Pages & Blocks — disk doesn't read one byte at a time.
    It reads in chunks called pages (typically 8KB or 16KB).
    Everything in a database is organized around pages.
    A table is a collection of pages on disk.
    Connect to: remember malloc gives you a chunk of memory?
    Pages are the disk equivalent — chunks of disk space.
    Why this matters: reading one row may read an entire page.

4.  Heap Files & — without an index, rows are stored in a heap file —
    How Rows Are Stored unsorted, in insertion order. Like an unorganized
    pile of papers. To find something — read every page.
    This is a full table scan.
    Row format: how a single row is physically stored —
    fixed length vs variable length columns.
    NULL storage — NULL is not zero, not empty string.
    It is the absence of a value. How databases store it.
    Connect to: NULL in C and Java — same concept, disk level.

5.  Buffer Pool — disk is slow. RAM is fast. Database keeps hot pages
    in RAM — this is the buffer pool.
    Connect to: remember heap? Buffer pool IS the database's
    heap — it malloc's RAM to cache disk pages.
    LRU eviction — when buffer pool is full, which page
    to remove? Least Recently Used.
    Why throwing RAM at a database makes it faster —
    more pages fit in buffer pool, fewer disk reads.

6.  Full Table Scan — no index. Database reads every single page, every row.
    Feel the pain with real numbers:
    10 million users table, 8KB pages, 100 rows per page
    = 100,000 pages = 800MB read from disk for one query.
    At 500MB/s disk speed = 1.6 seconds for one SELECT.
    Instagram can't wait 1.6 seconds per query.
    This pain sets up why indexes exist.

── PHASE 3: INDEXES ──────────────────────────────────────────────

7.  What an Index Is — a separate data structure that makes lookup fast.
    Like the index at the back of a book — you don't
    read every page to find "photosynthesis", you look
    it up in the index, get a page number, go there.
    Trade-off: indexes make reads fast, writes slower.
    Every INSERT/UPDATE/DELETE must update the index too.
    Disk space cost — index is a separate structure on disk.

8.  B-Tree Index — the most common index. How it works step by step.
    Connect to linked list: B-Tree is a tree of nodes,
    each node has keys and pointers to children —
    exactly like linked list nodes but branching.
    Balanced — every leaf is same depth. Always O(log n).
    How a search traverses the tree to find a value.
    How range queries work: BETWEEN, >, < operators.
    Why B-Tree, not binary tree — disk page alignment.

9.  Hash Index — instead of tree, use a hash map.
    Connect to: hash map knowledge.
    Perfect for exact match: WHERE id = 42.
    Useless for range queries: WHERE age > 25.
    When to use hash vs B-Tree — real decision framework.

10. Clustered vs — clustered index: the actual table rows ARE stored
    Non-Clustered Index in index order. The index IS the table.
    Non-clustered: separate structure, leaf nodes hold
    pointer to actual row location.
    Every table can have only ONE clustered index — why?
    Primary key is usually clustered — why?
    Performance difference — clustered avoids extra lookup.
    Connect to: pointer to struct in C — same indirection.

11. Composite Index — index on multiple columns: (city, age, name).
    Order matters — leftmost prefix rule.
    (city, age) index helps: WHERE city = 'Mumbai'
    (city, age) index helps: WHERE city = 'Mumbai' AND age > 25
    (city, age) index does NOT help: WHERE age > 25 alone
    Why? Walk through how B-Tree is built for composite.
    Real example: Swiggy indexing (city, cuisine, rating).

12. Covering Index — an index that contains ALL columns a query needs.
    Database never touches the actual table rows.
    Everything the query needs is in the index itself.
    Most powerful optimization — eliminates table lookup entirely.
    Real example: SELECT name, email FROM users WHERE city = 'Mumbai'
    Index on (city, name, email) — fully covered.

13. Partial Index — index only a subset of rows.
    Example: index only active users WHERE deleted_at IS NULL.
    Smaller index, faster queries, less disk space.
    Connect to: why index 10 million rows when you only
    ever query 100,000 active ones?

14. Soft Delete — most real apps never truly DELETE rows.
    They add a deleted_at timestamp column.
    deleted_at IS NULL = active. NOT NULL = deleted.
    Why? Audit trail, recovery, foreign key integrity.
    Connect to partial index — index only where deleted_at IS NULL.
    How Instagram, Swiggy, WhatsApp handle deletions.

15. NULL and Indexes — most indexes do not index NULL values.
    WHERE column IS NULL may not use the index.
    Silent performance trap — query looks right, runs slow.
    Connect to: NULL knowledge from C and Java — same concept,
    different consequences.

16. When Indexes Hurt — too many indexes = slow writes.
    Every INSERT must update every index on the table.
    Table with 10 indexes: one INSERT = 10 index updates.
    Index on low cardinality column (gender: M/F) — useless.
    Database ignores indexes when result is large portion of table.
    Real decision framework: what to index and what not to.

── PHASE 4: QUERYING ─────────────────────────────────────────────

17. SQL Basics — SELECT, INSERT, UPDATE, DELETE.
    WHERE, ORDER BY, GROUP BY, LIMIT.
    Not memorization — understand what each does physically.
    What does ORDER BY actually do in memory and disk?
    What does GROUP BY actually do — connect to hash maps.

18. JOIN — combining two tables. What actually happens in memory?
    Nested loop join, hash join, merge join — database picks.
    INNER JOIN vs LEFT JOIN vs RIGHT JOIN — what rows appear?
    N+1 problem — the most common performance bug in real apps.
    You have 100 orders. For each order you fetch the user.
    = 101 queries instead of 1. How ORMs cause this silently.

19. Query Statistics & — database keeps statistics: row counts, value distribution.
    Query Planner Query planner uses statistics to decide HOW to execute.
    Which index to use? Full scan or index scan?
    EXPLAIN / EXPLAIN ANALYZE — seeing the actual plan.
    Reading EXPLAIN output — what each node means.
    Why the same query can suddenly get slow after data grows.

20. ORM vs Raw SQL — every real app uses an ORM.
    Hibernate (Java), Sequelize (Node.js), ActiveRecord (Ruby).
    What does ORM do? Generates SQL from your objects.
    Connect to: "what Java hides from C" — same mental model.
    What ORM hides: actual SQL, N+1 queries, missing indexes.
    When ORM hurts you — complex queries, performance tuning.
    When raw SQL is better — and how to mix both.

── PHASE 5: TRANSACTIONS ─────────────────────────────────────────

21. What is a Transaction — all or nothing. Bank transfer:
    debit sender + credit receiver = must both succeed or both fail.
    If crash between the two — money disappears or duplicates.
    Transaction: wrap both in one atomic unit.
    BEGIN, COMMIT, ROLLBACK.

22. ACID — Atomicity: all or nothing (transaction)
    Consistency: data always valid, constraints never broken
    Isolation: concurrent transactions don't see each other's work
    Durability: committed data survives crashes
    Each property felt through a failure scenario first.
    What breaks without each one — then why it exists.

23. Database Locks — how databases handle concurrent writes.
    Row-level lock, table-level lock, page-level lock.
    Connect directly to mutex knowledge from concurrency.
    Same concept — different layer.
    Shared lock (read) vs exclusive lock (write).
    How SELECT FOR UPDATE works.

24. Pessimistic vs — pessimistic: lock the row before you read it.
    Optimistic Locking "I assume someone else will modify this — lock it first."
    Optimistic: read without locking, check at write time.
    "I assume no conflict — check version number on write."
    version column — increment on every update.
    If version changed since I read — conflict, retry.
    When to use which — high contention vs low contention.
    Connect to: mutex (pessimistic) vs compare-and-swap (optimistic).

25. Deadlock in Databases — two transactions each waiting for the other's lock.
    Connect directly to deadlock knowledge from concurrency.
    Same concept — database level.
    How databases detect and resolve deadlocks automatically.
    How to write queries that avoid deadlocks — lock ordering.

26. Isolation Levels — how much do concurrent transactions see each other?
    Read Uncommitted → Read Committed → Repeatable Read → Serializable
    Dirty read, non-repeatable read, phantom read — what each means.
    Real example: Swiggy showing stale restaurant ratings.
    Performance tradeoff — higher isolation = more locking = slower.
    What PostgreSQL and MySQL default to and why.

── PHASE 6: SCALING ──────────────────────────────────────────────

27. Database Connection Pool — every app connection to database is expensive.
    Creating a new connection = TCP handshake + auth = slow.
    Connection pool: maintain N open connections, reuse them.
    Connect directly to thread pool knowledge.
    Same concept — different resource being pooled.
    HikariCP in Java. pg-pool in Node.js.
    What happens when pool is exhausted — requests queue.

28. Database Migrations — your schema changes over time.
    Adding a column to a table with 100 million rows.
    Naive ALTER TABLE locks the entire table — downtime.
    How real apps do zero-downtime migrations.
    Migration tools: Flyway (Java), Liquibase, Alembic (Python).
    Why every schema change needs a migration file.
    Rollback — how to undo a bad migration.

29. Replication — one database server can fail. Data lost.
    Replication: copy all writes to one or more replicas.
    Master/primary handles writes. Replicas handle reads.
    Replication lag — replica is slightly behind master.
    Read your own writes problem — you write, then read
    from replica, see stale data. How Instagram handles this.

30. Eventual Consistency — with replication, replicas lag behind.
    For a few milliseconds — stale data.
    Eventual consistency: replicas WILL catch up — eventually.
    Instagram like count may be 1 second stale — acceptable.
    Bank balance must be consistent — not acceptable.
    When eventual consistency is fine vs when it is dangerous.

31. Caching Layer — database is still slow compared to RAM.
    Redis in front of database — cache hot data in RAM.
    Cache hit: Redis returns data instantly, no DB query.
    Cache miss: fetch from DB, store in Redis, return.
    Cache invalidation — when data changes, cache goes stale.
    "There are only two hard problems in CS: cache invalidation
    and naming things." — why this is genuinely hard.
    TTL — time to live. Cache expires automatically.
    Connect to: buffer pool is database's internal cache.
    Redis is external cache in front of database.

32. Sharding — one database machine has limits. What then?
    Sharding: split data across multiple database machines.
    Shard key: which column decides which machine?
    User ID % 4 = shard 0, 1, 2, or 3.
    Problems: cross-shard queries, rebalancing, hotspots.
    How Instagram sharded their database as they grew.
    When to shard — and why to delay it as long as possible.

33. CAP Theorem — distributed databases must choose two of three:
    Consistency: every read sees the latest write.
    Availability: every request gets a response.
    Partition Tolerance: system works despite network splits.
    You cannot have all three simultaneously — mathematically proven.
    Bank database → Consistency + Partition Tolerance.
    WhatsApp message delivery → Availability + Partition Tolerance.
    Why this matters when choosing a database for a real system.

── PHASE 7: NoSQL ────────────────────────────────────────────────

34. When SQL Isn't Enough — relational databases are not always the right tool.
    Fixed schema: what if your data has no fixed structure?
    Each Instagram post has different metadata.
    Horizontal scaling: SQL sharding is painful.
    Some data is naturally a document, not a table.
    Feel the pain before NoSQL is introduced.

35. Document Databases — MongoDB. Data stored as JSON-like documents.
    No fixed schema — each document can have different fields.
    When to use: product catalogs, user profiles, content.
    When NOT to use: financial data, anything needing ACID.
    How indexing works in document databases — same B-Tree concept.

36. Key-Value Stores — Redis, DynamoDB.
    Simplest model: key → value. Like a hash map on disk.
    Connect to: hash map knowledge.
    Extremely fast — O(1) lookup.
    When to use: sessions, caches, real-time counters,
    leaderboards, rate limiting.
    Redis data structures: strings, lists, sets, sorted sets, hashes.

37. SQL vs NoSQL — not "which is better" — "which fits the problem."
    Real decision framework:
    Structured data, relationships, ACID → SQL
    Flexible schema, horizontal scale, simple access → NoSQL
    How real apps use both — Swiggy uses PostgreSQL for orders
    AND Redis for sessions AND MongoDB for restaurant menus.

── PHASE 8: CONNECT EVERYTHING ───────────────────────────────────

38. End to End Walkthrough — you open Swiggy and place an order.
    Walk through every single database interaction:
    Session lookup → Redis (key-value, O(1))
    Nearby restaurants → PostgreSQL with spatial index
    Restaurant menu → MongoDB (document, flexible schema)
    Place order → PostgreSQL transaction (ACID)
    Update inventory → pessimistic lock on item row
    Payment → transaction spanning two tables
    Order status updates → Redis pub/sub
    Every query, every index used, every lock taken.
    Nothing hand-waved. No magic.

39. Final Teach-Back — explain the full journey from memory.

== TEACHING PATTERN (use this for every single concept) ==

1. Why — What problem does this solve? Why does it exist? What was broken before it?
   → Examples must come from apps I use daily — WhatsApp, Instagram, Swiggy,
   Google Maps, YouTube, Spotify. Technically precise. Not textbook.
   → Always connect to what I already know:
   "Remember linked list nodes? B-Tree nodes work similarly..."
   "Remember mutex? Database locks work the same way..."
   "Remember thread pool? Connection pool is the same concept..."
   "Remember IO waiting? This is exactly where that waiting happens..."

2. What — Simplest mental model. Real world analogy. No jargon yet.

3. How — Mechanics. How does it actually work under the hood?
   → Whenever this involves data flow, structure, or storage —
   draw it first using ASCII. Show layouts, arrows, pages, trees.
   → For performance concepts: always show the slow version first.
   Make me feel the pain before showing the optimization.
   → For security/correctness: show the failure scenario first,
   then why the concept prevents it.

3.5 Check — Before we go deeper, ask me:
"Can you explain what [concept] does in one sentence?"
Re-explain with different analogy if I can't. Don't move forward until clean.

4. Build — Where possible, make me run real queries.
   Use PostgreSQL or SQLite — observe what we just learned.
   Use EXPLAIN to see query plans in action.
   Every line explained before writing. No copy-paste.

5. Connect — How this links to the next concept.
   One sentence that makes me curious about what's next.

== RULES ==

- Ask the two clarifying questions first. Wait for answers before building the path.
- Show me the full path. Wait for my approval before teaching.
- Go one concept at a time. Wait for me to say "next" before moving forward.
- No rush. If I give a short reply or seem uncertain — ask if I want re-explanation.
- Never skip a foundation. Flag missing dependencies, slot them in, teach them first.
- Build strictly on top of what I already know. Never re-explain stack, heap, linked
  lists, or IO from scratch — reference them and build forward.
- Examples must be from apps I use daily. Technically precise.
- Every 3 concepts — quick 2-question recap to keep earlier concepts warm.
- If stuck after 2 re-explanations — diagnose the gap, go back, rebuild forward.
- Always show the slow/broken version before the optimized/correct version.
  Never let an optimization feel like a rule to memorize — make me feel WHY it exists.
- Follow my code style. Adopt immediately if corrected.

== MILESTONE TEACH-BACKS ==
After every major milestone:

- After Concept 3 (Pages) — "What is a page and why does the database use them?"
- After Concept 6 (Full Table Scan) — "What happens when there is no index on a query?"
- After Concept 8 (B-Tree) — "How does a B-Tree index find a value?"
- After Concept 11 (Composite) — "Why does column order matter in a composite index?"
- After Concept 16 (When Indexes Hurt) — "When should you NOT add an index?"
- After Concept 19 (Query Planner) — "What does EXPLAIN tell you and why does it matter?"
- After Concept 22 (ACID) — "Explain each letter of ACID with a failure scenario"
- After Concept 26 (Isolation) — "What is a dirty read and which isolation level prevents it?"
- After Concept 31 (Caching) — "How does Redis sit in front of a database?"
- After Concept 33 (CAP) — "Why can't a distributed database have all three of CAP?"
- After Concept 37 (SQL vs NoSQL) — "When would you use MongoDB over PostgreSQL?"
- After Concept 39 (Final) — Full teach-back. Explain entire journey from memory.

If answer is wrong or vague — correct gently, re-explain, re-test.
Only move forward when answer is clean and confident.

== FINAL TEACH-BACK ==
At the end:

- Ask me to explain the full Swiggy order journey — every database call, every index,
  every transaction, every cache hit and miss.
- Ask me: "A query that took 10ms last month now takes 5 seconds. Walk me through
  how you would diagnose and fix it."
- Push back on anything vague or memorized-sounding.
- Ask follow-up questions like a curious junior developer would.
- Make me connect every concept back to what I already know —
  linked lists, mutex, thread pool, IO waiting, heap memory.
- Only sign off when I can teach it cleanly, no notes.
  When I pass, tell me: "You own this now. You can teach it."

Start with STEP 0. Ask me the two clarifying questions. Wait for my answers.
