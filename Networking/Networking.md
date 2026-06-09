I want to learn Networking, HTTP, and Web Security from absolute scratch
using First Principles + Spiral Learning.

== MY STARTING POINT ==
I already understand these concepts deeply — build on top of them, never re-explain:

- Memory layout: Stack, Heap, CODE segment, DATA segment
- Processes: OS creates a process, allocates memory, calls main()
- IO vs CPU intensive programs
- Why IO has waiting: CPU is 1000x faster than disk, network, database
- Stack frames: every function call is isolated, dies on return
- Heap: manual in C (malloc/free), automatic in Java (GC)
- Pointers and references

I have built a linked list from scratch in both C and Java.
I understand what a network request IS at a high level —
my app sends something, waits, gets something back. That's IO waiting.
I want to understand what actually happens inside that wait.
And I want to understand how attackers exploit that — and how to stop them.

== MY ENVIRONMENT ==
I am working in: [VS Code on Mac / Windows / Linux — fill this in]

== MY GOAL ==
Phase 1: Understand the network — how data travels from my phone to a server and back.
Phase 2: Understand HTTP — the language apps use to talk to each other.
Phase 3: Understand security — how attackers exploit HTTP and how to defend against it.
Phase 4: Connect everything — end to end, every layer, no magic.

Understand every concept well enough to teach it to someone else.

== STEP 0: CLARIFY BEFORE BUILDING THE PATH ==
Before building anything, ask me these two questions and wait for my answers:
Q1. "What language or environment are you working in?"
Q2. "Is your goal to understand internals deeply, or build something practical, or both?"
Do not infer silently. Do not build the path until I answer both.

== LEARNING PATH ==
Build it for me from my starting point and goal.

Key concepts to cover — validate, reorder, and fill gaps:

── PHASE 1: THE NETWORK ──────────────────────────────────────────

1.  What is a Network? — computers talking to each other.
    How data physically travels — cables, wifi, signals.
    IP addresses — every device has an address.
    Connect to memory addresses: same idea, different scale.
    Public vs private IP. IPv4 vs IPv6 briefly.

2.  Packets — data is not sent in one chunk.
    Broken into small packets, sent separately, reassembled.
    Why? Reliability, routing, sharing the wire.
    What happens if a packet is lost?

3.  Ports — IP address gets you to the right machine.
    Port gets you to the right program on that machine.
    Your machine runs many programs — how does the OS know
    which packet belongs to which program?
    Common ports: 80 (HTTP), 443 (HTTPS), 22 (SSH),
    3306 (MySQL), 5432 (PostgreSQL).

4.  Layers — OSI Model — networking has layers like memory has regions.
    Each layer has one job. Don't memorize — understand
    why each layer exists and what problem it solves.
    Physical → Data Link → Network → Transport →
    Session → Presentation → Application.

5.  TCP vs UDP — two ways to send packets.
    TCP: guaranteed delivery, ordered, slower. Three-way
    handshake. Connect to: like a phone call — establish
    connection first, then talk.
    UDP: fire and forget, faster, no guarantees. Like
    sending a letter — no confirmation.
    Real apps: WhatsApp messages (TCP) vs video call (UDP).

6.  DNS — you type google.com. Computer needs an IP address.
    DNS is the phone book of the internet.
    DNS resolution chain: browser cache → OS cache →
    router → ISP → root → TLD → authoritative.
    What actually happens when you type a URL?
    DNS poisoning — how attackers abuse this.

── PHASE 2: HTTP ─────────────────────────────────────────────────

7.  HTTP — the language browsers and servers speak.
    Request and response. Stateless — every request
    is independent, server remembers nothing.
    Methods: GET, POST, PUT, PATCH, DELETE.
    Headers, body, status codes (200, 404, 500 etc.)
    Connect to what I know: like a function call —
    you send parameters, you get back a return value.

8.  HTTPS & TLS — HTTP is plain text. Anyone between you and the
    server can read it. HTTPS encrypts it.
    TLS handshake — how encryption is negotiated.
    Symmetric vs asymmetric encryption — briefly.
    What actually happens before your first HTTP request?

9.  Certificates & PKI — how does your browser trust a server?
    Certificate Authorities. Chain of trust.
    What is an SSL certificate? Who issues it?
    What happens when a certificate expires?
    Self-signed certificates — why browsers warn you.

10. HTTP/1.1 vs HTTP/2 — HTTP/1.1: one request at a time per connection.
    vs HTTP/3 HTTP/2: multiplexing — many requests, one connection.
    HTTP/3: built on UDP instead of TCP — why?
    How Instagram loads 12 images simultaneously.

11. Cookies & Sessions — HTTP is stateless. Server remembers nothing.
    How does Instagram know you're logged in?
    Cookies: small data stored in browser, sent with
    every request automatically.
    Sessions: server-side storage, cookie holds the key.
    Session hijacking — stealing someone's session.

12. REST APIs — how modern apps talk to each other.
    Resources, endpoints, JSON.
    Stateless, uniform interface, client-server.
    What Swiggy's app does when you place an order —
    every single API call, step by step.
    API versioning — why /api/v1/ exists.

13. Authentication — proving who you are to a server.
    Username/password — how it actually works.
    Hashing passwords — why servers never store
    plain text passwords. bcrypt, salt.
    JWT tokens — what they are, how they work,
    where they're stored, why they expire.
    OAuth — "Login with Google" — what actually happens.

14. WebSockets — HTTP is request-response. What if the server needs
    to push data without you asking?
    WhatsApp messages arriving in real time.
    How WebSocket handshake works on top of HTTP.
    Long polling — the old way of faking real-time.

── PHASE 3: WEB SECURITY ─────────────────────────────────────────

15. Security Mental Model — before attacking/defending: understand the attacker's
    mindset. CIA triad: Confidentiality, Integrity,
    Availability. Every attack targets one of these three.
    Attack surface — what can an attacker reach?

16. CORS — browsers enforce same-origin policy.
    Why can't your frontend call any API it wants?
    What is an origin? (protocol + domain + port)
    Preflight requests — what OPTIONS method does.
    How to configure CORS correctly.
    What happens if you get it wrong.

17. CSRF — Cross-Site Request Forgery.
    You're logged into your bank. You visit a malicious
    site. The site makes a request TO your bank AS you.
    How? Cookies are sent automatically.
    How to defend: CSRF tokens, SameSite cookies.

18. XSS — Cross-Site Scripting.
    Attacker injects malicious JavaScript into a page.
    Stored XSS vs Reflected XSS vs DOM XSS.
    What can an attacker do with XSS?
    Steal cookies, redirect users, keylog.
    How to defend: sanitize input, Content Security Policy.

19. SQL Injection — attacker injects SQL into your database query.
    Classic: ' OR '1'='1 — why this works.
    How to defend: parameterized queries, ORM.
    Why this is still the #1 attack in the world.

20. Man in the Middle — attacker sits between you and the server.
    Reads or modifies traffic.
    Why HTTP is dangerous on public wifi.
    How HTTPS prevents this.
    Certificate pinning — extra layer of defense.

21. Rate Limiting & — what happens if someone sends 1 million requests?
    DDoS Denial of Service. Distributed DoS.
    How Cloudflare and CDNs protect against this.
    Rate limiting — how APIs protect themselves.

22. Security Headers — HTTP response headers that protect the browser.
    Content-Security-Policy, X-Frame-Options,
    Strict-Transport-Security, X-Content-Type-Options.
    What each one does and why it exists.

── PHASE 4: CONNECT EVERYTHING ───────────────────────────────────

23. Caching — not every request needs to hit the server.
    Browser cache, CDN cache, server cache.
    Cache-Control headers. ETags.
    What Cloudflare does when you load a website.
    Cache poisoning — security angle.

24. Load Balancing — one server can't handle millions of requests.
    How traffic is distributed across many servers.
    Round robin, least connections, consistent hashing.
    Sticky sessions — why they exist and their problems.

25. End to End Walkthrough — you open Instagram on your phone.
    Walk through every single step:
    DNS → TCP handshake → TLS handshake →
    Certificate verification → HTTP/2 request →
    Load balancer → Server → Database →
    Response → Browser renders.
    Every layer. Every hop. No magic.

26. Final Teach-Back — explain the full journey from memory.

== TEACHING PATTERN (use this for every single concept) ==

1. Why — What problem does this solve? Why does it exist? What was broken before it?
   → Examples must come from apps I use daily — WhatsApp, Instagram, Swiggy,
   Google Maps, YouTube, Spotify. Technically precise. Not textbook.
   → Connect to what I already know wherever possible:
   "Remember IO waiting? This is WHERE that waiting happens..."
   "Remember memory addresses? IP addresses work similarly..."
   "Remember stack isolation? CORS works on a similar principle..."

2. What — Simplest mental model. Real world analogy. No jargon yet.

3. How — Mechanics. How does it actually work under the hood?
   → Whenever this involves data flow, structure, or layers —
   draw it first using ASCII. Show layouts, arrows, hops, connections.
   → For security concepts: always show the ATTACK first, then the DEFENSE.
   Understanding the attack is what makes the defense feel necessary.

3.5 Check — Before we go deeper, ask me:
"Can you explain what [concept] does in one sentence?"
Re-explain with different analogy if I can't. Don't move forward until clean.

4. Build — Where possible, make me see it in action.
   Use curl, browser dev tools, Wireshark, or code to observe what we just learned.
   Every line explained before writing. No copy-paste.

5. Connect — How this links to the next concept.
   One sentence that makes me curious about what's next.

== RULES ==

- Ask the two clarifying questions first. Wait for answers before building the path.
- Show me the full path. Wait for my approval before teaching.
- Go one concept at a time. Wait for me to say "next" before moving forward.
- No rush. If I give a short reply or seem uncertain — ask if I want re-explanation.
- Never skip a foundation. Flag missing dependencies, slot them in, teach them first.
- Build strictly on top of what I already know. Never re-explain stack, heap, or
  IO from scratch — reference them and build forward.
- Examples must be from apps I use daily. Technically precise.
- Every 3 concepts — quick 2-question recap to keep earlier concepts warm.
- If stuck after 2 re-explanations — diagnose the gap, go back, rebuild forward.
- For security concepts: always show the attack before the defense.
  Never let a defense feel like a rule to memorize — make me feel WHY it exists.
- Follow my code style. Adopt immediately if corrected.

== MILESTONE TEACH-BACKS ==
After every major milestone:

- After Concept 3 (Ports) — "How does OS know which packet belongs to which program?"
- After Concept 6 (DNS) — "Walk me through what happens when you type google.com"
- After Concept 9 (Certificates) — "How does your browser decide to trust a server?"
- After Concept 11 (Cookies) — "How does Instagram know you're still logged in?"
- After Concept 14 (WebSockets) — "How is WebSocket different from HTTP?"
- After Concept 16 (CORS) — "Why can't your frontend call any API it wants?"
- After Concept 19 (SQL Injection) — "What is SQL injection and how do you stop it?"
- After Concept 22 (Security Headers) — "Name three security headers and what they do"
- After Concept 26 (Final) — Full teach-back. Explain entire journey from memory.

If answer is wrong or vague — correct gently, re-explain, re-test.
Only move forward when answer is clean and confident.

== FINAL TEACH-BACK ==
At the end:

- Ask me to explain what happens end to end when I open Instagram — every layer.
- Ask me to explain three different attacks and how to defend against each.
- Push back on anything vague or memorized-sounding.
- Ask follow-up questions like a curious junior developer would.
- Make me connect every concept back to what I already know —
  IO waiting, memory addresses, processes, stack isolation.
- Only sign off when I can teach it cleanly, no notes.
  When I pass, tell me: "You own this now. You can teach it."

Start with STEP 0. Ask me the two clarifying questions. Wait for my answers.
