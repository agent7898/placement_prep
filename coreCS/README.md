# Core CS — Placement Preparation

Educational content for the three core CS subjects covered in Sections 4, 5 and 6 of [Placement_Preparation_Handbook.md](../Placement_Preparation_Handbook.md), expanded into full study material for **online assessments, technical interviews, and rapid revision**.

| File | Subject | Best for |
|---|---|---|
| [Operating_Systems.md](Operating_Systems.md) | Processes, threads, scheduling, synchronization, deadlocks, memory, virtual memory, file systems, disk scheduling, IPC | OA numericals (scheduling, page replacement, disk) + scenario interview questions |
| [Computer_Networks.md](Computer_Networks.md) | OSI/TCP-IP, IP & subnetting, TCP/UDP, HTTP, HTTPS/TLS, DNS, DHCP, ARP, NAT, REST, WebSockets, security | OA MCQs (layers, ports, status codes, subnetting) + "what happens when you type google.com" |
| [DBMS_and_SQL.md](DBMS_and_SQL.md) | Keys, ER design, normalization, SQL from basics to window functions, indexes, transactions, isolation, CAP, MongoDB | Live SQL writing rounds + normalization/ACID theory |

---

## Common structure of each file

Every file follows the same shape, so you can revise them the same way:

1. **Concept sections** — definition → mechanism → trade-off, with tables and worked examples
2. **Rapid revision sheet** — 20 questions answerable in under 30 seconds each
3. **Interview question bank** — with model answers phrased the way you should say them
4. **Traps that cost marks** — the specific mistakes that convert a correct understanding into a wrong answer
5. **Practice set** — problems with collapsible worked solutions

---

## How to study these three subjects

**Depth target (from the handbook):** move from *definition → mechanism → scenario → trade-off*. Reciting "LRU removes the least recently used page" is a weak answer; explaining why it exists, how a HashMap + doubly linked list implements it in O(1), and where it fails is a strong one.

### Suggested sequence

| Week | OS | CN | DBMS/SQL |
|---|---|---|---|
| 1 | Processes, threads, scheduling | OSI/TCP-IP, devices, IP addressing | Keys, ER design, normalization |
| 2 | Synchronization, deadlocks | Subnetting, TCP/UDP, handshakes | SQL basics, joins, aggregation |
| 3 | Memory, paging, virtual memory | HTTP, HTTPS/TLS, DNS/DHCP/ARP/NAT | Subqueries, CTEs, window functions |
| 4 | File systems, disk, IPC | REST, WebSockets, security, request lifecycle | Indexes, transactions, isolation, NoSQL |

Then loop: rapid revision sheets daily, one practice set per subject per week, and spaced repetition at Day 1 / 3 / 7 / 14 / 30.

### Effort split by company type

| Company type | OS | CN | DBMS/SQL |
|---|---|---|---|
| Service-based (TCS, Infosys, Wipro, Accenture, Cognizant) | High — MCQ heavy | High — MCQ heavy | High — SQL + theory |
| Product-based | Medium — scenarios and concurrency | Medium — HTTP/TCP depth | High — schema design + query writing |
| Startups / backend roles | Medium | High — HTTP, REST, TLS, caching | Very high — indexes, transactions, modelling |

### The three things that actually get tested

1. **OS** — can you compute (scheduling, page faults, EAT, head movement) and can you reason about concurrency bugs?
2. **CN** — can you subnet, and can you explain a request end-to-end at multiple levels of detail?
3. **DBMS** — can you *write* a non-trivial SQL query under pressure, and justify a schema?

If you are short on time, prioritize **SQL writing → subnetting → scheduling/page-replacement numericals → the top-20 rapid revision questions in each file**.
