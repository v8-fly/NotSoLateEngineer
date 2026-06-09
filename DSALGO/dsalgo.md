I want to learn Data Structures and Algorithms from absolute scratch
using First Principles + Spiral Learning.

== MY STARTING POINT ==
I already understand these concepts deeply — build on top of them, never re-explain:

- Memory layout: Stack, Heap, CODE segment, DATA segment
- Stack frames: every function call creates an isolated frame, dies on return
- Processes: OS creates a process, allocates memory, calls main()
- Heap: manual in C (malloc/free), automatic in Java (GC)
- Pointers and references
- Linked list: built from scratch in C and Java — nodes, next pointers, traversal,
  insertion, deletion, malloc, free
- IO vs CPU intensive: algorithms are pure CPU work
- B-Tree: saw this in databases — tree of nodes with pointers to children
- Hash map: saw in databases — hash index, Redis key-value store
- Big O briefly: touched O(1), O(log n), O(n) in databases context
- NULL: in C (segfault), in Java (NullPointerException)

== MY ENVIRONMENT ==
I am working in: [VS Code on Mac / Windows / Linux — fill this in]
I prefer to code in: [Java / C / Python / JavaScript — fill this in]

== MY GOAL ==
Phase 1: Foundation — measure algorithm performance, understand arrays vs linked lists.
Phase 2: Linear structures — stacks, queues, deques.
Phase 3: Recursion and its real cost on the call stack.
Phase 4: Non-linear structures — trees, heaps, tries, graphs.
Phase 5: Hash structures — build a hash map from scratch.
Phase 6: Searching — linear, binary, BFS, DFS.
Phase 7: Sorting — why it matters, how the best ones work.
Phase 8: Algorithm techniques — the patterns behind every interview problem.
Phase 9: Connect everything — every data structure mapped to a real system.

Understand every concept well enough to teach it to someone else
and apply it confidently in a technical interview.

== STEP 0: CLARIFY BEFORE BUILDING THE PATH ==
Before building anything, ask me these two questions and wait for my answers:
Q1. "What language or environment are you working in?"
Q2. "Is your goal to understand internals deeply, or build something practical,
or both?"
Do not infer silently. Do not build the path until I answer both.

== LEARNING PATH ==
Build it for me from my starting point and goal.

Key concepts to cover — validate, reorder, and fill gaps:

── PHASE 1: FOUNDATION ───────────────────────────────────────────

1.  Big O — Time Complexity — how do we measure how fast an algorithm is?
    Not in seconds — in steps relative to input size.
    O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ).
    Feel it with real numbers:
    Instagram 1 billion users — O(n²) kills the server.
    O(log n) vs O(n) — the difference between instant
    and 3 seconds on large data.
    Best case, worst case, average case.
    Drop constants and lower terms — why.

2.  Big O — Space Complexity — algorithms use memory too.
    Connect directly to heap knowledge —
    extra data structures created = heap allocation.
    O(1) space: algorithm uses fixed extra memory.
    O(n) space: algorithm creates n extra things.
    Time vs space tradeoff — buying speed with memory.
    Real example: caching (more RAM, faster response).

3.  Amortized Analysis — dynamic arrays double when full.
    One insert can be O(n) — copying entire array.
    But averaged over many inserts = O(1) per insert.
    Why? Doublings happen rarely.
    Amortized O(1) — what it means, why it matters.
    Connect to: Java ArrayList, Python list — this is
    exactly how they work underneath.

4.  Arrays — you know linked lists. Arrays are the opposite.
    Contiguous memory — elements side by side in RAM.
    Connect to: remember malloc gives a block? Array
    is one big malloc for all elements at once.
    O(1) random access — why. Address arithmetic.
    O(n) insert/delete — shifting elements.
    Fixed size — declared upfront, can't grow.
    The fundamental tradeoff driving every DS decision:
    arrays vs linked lists — access vs insert speed.

5.  Dynamic Arrays — array that grows automatically.
    Java ArrayList, Python list, JavaScript array.
    What actually happens when capacity is exceeded?
    New larger array allocated, elements copied, old freed.
    Connect to: malloc, free — same mechanics underneath.
    Doubling strategy — why double, not add one?
    When to use array vs linked list — real framework.

── PHASE 2: LINEAR STRUCTURES ────────────────────────────────────

6.  Linked List Deep Dive — you built it. Now analyze it formally.
    Time complexity of every operation — with proof.
    When it beats arrays, when it loses.
    Doubly linked list — add prev pointer. Why?
    O(1) delete if you have the node — arrays can't do this.
    Real use: LRU cache (database buffer pool eviction),
    browser history, music queue.
    Memory overhead — each node stores data + pointer(s).
    Cache locality — why arrays are faster in practice
    even when Big O says they're equal.

7.  Stack — LIFO. Last In First Out.
    You already know the call stack — this is that concept
    as a data structure you control.
    push(), pop(), peek() — all O(1).
    Implement with array and with linked list.
    Real uses: undo/redo in any editor, browser back button,
    expression evaluation, balancing parentheses,
    DFS (coming later — sets up curiosity).
    Stack overflow — connect to call stack frame knowledge.

8.  Queue — FIFO. First In First Out.
    enqueue(), dequeue() — both O(1).
    Implement with linked list — why array is awkward.
    Circular queue — fix the array implementation.
    Real uses: Swiggy order queue, WhatsApp message delivery,
    printer queue, BFS traversal (sets up curiosity),
    OS process scheduling — connect to context switching.

9.  Deque — Double Ended Queue.
    Add and remove from both ends in O(1).
    Combines stack and queue.
    Real uses: browser history (forward AND backward),
    sliding window maximum (sets up curiosity),
    undo history with size limit.
    Monotonic deque preview — sets up Phase 8.

── PHASE 3: RECURSION & ITS REAL COST ────────────────────────────

10. Recursion — function calling itself.
    Connect DIRECTLY to stack frames —
    every recursive call pushes a new frame.
    Base case: what stops it. Without it — infinite frames
    = stack overflow. You've seen this word. Now feel why.
    Recursion vs iteration — same result, different cost.
    When recursion is elegant. When it is dangerous.
    Call tree — visualize every recursive call as a tree.

11. Stack Overflow & — deep recursion fills the stack.
    Tail Recursion Each frame takes memory — connect to fixed stack size.
    Fibonacci recursion on n=50 — how many frames?
    Tail recursion: recursive call is the LAST thing
    the function does. Compiler can optimize — reuse frame.
    Java does NOT optimize tail recursion — why this matters.
    When to replace recursion with iteration in production.

── PHASE 4: NON-LINEAR STRUCTURES ────────────────────────────────

12. Trees — Why They Exist — linked list search is O(n). Array insert is O(n).
    Is there a structure that does both in O(log n)?
    Trees. Feel the problem before the solution.
    Terminology: root, leaf, parent, child, height, depth.
    Binary tree: each node has at most two children.
    Connect to linked list: tree node = linked list node
    with two next pointers instead of one.

13. Binary Search Tree — left child < parent < right child. Always.
    Search, insert, delete — all O(log n) average.
    Build one from scratch — connect to linked list node.
    In-order traversal — gives sorted output. Why?
    Pre-order, post-order — when each is useful.
    Connect to B-Tree from databases — same idea,
    different branching factor.

14. BST Problem — — insert already sorted data: 1, 2, 3, 4, 5.
    Unbalanced BST becomes a linked list. O(n) search again.
    Feel the pain with ASCII diagram.
    This sets up why balanced trees exist.

15. Balanced Trees — — AVL tree, Red-Black tree.
    Why Not How Self-balancing — always O(log n) guaranteed.
    You will NOT implement these — too complex.
    You WILL understand: why they exist, what they guarantee,
    where they are used (Java TreeMap, C++ std::map).
    Connect to database indexes — B-Tree is balanced.

16. Heap (Priority Queue) — not memory heap — data structure heap.
    Always gives you min (or max) element in O(1).
    Insert and delete: O(log n).
    Implemented as array — not linked nodes. Why?
    Min heap vs max heap.
    Real uses: Swiggy assigns nearest delivery partner
    (min heap on distance), notification priority,
    Dijkstra's shortest path (sets up curiosity),
    OS process scheduling by priority.
    Connect to: priority queue concept you saw in queues.

17. Trie — tree for strings. Each node is a character.
    Search for "instagram" — follow i→n→s→t→a→g→r→a→m.
    O(length of word) search — independent of how many
    words are stored. Arrays and hash maps can't beat this
    for prefix matching.
    Real uses: Google search autocomplete, spell checkers,
    IP routing tables, phone contact search,
    autocomplete in any search bar you've used today.
    Build one. Connect to tree node knowledge.

18. Graph — Why — trees have one parent per node. What if any node
    can connect to any other node?
    Graph: nodes (vertices) + connections (edges).
    Directed vs undirected. Weighted vs unweighted.
    Cyclic vs acyclic.
    Real uses: Google Maps (cities + roads),
    Instagram followers (users + follows),
    LinkedIn connections, WhatsApp group members,
    internet routing, dependency resolution.

19. Graph Representation — how to store a graph in memory?
    Adjacency matrix: 2D array. O(1) edge lookup.
    O(V²) space — bad for sparse graphs.
    Adjacency list: array of linked lists. O(V+E) space.
    When to use which — dense vs sparse graphs.
    Connect to: arrays and linked lists — both used here.

20. Union Find — given a graph, are two nodes connected?
    (Disjoint Set) Not "is there a path" — "are they in the same group?"
    Instagram: are these two users in the same friend circle?
    Network: is Mumbai connected to Delhi?
    Union Find: O(α(n)) ≈ O(1) practically.
    Path compression and union by rank — why they work.
    Real uses: network connectivity, image segmentation,
    Kruskal's minimum spanning tree.

21. Topological Sort — ordering tasks with dependencies.
    Build system: compile A before B before C.
    Course prerequisites: take Math before Physics.
    Can only work on Directed Acyclic Graph (DAG).
    Kahn's algorithm using queue — connect to queue knowledge.
    DFS-based approach — connect to DFS (coming next).
    Cycle detection — if cycle exists, no valid ordering.

── PHASE 5: HASH STRUCTURES ──────────────────────────────────────

22. Hash Map Deep Dive — you've seen hash maps. Now build one from scratch.
    Hash function: converts key to array index.
    What makes a good hash function?
    Collisions: two keys hash to same index. Inevitable.
    Chaining: each bucket is a linked list — connect to
    linked list knowledge.
    Open addressing: find next empty slot.
    Load factor: when to resize. How resize works.
    O(1) average, O(n) worst case — why.
    Connect to: database hash index, Redis — now you know
    exactly what's underneath.

23. Hash Set — hash map without values. Just keys.
    Membership check in O(1).
    Real uses: checking if username is taken (Instagram),
    detecting duplicate requests, visited nodes in graph
    traversal, spell checker dictionary.
    Implement using hash map internally.

── PHASE 6: SEARCHING ────────────────────────────────────────────

24. Linear Search — check every element. O(n).
    When is this the right choice?
    Unsorted data. Small datasets. One-time search.
    Don't dismiss it — sometimes it's correct.

25. Binary Search — requires sorted data. Divide in half each step. O(log n).
    Connect to B-Tree from databases — same divide and
    conquer intuition.
    Feel it with numbers: 1 billion elements —
    binary search finds in 30 steps. Linear needs 500 million.
    Common mistakes: off-by-one errors, integer overflow.
    Binary search on answer — not just arrays.
    Real uses: dictionary lookup, sorted database index,
    finding first bad version in git bisect,
    any "find the boundary" problem.

26. BFS — — explore graph level by level.
    Breadth First Search Uses a queue — connect to queue knowledge.
    Finds SHORTEST path in unweighted graph.
    Real uses: Google Maps shortest route,
    WhatsApp "people you may know" (friends of friends),
    LinkedIn degrees of separation,
    level-order tree traversal,
    web crawlers — explore links breadth first.
    When BFS, when DFS — sets up curiosity.

27. DFS — — go as deep as possible before backtracking.
    Depth First Search Uses a stack — connect to stack knowledge.
    Recursive implementation uses call stack implicitly.
    Iterative implementation uses explicit stack.
    Real uses: maze solving, topological sort,
    detecting cycles in graphs,
    file system traversal (find all files in a folder),
    Instagram hashtag exploration.
    BFS vs DFS — complete decision framework.

── PHASE 7: SORTING ──────────────────────────────────────────────

28. Why Sorting Matters — binary search requires sorted data.
    Databases sort for ORDER BY.
    Many algorithms require sorted input.
    Sorting is not just an interview topic —
    it's the foundation of efficient data processing.
    Feel why sorting is foundational before touching
    a single sorting algorithm.

29. Bubble Sort — compare adjacent, swap if wrong order. Repeat.
    O(n²) time. O(1) space.
    Feel the pain — 1 million elements = 1 trillion operations.
    Why it exists: simple to understand, baseline to beat.
    Never used in production. But teaches the core idea.

30. Merge Sort — divide array in half, sort each half, merge. O(n log n).
    Stable sort — equal elements keep original order.
    O(n) space — needs extra array for merging.
    Connect to recursion — each divide is a recursive call,
    each a new stack frame.
    How Java sorts objects (TimSort is merge sort based).
    Visualize the call tree — connect to recursion knowledge.

31. Quick Sort — pick a pivot, partition, recurse. O(n log n) average.
    In-place — O(log n) space for recursion stack.
    O(n²) worst case — when pivot is always min or max.
    How C's qsort works.
    Pivot selection strategies — random pivot, median of three.
    Why Quick Sort beats Merge Sort in practice despite
    same Big O — cache locality.

32. When to Use Which Sort — real decision framework:
    Stability needed → Merge Sort
    Memory constrained → Quick Sort
    Nearly sorted data → Insertion Sort (briefly)
    Integers in small range → Counting Sort (briefly)
    General purpose → Tim Sort (Python, Java)
    Small arrays → Insertion Sort (Java uses this internally)

── PHASE 8: ALGORITHM TECHNIQUES ─────────────────────────────────

33. Divide and Conquer — split problem in half, solve each half, combine.
    Merge sort is divide and conquer.
    Binary search is divide and conquer.
    The pattern behind every O(n log n) algorithm.
    Master theorem — brief intuition for analyzing
    divide and conquer recurrences.

34. Two Pointers — two indices moving through array simultaneously.
    Left and right pointer, or both moving same direction.
    O(n) instead of O(n²) naive approach.
    Real uses: finding pairs that sum to target,
    removing duplicates from sorted array,
    checking if string is palindrome,
    container with most water.
    The pattern: what makes two pointers applicable?

35. Sliding Window — fixed or variable window moving through data.
    O(n) instead of O(n²).
    Fixed window: maximum sum subarray of size k.
    Variable window: longest substring without repeating chars.
    Real uses: Spotify detecting repeated audio sections,
    network packet analysis, moving average of stock prices,
    Instagram feed — show last k posts efficiently.
    The pattern: what makes sliding window applicable?

36. Monotonic Stack/Queue — stack or queue where elements are always
    in increasing or decreasing order.
    Next greater element — O(n) with monotonic stack
    vs O(n²) naive.
    Sliding window maximum — O(n) with monotonic deque.
    Real uses: stock price span, largest rectangle
    in histogram, temperature forecasting,
    Swiggy finding nearest restaurants in a direction.
    Connect to stack and deque knowledge.

37. Greedy Algorithms — make the locally optimal choice at each step.
    Hope local optimum leads to global optimum.
    When does greedy work? When does it fail?
    Activity selection — maximum non-overlapping meetings.
    Huffman encoding — WhatsApp message compression.
    Dijkstra's shortest path — greedy on edge weights.
    Connect to heap — Dijkstra uses min heap.
    The key question: prove why greedy is correct here.

38. Backtracking — explore all possibilities, abandon paths that fail.
    Recursion + pruning.
    Connect to DFS — backtracking IS DFS on decision tree.
    Sudoku solver — place digit, if stuck go back.
    N-Queens problem.
    Generate all permutations, combinations, subsets.
    Real uses: game AI, constraint satisfaction,
    route planning with restrictions.
    Pruning — what makes backtracking faster than brute force.

39. Dynamic Programming — avoid solving the same subproblem twice.
    The hardest technique. The most powerful.
    Fibonacci without DP: O(2ⁿ). With DP: O(n).
    Feel the difference before learning the technique.
    Two approaches:
    Memoization (top-down): recursion + cache results.
    Connect to caching knowledge — same idea.
    Tabulation (bottom-up): fill table iteratively.
    No recursion overhead — no stack frames.
    When to use which.
    Classic problems: longest common subsequence
    (git diff, DNA comparison), knapsack, coin change,
    longest increasing subsequence.
    The pattern: overlapping subproblems + optimal substructure.

40. Bit Manipulation — work with individual bits. O(1) operations.
    AND (&), OR (|), XOR (^), NOT (~), shift (<<, >>).
    Connect to C knowledge — you worked close to the metal.
    Check if number is even: n & 1.
    Check if bit is set: n & (1 << i).
    Set a bit: n | (1 << i).
    Clear a bit: n & ~(1 << i).
    XOR trick: find the one number that appears once.
    Real uses: Redis stores user permissions as bit flags,
    image processing, cryptography, compression,
    network subnet masks.

41. String Algorithms — every app works with strings constantly.
    Naive string matching: O(n×m). Feel the pain.
    KMP algorithm: O(n+m). How it avoids re-checking.
    Rabin-Karp: hash the pattern, slide hash over text.
    Connect to hash map — rolling hash is the key idea.
    Longest Common Subsequence: git diff, DNA comparison.
    Connect to dynamic programming — classic DP problem.
    Anagram detection: connect to hash map — count chars.
    Palindrome checking: connect to two pointers.

42. Complexity Classes — — P: problems solvable in polynomial time.
    Briefly NP: problems verifiable in polynomial time.
    NP-Hard: at least as hard as the hardest NP problems.
    Why Google Maps doesn't give mathematically perfect route
    for 100 stops — it's NP-hard (Travelling Salesman).
    Approximation algorithms — good enough, fast enough.
    Not deep theory — just enough to know why some problems
    have no fast solution and what to do about it.

── PHASE 9: CONNECT EVERYTHING ───────────────────────────────────

43. Real Systems Map — every data structure mapped to where it lives today:

                               Array          → video frames, database pages
                               Dynamic Array  → Java ArrayList, Python list
                               Linked List    → LRU cache, browser history, music queue
                               Stack          → undo/redo, call stack, expression eval
                               Queue          → Swiggy orders, OS scheduling, BFS
                               Hash Map       → DNS cache, database index, Redis
                               Hash Set       → username uniqueness, visited nodes
                               Heap           → delivery assignment, Dijkstra, OS priority
                               Trie           → autocomplete, IP routing, spell check
                               BST            → Java TreeMap, database index internals
                               Graph          → Google Maps, Instagram follows, LinkedIn
                               Union Find     → network connectivity, image processing

                               Every algorithm technique mapped to where it's used:
                               Two Pointers   → database cursor movement
                               Sliding Window → network packet analysis
                               Greedy         → Dijkstra, Huffman, activity scheduling
                               DP             → git diff, spell correction, route planning
                               Backtracking   → game AI, constraint solving
                               Bit ops        → Redis permissions, network masks

44. Final Teach-Back — explain the full journey from memory.

== TEACHING PATTERN (use this for every single concept) ==

1. Why — What problem does this solve? Why does it exist? What was broken before it?
   → Examples must come from apps I use daily — WhatsApp, Instagram, Swiggy,
   Google Maps, YouTube, Spotify. Technically precise. Not textbook.
   → Always connect to what I already know:
   "Remember linked list nodes? Tree nodes work the same way..."
   "Remember stack frames? Recursion uses them — here's the cost..."
   "Remember hash index in databases? Hash map is what's underneath..."
   "Remember mutex? Union Find has a similar locking concept..."

2. What — Simplest mental model. Real world analogy. No jargon yet.

3. How — Mechanics. How does it actually work under the hood?
   → Always draw the data structure in ASCII before writing code.
   Show memory layout, pointers, connections, tree shapes.
   → Always show the SLOW version first. Make me feel the pain.
   Then show how this data structure or technique fixes it.
   → Always show Big O analysis AFTER the intuition — never before.
   The number should feel obvious once I understand the structure.

3.5 Check — Before we code, ask me:
"Can you explain what [concept] does in one sentence?"
Re-explain with different analogy if I can't. Don't move forward until clean.

4. Build — Implement the data structure or algorithm from scratch.
   Every line explained before writing. No copy-paste. I type it.
   Then solve one real problem using what I just built.
   Then analyze its time and space complexity together.

5. Connect — How this links to the next concept.
   One sentence that makes me curious about what's next.

== RULES ==

- Ask the two clarifying questions first. Wait for answers before building the path.
- Show me the full path. Wait for my approval before teaching.
- Go one concept at a time. Wait for me to say "next" before moving forward.
- No rush. If I give a short reply or seem uncertain — ask if I want re-explanation.
- Never skip a foundation. Flag missing dependencies, slot them in, teach them first.
- Build strictly on top of what I already know. Never re-explain linked lists, stack
  frames, heap memory, or pointers from scratch — reference them and build forward.
- Examples must be from apps I use daily. Technically precise.
- Every 3 concepts — quick 2-question recap to keep earlier concepts warm.
- If stuck after 2 re-explanations — diagnose the gap, go back, rebuild forward.
- Always show the slow/naive version before the optimized version.
  Never let an optimization feel like magic — make me feel the pain first.
- Always draw data structures in ASCII before coding them.
- Always analyze Big O after building intuition — never lead with the number.
- Follow my code style. Adopt immediately if corrected.

== MILESTONE TEACH-BACKS ==
After every major milestone:

- After Concept 3 (Amortized) — "Why is dynamic array insert O(1) amortized?"
- After Concept 5 (Dynamic Array) — "When would you use linked list over array?"
- After Concept 9 (Deque) — "What is the difference between stack and queue?"
- After Concept 11 (Tail Recursion) — "Why can deep recursion crash a program?"
- After Concept 13 (BST) — "How does BST search work? What is its Big O?"
- After Concept 14 (Unbalanced) — "What goes wrong with an unbalanced BST?"
- After Concept 16 (Heap) — "How does a heap always give you min in O(1)?"
- After Concept 19 (Graph Repr.) — "When would you use adjacency matrix vs list?"
- After Concept 22 (Hash Map) — "What is a collision and how do you handle it?"
- After Concept 25 (Binary Search) — "Why does binary search require sorted data?"
- After Concept 27 (DFS) — "When do you use BFS vs DFS?"
- After Concept 31 (Quick Sort) — "Why is Quick Sort faster than Merge Sort in practice?"
- After Concept 34 (Two Pointers) — "What makes a problem solvable with two pointers?"
- After Concept 39 (DP) — "What is the difference between memoization and tabulation?"
- After Concept 44 (Final) — Full teach-back. Explain entire journey from memory.

If answer is wrong or vague — correct gently, re-explain, re-test.
Only move forward when answer is clean and confident.

== FINAL TEACH-BACK ==
At the end — run a complete interview simulation:

Round 1 — Concept questions:

- "What data structure would you use to implement a browser back button? Why?"
- "Instagram needs to suggest usernames as you type. What data structure?"
- "Swiggy needs to assign the nearest delivery partner. What data structure?"
- "Google Maps needs shortest route between two cities. What algorithm?"
- "You have 1 billion users. Find if two users are in the same friend group. How?"

Round 2 — Coding problems:

- Two sum (hash map)
- Valid parentheses (stack)
- Binary search on sorted array
- BFS shortest path in a graph
- Longest substring without repeating characters (sliding window)

Round 3 — System design angle:

- "Instagram stores 1 billion posts. How do you search by hashtag efficiently?"
- "WhatsApp needs to deliver messages in order. What ensures this?"
- "A database query was fast last month, slow today. Walk me through diagnosing it."

Push back on anything vague or memorized-sounding.
Only sign off when every answer is clean, confident, and connected to internals.
When I pass, tell me: "You own this now. You can teach it and interview with it."

Start with STEP 0. Ask me the two clarifying questions. Wait for my answers.
