# Computer Networks — Placement Study Guide

> **How to use this file:** CN is the subject interviewers use to test *depth of understanding*, because everyone can recite "TCP is reliable." The differentiator is being able to explain **how** reliability is achieved and **what it costs**. Study each topic at three levels: **definition → mechanism → trade-off**. OAs test layers, protocol/port mapping, TCP vs UDP, status codes, and subnetting arithmetic.

**Priority legend:** 🔴 MUST KNOW · 🟠 HIGH · 🟡 IMPORTANT · 🟢 GOOD TO KNOW

---

## Table of contents

1. [Network fundamentals](#1-network-fundamentals-)
2. [OSI and TCP/IP models](#2-osi-and-tcpip-models-)
3. [Physical and Data Link layer](#3-physical-and-data-link-layer-)
4. [Network layer: IP, subnetting, routing](#4-network-layer-)
5. [Transport layer: TCP and UDP](#5-transport-layer-)
6. [Application layer protocols](#6-application-layer-)
7. [HTTP in depth](#7-http-in-depth-)
8. [HTTPS and TLS](#8-https-and-tls-)
9. [DNS](#9-dns-)
10. [DHCP, ARP, NAT](#10-dhcp-arp-and-nat-)
11. [REST and WebSockets](#11-rest-and-websockets-)
12. [Security essentials](#12-security-essentials-)
13. ["What happens when you type google.com"](#13-what-happens-when-you-type-googlecom-)
14. [Rapid revision sheet](#14-rapid-revision-sheet)
15. [Interview question bank with model answers](#15-interview-question-bank-with-model-answers)
16. [Traps that cost marks](#16-traps-that-cost-marks)
17. [Practice set with solutions](#17-practice-set-with-solutions)

---

## 1. Network fundamentals 🔴

A **computer network** is a set of devices connected so they can exchange data.

### By scale

| Type | Span | Example |
|---|---|---|
| PAN | A few metres | Bluetooth earbuds |
| LAN | Building/campus | Office Ethernet + Wi-Fi |
| MAN | City | Cable/metro network |
| WAN | Country/global | The Internet |

### Topologies 🟡

| Topology | Structure | Pro | Con |
|---|---|---|---|
| Bus | One shared backbone | Cheap | Single point of failure; collisions |
| Star | All nodes → central switch | Easy to manage; failure isolated | Hub/switch is a single point of failure |
| Ring | Circular | Predictable latency | One break can kill the ring |
| Mesh | Everyone to everyone | Highly fault tolerant | Expensive, *n(n−1)/2* links |
| Hybrid | Combination | Practical | Complex |

### Transmission modes
**Simplex** (one direction only) · **Half-duplex** (both directions, one at a time — walkie-talkie) · **Full-duplex** (both simultaneously — telephone, switched Ethernet).

### Key performance terms 🔴

| Term | Meaning |
|---|---|
| **Bandwidth** | Maximum data rate the link supports (bits/second) — *capacity* |
| **Throughput** | Actual data rate achieved — always ≤ bandwidth |
| **Latency** | Time for one bit to travel source → destination |
| **Jitter** | Variation in latency (kills VoIP/video quality) |
| **Packet loss** | Fraction of packets that never arrive |
| **RTT** | Round-Trip Time — request out plus response back |

**Latency has four components** (be able to list these — it's a strong answer):
1. **Transmission delay** = packet size / bandwidth
2. **Propagation delay** = distance / propagation speed
3. **Queuing delay** — time waiting in router buffers (the variable one)
4. **Processing delay** — header inspection, checksum, routing lookup

> **Bandwidth vs latency analogy:** bandwidth is the number of lanes on a highway; latency is how long the drive takes. Adding lanes doesn't make the trip shorter.

### Network devices 🔴

| Device | OSI layer | Function |
|---|---|---|
| Hub | 1 (Physical) | Repeats bits to **all** ports — one collision domain, wasteful |
| Repeater | 1 | Regenerates the signal to extend distance |
| Switch | 2 (Data Link) | Forwards frames by **MAC address**; each port is its own collision domain |
| Bridge | 2 | Connects two LAN segments |
| Router | 3 (Network) | Forwards packets between **networks** using IP; separates broadcast domains |
| Gateway | Up to 7 | Translates between different protocols/architectures |
| Firewall | 3–7 | Filters traffic by rules |
| Load balancer | 4 or 7 | Distributes traffic across servers |

> **Hub vs switch vs router (guaranteed MCQ):** hub = dumb electrical repeater (layer 1); switch = MAC-based forwarding within a LAN (layer 2); router = IP-based forwarding between LANs (layer 3). Switches split **collision** domains; routers split **broadcast** domains.

### Circuit vs packet switching 🟡

| Circuit switching | Packet switching |
|---|---|
| Dedicated path reserved for the whole session | Data split into packets routed independently |
| Guaranteed bandwidth, constant delay | Variable delay, possible loss/reordering |
| Wasteful when idle | Efficient statistical multiplexing |
| Traditional telephony | The Internet |

---

## 2. OSI and TCP/IP models 🔴

### OSI — 7 layers

Mnemonic (top → bottom): **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

| # | Layer | Job | PDU | Examples |
|---|---|---|---|---|
| 7 | **Application** | Interface to user applications | Data | HTTP, FTP, SMTP, DNS, DHCP |
| 6 | **Presentation** | Translation, encryption, compression | Data | TLS/SSL, JPEG, ASCII |
| 5 | **Session** | Establish/manage/terminate sessions | Data | NetBIOS, RPC, session tokens |
| 4 | **Transport** | End-to-end delivery, segmentation, reliability, ports | **Segment** (TCP) / **Datagram** (UDP) | TCP, UDP |
| 3 | **Network** | Logical addressing, routing between networks | **Packet** | IP, ICMP, OSPF, BGP |
| 2 | **Data Link** | Node-to-node delivery on a link, MAC addressing, error detection | **Frame** | Ethernet, PPP, ARP*, switches |
| 1 | **Physical** | Bits over the medium | **Bit** | Cables, radio, hubs, voltages |

\* ARP is usually placed at layer 2/3 boundary; textbooks vary. If asked, say "layer 2, bridging to layer 3."

**Data Link has two sublayers:** **LLC** (Logical Link Control — flow/error control, protocol multiplexing) and **MAC** (Media Access Control — channel access, physical addressing).

### TCP/IP model — 4 layers (what the Internet actually uses)

| TCP/IP layer | Corresponding OSI layers |
|---|---|
| Application | Application + Presentation + Session (5–7) |
| Transport | Transport (4) |
| Internet | Network (3) |
| Network Access / Link | Data Link + Physical (1–2) |

| OSI | TCP/IP |
|---|---|
| 7 layers | 4 layers |
| Reference/theoretical model | Implemented, practical |
| Strict layer separation | More pragmatic, layers overlap |
| Developed by ISO | Developed by DARPA |
| Protocol-independent standard | Built around specific protocols |

### Encapsulation 🔴

As data goes **down** the stack, each layer adds its own header:

```
Application:   [        Data        ]
Transport:     [TCP hdr][   Data    ]        → Segment
Network:       [IP hdr][TCP hdr][Data]       → Packet
Data Link:     [Eth hdr][IP hdr][TCP hdr][Data][Eth trailer/FCS]  → Frame
Physical:      101101000111010...            → Bits
```

The reverse on the receiving side is **decapsulation**. Each layer only reads its own header — this is why layering works.

---

## 3. Physical and Data Link layer 🟠

### Framing and error control

- **Error detection:** parity bit, checksum, **CRC** (Cyclic Redundancy Check — used by Ethernet, very strong for burst errors).
- **Error correction:** Hamming code (can correct single-bit errors).
- **Error recovery protocols (ARQ):**

| Protocol | Idea | Efficiency |
|---|---|---|
| Stop-and-Wait | Send one frame, wait for ACK | Simple, very poor on high bandwidth-delay links |
| Go-Back-N | Window of N unacked frames; on error, retransmit from the lost one | Receiver needs no buffering; wasteful retransmits |
| Selective Repeat | Retransmit only the lost frame | Efficient; receiver must buffer and reorder |

**Sliding window sizes:** Go-Back-N needs sender window ≤ 2ᵏ − 1 (k = sequence number bits); Selective Repeat needs both windows ≤ 2ᵏ⁻¹.

### MAC / media access 🟡

- **CSMA/CD** (Ethernet, legacy shared media): Carrier Sense Multiple Access with **Collision Detection** — listen before sending; if a collision is detected, stop, send a jam signal, back off exponentially. Irrelevant on modern full-duplex switched Ethernet.
- **CSMA/CA** (Wi-Fi): **Collision Avoidance** — wireless nodes can't reliably detect collisions while transmitting, so they avoid them using random backoff and optional RTS/CTS.

### MAC address vs IP address 🔴

| MAC address | IP address |
|---|---|
| Layer 2 | Layer 3 |
| 48 bits, hex, e.g. `00:1A:2B:3C:4D:5E` | 32 bits (IPv4) / 128 bits (IPv6) |
| Burned into the NIC (physically assigned) | Assigned logically by the network/DHCP |
| Flat address space — no hierarchy | Hierarchical — enables routing |
| Used for delivery **within a LAN** | Used for delivery **across networks** |
| Changes at every hop | Stays the same end-to-end (unless NAT) |

> **The key insight:** As a packet travels, the **IP header's source/destination stay constant** (end-to-end identity), while the **MAC addresses are rewritten at every router hop** (hop-to-hop delivery). Saying this cleanly signals real understanding.

### VLAN 🟢
A logically segmented broadcast domain within a physical switch infrastructure. Used to separate departments without extra hardware.

---

## 4. Network layer 🔴

### IPv4 addressing

32 bits, written as four dotted decimal octets: `192.168.1.10`.

**Classes (legacy but heavily asked in OAs):**

| Class | First octet range | Default mask | Purpose |
|---|---|---|---|
| A | 1–126 | /8 (255.0.0.0) | Very large networks |
| B | 128–191 | /16 (255.255.0.0) | Medium networks |
| C | 192–223 | /24 (255.255.255.0) | Small networks |
| D | 224–239 | — | **Multicast** |
| E | 240–255 | — | Experimental/reserved |

- **127.x.x.x** = loopback (`127.0.0.1` = localhost). Note this is why class A stops at 126.
- **169.254.x.x** = APIPA/link-local (what you get when DHCP fails).
- **0.0.0.0** = "this host"/any interface.

**Private address ranges** 🔴 (must memorize — they are not routable on the public Internet):
- `10.0.0.0 – 10.255.255.255` (10.0.0.0/8)
- `172.16.0.0 – 172.31.255.255` (172.16.0.0/12)
- `192.168.0.0 – 192.168.255.255` (192.168.0.0/16)

### Subnetting 🔴 — the most common CN numerical

A **subnet mask** splits the address into a **network portion** (1 bits) and **host portion** (0 bits). **CIDR notation** `/n` means the first *n* bits are network bits.

**Formulas:**
- Number of host bits = 32 − n
- **Total addresses per subnet = 2^(32−n)**
- **Usable hosts = 2^(32−n) − 2** (subtract the network address and the broadcast address)
- Number of subnets from borrowing *b* bits = 2^b
- **Block size = 256 − (the interesting octet of the mask)**

**Mask cheat sheet:**

| CIDR | Mask | Block | Usable hosts |
|---|---|---|---|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 (point-to-point links) |
| /22 | 255.255.252.0 | 4 (3rd octet) | 1022 |
| /16 | 255.255.0.0 | — | 65534 |

**Worked example 1:** Subnet `192.168.10.0/24` into 4 subnets.
Need 4 subnets → borrow 2 bits → /26. Block size = 256 − 192 = 64.

| Subnet | Network | Usable range | Broadcast |
|---|---|---|---|
| 1 | 192.168.10.0 | .1 – .62 | 192.168.10.63 |
| 2 | 192.168.10.64 | .65 – .126 | 192.168.10.127 |
| 3 | 192.168.10.128 | .129 – .190 | 192.168.10.191 |
| 4 | 192.168.10.192 | .193 – .254 | 192.168.10.255 |

**Worked example 2:** Given host `172.16.45.130/22`, find network address, broadcast, and usable range.
- /22 → mask 255.255.252.0 → interesting octet is the 3rd; block size = 256 − 252 = **4**.
- Third octet 45 → largest multiple of 4 that is ≤ 45 is **44**.
- **Network = 172.16.44.0**
- Next network = 172.16.48.0 → **Broadcast = 172.16.47.255**
- **Usable = 172.16.44.1 – 172.16.47.254** → 2¹⁰ − 2 = **1022 hosts**

**Method to memorize (works every time):**
1. Convert CIDR to mask; find the "interesting" octet (the one that isn't 255 or 0).
2. Block size = 256 − mask value in that octet.
3. Network address = largest multiple of the block size ≤ the host's value in that octet.
4. Broadcast = next network − 1. Usable = network+1 to broadcast−1.

**VLSM (Variable Length Subnet Masking):** assign different mask lengths to subnets to match actual host counts, avoiding waste. Always allocate the **largest requirement first**.

### IPv4 header 🟡
Minimum **20 bytes**, maximum 60. Fields worth knowing: Version, IHL, **TTL**, Protocol (6 = TCP, 17 = UDP, 1 = ICMP), Header Checksum, Source IP, Destination IP, Identification/Flags/Fragment Offset (for fragmentation).

**TTL (Time To Live)** — decremented by 1 at each router; at 0 the packet is dropped and an ICMP "Time Exceeded" is returned. Prevents infinite routing loops, and is exactly how **traceroute** works (send packets with TTL = 1, 2, 3, … and record who complains).

**Fragmentation:** if a packet exceeds the link **MTU** (typically 1500 bytes for Ethernet), IPv4 routers may fragment it; reassembly happens only at the destination. IPv6 removed router fragmentation — the source must do Path MTU Discovery.

### IPv4 vs IPv6 🔴

| IPv4 | IPv6 |
|---|---|
| 32 bits (~4.3 billion addresses) | 128 bits (3.4 × 10³⁸) |
| Dotted decimal `192.168.1.1` | Hex, colon-separated `2001:db8::1` |
| Header 20–60 bytes, variable | Header fixed 40 bytes, simpler |
| Has a header checksum | **No header checksum** (relies on L2/L4) |
| Broadcast supported | **No broadcast** — uses multicast/anycast |
| Fragmentation by routers and host | Only by the source |
| NAT widely required | Huge space makes NAT largely unnecessary |
| DHCP for configuration | SLAAC (stateless autoconfiguration) + DHCPv6 |
| ARP for address resolution | **NDP** (Neighbor Discovery Protocol) |
| IPsec optional | IPsec designed in |

### Routing 🟠

**Routing** = determining the path (control plane). **Forwarding** = moving a packet to the next hop (data plane).

| Category | Protocols | Idea |
|---|---|---|
| **Distance Vector** | RIP | Each router shares its whole routing table with neighbours; uses Bellman-Ford; hop-count metric (RIP max 15); suffers **count-to-infinity** (mitigated by split horizon, poison reverse) |
| **Link State** | OSPF, IS-IS | Each router floods link states to all; every router builds a full map and runs **Dijkstra**; faster convergence, more CPU/memory |
| **Path Vector** | **BGP** | Between autonomous systems (the Internet's routing protocol); policy-based, carries full AS path |

**IGP vs EGP:** IGP (RIP, OSPF) routes *within* an autonomous system; EGP (BGP) routes *between* autonomous systems.

**Static vs dynamic routing:** static = manually configured, predictable, no overhead, no failover; dynamic = adapts automatically, costs bandwidth/CPU.

### ICMP 🟠
A control/error-reporting protocol at layer 3 (not a transport protocol; it carries no user data).
- **`ping`** — Echo Request / Echo Reply. Tests reachability and measures RTT.
- **`traceroute`/`tracert`** — TTL trick above; maps the hop-by-hop path.
- Other messages: Destination Unreachable, Time Exceeded, Redirect.
- Firewalls often block ICMP, which is why a host can be up while ping fails.

---

## 5. Transport layer 🔴

Responsibilities: **process-to-process delivery** (via ports), segmentation/reassembly, and — for TCP — reliability, ordering, flow control, and congestion control.

### Ports 🔴
A 16-bit number (0–65535) identifying an application endpoint. **Socket = IP address + port**. A TCP connection is uniquely identified by the **4-tuple**: (source IP, source port, destination IP, destination port).

- **Well-known:** 0–1023 · **Registered:** 1024–49151 · **Ephemeral/dynamic:** 49152–65535

**Memorize this table** — it appears in nearly every OA:

| Port | Protocol | Port | Protocol |
|---|---|---|---|
| 20/21 | FTP (data/control) | 80 | HTTP |
| 22 | SSH / SFTP | 110 | POP3 |
| 23 | Telnet | 143 | IMAP |
| 25 | SMTP | 443 | HTTPS |
| 53 | DNS (UDP mostly, TCP for zone transfer & large responses) | 3306 | MySQL |
| 67/68 | DHCP (server/client) | 5432 | PostgreSQL |
| 69 | TFTP (UDP) | 27017 | MongoDB |

### TCP vs UDP 🔴 (the single most asked CN question)

| | **TCP** | **UDP** |
|---|---|---|
| Connection | Connection-oriented (handshake first) | Connectionless |
| Reliability | Guaranteed delivery via ACK + retransmission | Best-effort, no guarantee |
| Ordering | In-order delivery guaranteed | No ordering guarantee |
| Flow control | Yes (sliding window) | No |
| Congestion control | Yes | No |
| Error handling | Checksum + retransmit | Checksum, then **discard** |
| Header size | **20 bytes** (up to 60) | **8 bytes** |
| Speed/overhead | Slower, higher overhead | Faster, minimal overhead |
| Data transfer | Byte stream | Message/datagram oriented (preserves boundaries) |
| Broadcast/multicast | Not supported | Supported |
| Use cases | HTTP(S), FTP, SMTP, SSH, database connections, file transfer | DNS, DHCP, VoIP, video streaming, online gaming, SNMP, TFTP |

**How to answer "when would you choose UDP?"** — when *timeliness beats completeness*: in live video or voice, a retransmitted frame arrives too late to be useful, so you'd rather drop it and keep playing. Also when the application implements its own reliability (QUIC, game netcode), or for single request/response exchanges where a handshake would double the latency (DNS).

### TCP header fields 🟠
Source port, destination port, **sequence number**, **acknowledgement number**, data offset, **flags (URG, ACK, PSH, RST, SYN, FIN)**, **window size**, checksum, urgent pointer, options.

### Three-way handshake 🔴

```
Client                                Server
  │  ── SYN,  seq = x ──────────────► │     (client: SYN-SENT)
  │  ◄── SYN+ACK, seq = y, ack = x+1 ─│     (server: SYN-RECEIVED)
  │  ── ACK,  seq = x+1, ack = y+1 ─► │     (both: ESTABLISHED)
```

**Why three and not two?** Both sides must synchronize sequence numbers **and** confirm the other side can both send and receive. A two-way exchange would leave the client's ability to receive unconfirmed and would allow stale duplicate SYNs to open phantom connections.

**SYN flood attack:** an attacker sends many SYNs with spoofed sources and never completes the handshake, exhausting the server's half-open connection table. Mitigations: **SYN cookies**, reduced backlog timeouts, rate limiting.

### Four-way termination 🔴

```
Client                          Server
  │ ── FIN ──────────────────►  │
  │ ◄── ACK ────────────────── │      (client: FIN-WAIT-2, server: CLOSE-WAIT)
  │ ◄── FIN ────────────────── │      (server: LAST-ACK)
  │ ── ACK ──────────────────►  │      (client: TIME-WAIT → CLOSED)
```

**Why four and not three?** TCP connections are **full-duplex** — each direction is closed independently. After receiving a FIN, the server may still have data to send, so its ACK and its own FIN are usually separate segments (they can be combined into three if it has nothing left to send).

**TIME-WAIT state** (asked in good interviews): the initiator waits **2 × MSL** (Maximum Segment Lifetime) before fully closing, to (a) ensure the final ACK reaches the peer, and (b) let old duplicate segments die out so they can't be misdelivered to a new connection reusing the same 4-tuple.

### Flow control vs congestion control 🔴 — never confuse these

| **Flow control** | **Congestion control** |
|---|---|
| Protects the **receiver** | Protects the **network** |
| Prevents a fast sender from overwhelming a slow receiver | Prevents senders from overwhelming routers/links |
| End-to-end, between two hosts | Involves the whole network path |
| Mechanism: **receiver-advertised window** (rwnd) in every ACK | Mechanism: **congestion window** (cwnd), computed by the sender from loss/delay signals |

The sender's effective window = **min(rwnd, cwnd)**.

**Sliding window** — the sender may have up to `window` bytes unacknowledged in flight; as ACKs arrive the window slides forward. This is what makes TCP fast: without it you'd pay one RTT per segment.

**Silly Window Syndrome** — tiny window advertisements cause tiny, inefficient segments. Fixed by **Nagle's algorithm** (sender side: buffer small writes until the previous data is ACKed) and **Clark's solution** (receiver side: don't advertise tiny windows).

### TCP congestion control 🟠

Four phases:

1. **Slow start** — cwnd starts at 1 MSS and **doubles every RTT** (exponential) until it reaches `ssthresh`.
2. **Congestion avoidance** — cwnd grows by ~1 MSS per RTT (**additive increase**, linear).
3. **Fast retransmit** — on **3 duplicate ACKs**, retransmit the missing segment immediately without waiting for a timeout.
4. **Fast recovery** — after fast retransmit, halve cwnd and continue in congestion avoidance rather than dropping to 1.

**Response to loss:**
- **Timeout** (severe): ssthresh = cwnd/2, cwnd = 1, restart slow start.
- **3 duplicate ACKs** (mild): **TCP Reno** halves cwnd and enters fast recovery. **TCP Tahoe** drops to 1 in both cases.

The overall pattern is **AIMD** — Additive Increase, Multiplicative Decrease — which is what makes TCP fair across flows sharing a bottleneck.

> **Bandwidth-Delay Product (BDP)** = bandwidth × RTT = the amount of data that can be "in flight." A window smaller than the BDP under-utilizes the link. This is why long fat networks need window scaling.

### Reliability mechanisms, summarized
Sequence numbers (ordering + duplicate detection) · cumulative ACKs (+ SACK option for selective acknowledgement) · checksums · retransmission timers with **adaptive RTO** from smoothed RTT estimates (Karn's algorithm avoids sampling retransmitted segments).

---

## 6. Application layer 🔴

| Protocol | Transport/Port | Purpose |
|---|---|---|
| HTTP/HTTPS | TCP 80/443 | Web |
| DNS | UDP 53 (TCP for large/zone transfers) | Name → IP resolution |
| DHCP | UDP 67/68 | Automatic IP configuration |
| SMTP | TCP 25 (587 submission) | **Sending** mail |
| POP3 | TCP 110 | **Downloading** mail (typically deletes from server) |
| IMAP | TCP 143 | **Syncing** mail (keeps it on the server, multi-device) |
| FTP | TCP 20/21 | File transfer (separate control and data channels) |
| SFTP/SSH | TCP 22 | Secure shell and file transfer |
| Telnet | TCP 23 | Remote login, **unencrypted** — replaced by SSH |
| SNMP | UDP 161/162 | Network device monitoring |
| NTP | UDP 123 | Time synchronization |

> **POP3 vs IMAP** is a common one-liner: POP3 downloads and typically deletes (single device); IMAP keeps mail on the server and synchronizes state across devices.

---

## 7. HTTP in depth 🔴

**HTTP is a stateless, text-based request/response protocol** running over TCP (over QUIC/UDP in HTTP/3).

### Methods

| Method | Purpose | **Safe?** | **Idempotent?** | Body |
|---|---|---|---|---|
| GET | Retrieve a resource | ✔ | ✔ | No |
| HEAD | Like GET but headers only | ✔ | ✔ | No |
| OPTIONS | Query supported methods (CORS preflight) | ✔ | ✔ | No |
| POST | Create / submit; non-idempotent action | ✘ | ✘ | Yes |
| PUT | Replace a resource entirely | ✘ | ✔ | Yes |
| PATCH | Partial update | ✘ | ✘ (generally) | Yes |
| DELETE | Remove a resource | ✘ | ✔ | Optional |

- **Safe** = does not modify server state.
- **Idempotent** = performing it N times has the same effect as performing it once. *This is the concept interviewers probe:* `DELETE /users/5` twice leaves the same end state (deleted), so it's idempotent even though the second call returns 404. `POST /orders` twice creates two orders, so it isn't.

**GET vs POST** 🔴: GET puts parameters in the URL (visible, logged, length-limited, cacheable, bookmarkable); POST puts them in the body (not logged in URLs, no practical length limit, not cached by default). GET must never be used for state-changing operations.

### Status codes 🔴

| Code | Meaning | When you'd use it |
|---|---|---|
| **200** OK | Success | Standard successful GET |
| **201** Created | Resource created | Successful POST; include a `Location` header |
| **204** No Content | Success, empty body | Successful DELETE |
| **301** Moved Permanently | Permanent redirect | Domain/URL change; browsers cache it |
| **302** Found | Temporary redirect | Temporary relocation |
| **304** Not Modified | Cached copy is still valid | Conditional GET with `If-None-Match`/`If-Modified-Since` |
| **400** Bad Request | Malformed request | Validation failure |
| **401** Unauthorized | **Not authenticated** | Missing/invalid credentials |
| **403** Forbidden | **Authenticated but not allowed** | Insufficient permissions |
| **404** Not Found | Resource doesn't exist | — |
| **405** Method Not Allowed | Wrong verb for this endpoint | — |
| **409** Conflict | State conflict | Duplicate registration, concurrent edit |
| **422** Unprocessable Entity | Syntactically valid, semantically wrong | Business-rule validation |
| **429** Too Many Requests | Rate limited | Include `Retry-After` |
| **500** Internal Server Error | Unhandled server fault | — |
| **502** Bad Gateway | Upstream returned an invalid response | Proxy/gateway issue |
| **503** Service Unavailable | Overloaded or in maintenance | — |
| **504** Gateway Timeout | Upstream didn't respond in time | — |

> **401 vs 403 is the classic trap:** 401 = "I don't know who you are"; 403 = "I know who you are, and you still can't."

### Important headers 🟠

**Request:** `Host`, `User-Agent`, `Accept`, `Authorization`, `Cookie`, `Content-Type`, `If-None-Match`, `Origin`.
**Response:** `Content-Type`, `Content-Length`, `Set-Cookie`, `Cache-Control`, `ETag`, `Location`, `Access-Control-Allow-Origin`, `Strict-Transport-Security`.

### HTTP versions 🟠

| Version | Key changes |
|---|---|
| **HTTP/1.0** | New TCP connection per request — very slow |
| **HTTP/1.1** | **Persistent connections** (keep-alive), pipelining, `Host` header (enables virtual hosting), chunked transfer |
| **HTTP/2** | Binary framing, **multiplexing** many streams over one TCP connection, header compression (HPACK), server push. Solves *application-layer* head-of-line blocking |
| **HTTP/3** | Runs over **QUIC on UDP**; solves **TCP-level head-of-line blocking**, faster connection setup (TLS built in, 0/1-RTT), better mobility across network changes |

**Head-of-line blocking:** in HTTP/1.1, one slow response blocks the rest on that connection. HTTP/2 fixes this at the HTTP layer, but a single lost TCP segment still stalls *all* streams — because TCP guarantees ordering. HTTP/3 fixes that by using QUIC's independent streams. This is an excellent thing to be able to explain.

### Statelessness, cookies, and sessions 🔴

HTTP is stateless — the server does not remember previous requests. State is added on top:

| **Cookie** | **Session** |
|---|---|
| Stored **client-side** | Stored **server-side** |
| Sent with every request to the domain | Identified by a session ID (usually in a cookie) |
| Size-limited (~4 KB) | Limited only by server storage |
| Less secure — visible/tamperable unless signed | More secure; client only holds an opaque ID |
| Can persist after the browser closes (`Expires`/`Max-Age`) | Typically expires on timeout/logout |

**Cookie security flags to name-drop:** `HttpOnly` (JS can't read it → mitigates XSS theft), `Secure` (HTTPS only), `SameSite=Strict/Lax` (mitigates CSRF), `Domain`/`Path` scoping.

**Session vs JWT (very common in project interviews):** sessions are stateful, easy to revoke, need shared storage to scale horizontally; JWTs are stateless and self-contained, scale trivially, but are **hard to revoke before expiry** — which is why refresh tokens and short access-token lifetimes exist.

### Caching 🟠
`Cache-Control: max-age=…, no-cache, no-store, private/public` · `ETag` + `If-None-Match` → **304 Not Modified** · `Last-Modified` + `If-Modified-Since`. A **CDN** caches static assets at edge locations near users, cutting latency and origin load.

### CORS 🟠
Browsers enforce the **Same-Origin Policy**: a page at origin A cannot read responses from origin B by default. **CORS** is the server's way to opt in, using `Access-Control-Allow-Origin` and friends. For "non-simple" requests the browser first sends an **OPTIONS preflight**. Two things worth saying: (1) CORS is enforced by the *browser*, not the server — curl ignores it; (2) it protects the *user*, not the server.

---

## 8. HTTPS and TLS 🔴

**HTTPS = HTTP over TLS.** TLS provides three guarantees:

1. **Confidentiality** — data is encrypted, so eavesdroppers see ciphertext.
2. **Integrity** — MAC/AEAD tags detect tampering.
3. **Authentication** — certificates prove the server is who it claims to be.

### Symmetric vs asymmetric 🔴

| Symmetric | Asymmetric |
|---|---|
| One shared secret key | Public/private key pair |
| Fast | Slow (orders of magnitude) |
| Problem: how do you share the key? | Solves key distribution |
| AES, ChaCha20 | RSA, ECDSA, Diffie-Hellman |

**TLS uses both:** asymmetric crypto to authenticate and to establish a shared secret, then fast symmetric crypto for the actual data. Say this — it's the whole point of the design.

### TLS handshake (1.2, conceptually)

1. **ClientHello** — supported TLS versions, cipher suites, client random.
2. **ServerHello** — chosen version and cipher suite, server random.
3. **Certificate** — server sends its X.509 certificate chain.
4. Client **validates** the certificate: signature chains to a trusted root CA, domain matches, not expired, not revoked.
5. **Key exchange** — typically ephemeral Diffie-Hellman (ECDHE) → both derive the same **session key** without ever transmitting it.
6. **Finished** — both switch to symmetric encryption and verify the handshake wasn't tampered with.

**TLS 1.3** simplified this to **one round trip** (and 0-RTT for resumption), removed insecure ciphers and static RSA key exchange, and makes **forward secrecy** mandatory.

**Forward secrecy** — because the session key comes from ephemeral keys, an attacker who later steals the server's private key still cannot decrypt recorded past sessions.

### Certificates and PKI 🟠
A **certificate** binds a public key to an identity and is **signed by a Certificate Authority**. Your browser ships with a trusted root store; a chain root → intermediate → leaf must validate. A **self-signed certificate** encrypts fine but proves no identity, hence the browser warning.

### HTTP vs HTTPS

| HTTP | HTTPS |
|---|---|
| Port 80 | Port 443 |
| Plaintext | Encrypted |
| No server authentication | Certificate-based authentication |
| Vulnerable to eavesdropping and MITM | Protected |
| Slightly lower overhead | Handshake + encryption cost (negligible today) |

**HSTS** (`Strict-Transport-Security`) tells browsers to only ever use HTTPS for this domain, closing the initial-plaintext-redirect window.

---

## 9. DNS 🔴

**DNS** translates human-readable names into IP addresses. It is a **hierarchical, distributed** database — usually over **UDP port 53** (TCP for responses over 512 bytes and for zone transfers).

### Resolution flow (recursive + iterative)

```
Browser cache → OS cache → hosts file → Local/ISP resolver
                                            │  (resolver does iterative queries)
                                            ├──► Root server        → "ask .com TLD"
                                            ├──► TLD server (.com)  → "ask ns1.google.com"
                                            └──► Authoritative NS   → "142.250.x.x"
                                            │
                     answer cached at each level per TTL, returned to browser
```

**Recursive query** — the client asks the resolver for a final answer and the resolver does all the work. **Iterative query** — each server responds with a referral to the next server. Your machine makes a recursive query; the resolver makes iterative ones.

### Record types 🔴

| Record | Meaning |
|---|---|
| **A** | Hostname → IPv4 address |
| **AAAA** | Hostname → IPv6 address |
| **CNAME** | Alias → another hostname |
| **MX** | Mail exchange server (with priority) |
| **NS** | Authoritative name servers for the zone |
| **TXT** | Arbitrary text — SPF, DKIM, domain verification |
| **SOA** | Start of Authority — zone metadata, serial, TTLs |
| **PTR** | IP → hostname (reverse DNS) |
| **SRV** | Service location (host + port) |

**TTL** controls how long each cache may hold a record — this is why DNS changes "take time to propagate" (they don't propagate; old cached entries simply expire).

**DNS load balancing:** returning multiple A records / rotating them, or geo-based responses (GeoDNS), is a cheap first layer of load distribution.

**Security:** **DNS spoofing/cache poisoning** injects false records; **DNSSEC** adds cryptographic signatures to records to prevent it. **DoH/DoT** encrypt DNS queries themselves.

---

## 10. DHCP, ARP, and NAT 🔴

### DHCP — Dynamic Host Configuration Protocol

Automatically assigns IP address, subnet mask, default gateway, and DNS servers. Four-step **DORA** process (UDP, client port 68 → server port 67):

```
1. DISCOVER  — client broadcasts "is there a DHCP server?"      (from 0.0.0.0 to 255.255.255.255)
2. OFFER     — server offers an IP address and options
3. REQUEST   — client broadcasts acceptance of one offer
4. ACK       — server confirms and commits the lease
```

The final REQUEST is broadcast so that **other** DHCP servers know their offers were declined. Addresses are **leased** with an expiry and renewed at ~50% of the lease time. If no server responds, the host self-assigns an **APIPA** address (169.254.x.x) and can only talk to the local link.

### ARP — Address Resolution Protocol 🔴

Maps a known **IP address → MAC address** within a LAN.

1. Host needs to send to 192.168.1.5 but has no MAC for it.
2. **ARP Request** is **broadcast**: "Who has 192.168.1.5? Tell 192.168.1.10."
3. The owner replies with a **unicast ARP Reply** containing its MAC.
4. The mapping is stored in the **ARP cache** with a timeout.

Related: **RARP** (MAC → IP, obsolete, replaced by DHCP), **Gratuitous ARP** (announce your own mapping), **ARP spoofing/poisoning** (attacker forges replies to become a man-in-the-middle — mitigated with dynamic ARP inspection or static entries).

> If the destination is on a **different** network, ARP resolves the **default gateway's** MAC, not the destination's. This is a favourite exam distinction.

### NAT — Network Address Translation 🔴

Translates private addresses to a public address so many internal hosts can share one public IP.

| Type | Mapping |
|---|---|
| Static NAT | One private ↔ one public, fixed |
| Dynamic NAT | Private → any free public IP from a pool |
| **PAT / NAT overload** | Many private → **one** public IP, distinguished by **port number**. This is what home routers do. |

**Benefits:** conserves IPv4 addresses; hides internal topology (a side-effect security benefit).
**Costs:** breaks end-to-end connectivity; inbound connections need port forwarding; complicates P2P (hence STUN/TURN/ICE); breaks protocols that embed IPs in payloads.

---

## 11. REST and WebSockets 🔴

### REST

REST (REpresentational State Transfer) is an **architectural style**, not a protocol. Constraints:

1. **Client–server** separation.
2. **Stateless** — every request carries all the context it needs; the server keeps no client session state.
3. **Cacheable** — responses declare their cacheability.
4. **Uniform interface** — resources identified by URIs, manipulated through representations, self-descriptive messages, HATEOAS.
5. **Layered system** — proxies/gateways can sit in between transparently.
6. **Code on demand** (optional).

**Good REST design:**

```
GET    /api/users          → list users
GET    /api/users/42       → one user
POST   /api/users          → create (201 + Location)
PUT    /api/users/42       → full replace
PATCH  /api/users/42       → partial update
DELETE /api/users/42       → delete (204)
GET    /api/users/42/orders → nested resource
```

Rules: nouns not verbs (`/getUsers` is wrong), plural collection names, versioning (`/api/v1/…`), meaningful status codes, pagination (`?page=2&limit=20`), filtering/sorting via query params.

> **Interview trap:** "REST is just JSON over HTTP" is wrong. REST is about resources, uniform interface, and statelessness; the representation format is incidental.

**REST vs GraphQL vs gRPC** 🟡: REST — multiple endpoints, risk of over/under-fetching, great caching. GraphQL — single endpoint, client specifies exactly the fields it wants, solves over-fetching, but caching and rate-limiting are harder. gRPC — HTTP/2 + Protocol Buffers, binary, fast, strongly typed, streaming; ideal for internal service-to-service, awkward directly from browsers.

### WebSockets 🔴

A **persistent, full-duplex** connection over a single TCP connection. It starts as an HTTP request with `Upgrade: websocket` and, after a `101 Switching Protocols` response, the connection becomes a bidirectional message channel (`ws://` / `wss://`).

| **HTTP** | **WebSocket** |
|---|---|
| Request–response, client-initiated | Full-duplex, either side can send |
| New request per exchange | One long-lived connection |
| Header overhead per message | Small frame overhead after handshake |
| Stateless | Stateful connection |
| Web pages, REST APIs | Chat, live notifications, collaborative editing, trading dashboards, multiplayer games |

**Alternatives to know:** **short polling** (repeated requests — simple, wasteful), **long polling** (server holds the request open until data exists), **Server-Sent Events (SSE)** (server→client only, over plain HTTP, auto-reconnecting — the right choice for one-way live feeds).

---

## 12. Security essentials 🟠

| Concept | One-line explanation |
|---|---|
| **CIA triad** | Confidentiality, Integrity, Availability — the goals of security |
| **Authentication vs Authorization** | Who you are vs what you're allowed to do |
| **Symmetric vs asymmetric** | Shared secret vs public/private key pair |
| **Hashing** | One-way; used for passwords (with **salt** + a slow hash like bcrypt/Argon2), never for encryption |
| **Digital signature** | Sign with private key, verify with public key → authenticity + integrity + non-repudiation |
| **MITM** | Attacker sits between two parties; defeated by certificate validation |
| **DoS / DDoS** | Overwhelm a service with traffic; mitigated by rate limiting, CDNs, scrubbing |
| **SQL injection** | Untrusted input concatenated into SQL; fix with **parameterized queries** |
| **XSS** | Attacker's script runs in a victim's browser; fix with output escaping, CSP, `HttpOnly` cookies |
| **CSRF** | Victim's browser is tricked into sending an authenticated request; fix with anti-CSRF tokens and `SameSite` cookies |
| **Firewall** | Filters traffic by rules — stateless (packet filter) vs stateful (tracks connections) |
| **VPN** | Encrypted tunnel over a public network |
| **Proxy vs reverse proxy** | Forward proxy acts on behalf of *clients*; reverse proxy sits in front of *servers* (TLS termination, load balancing, caching) |

---

## 13. "What happens when you type google.com" 🔴

This is *the* CN interview question. Answer it as a layered narrative, and be ready to go deeper on any step.

```
1.  URL parsing        Browser separates scheme, host, path; handles autocomplete/search vs URL
2.  HSTS check         If the domain is on the HSTS list, force https:// immediately
3.  Cache checks       Browser cache → OS cache → hosts file → router cache → ISP resolver cache
4.  DNS resolution     Recursive resolver → root → TLD (.com) → authoritative NS → A/AAAA record
5.  ARP / routing      Find the default gateway's MAC via ARP; packets routed hop-by-hop toward the IP
6.  TCP handshake      SYN → SYN+ACK → ACK  (1 RTT)
7.  TLS handshake      ClientHello/ServerHello, certificate validation, key exchange → session keys
8.  HTTP request       GET / HTTP/2, Host, cookies, Accept-Encoding …
9.  Server processing   Load balancer → reverse proxy → app server → cache/DB → response built
10. HTTP response      Status line, headers, body (often gzip/brotli compressed)
11. Parsing            HTML → DOM tree; CSS → CSSOM; JS may block parsing
12. Subresource fetch  CSS, JS, images, fonts — often from a CDN, in parallel
13. Render             DOM + CSSOM → render tree → layout (reflow) → paint → composite
14. Continued activity XHR/fetch calls, WebSockets, lazy loading; connection kept alive
```

**Depth to add if pushed:** mention that the TCP + TLS handshakes cost ~2–3 RTTs (which HTTP/3 reduces), that DNS TTLs govern caching, that the browser opens multiple connections per origin in HTTP/1.1 but multiplexes in HTTP/2, and that anycast routing sends you to the nearest Google front-end.

---

## 14. Rapid revision sheet

| # | Question | One-line answer |
|---|---|---|
| 1 | OSI layers | Physical, Data Link, Network, Transport, Session, Presentation, Application |
| 2 | TCP vs UDP | Reliable/ordered/connection-oriented vs fast/connectionless/best-effort |
| 3 | Why 3-way handshake | Synchronize sequence numbers and confirm both directions work |
| 4 | Why 4-way close | Full-duplex — each direction closes independently |
| 5 | Flow vs congestion control | Protect the receiver vs protect the network |
| 6 | MAC vs IP | Layer-2 physical identifier (hop-local) vs layer-3 logical address (end-to-end) |
| 7 | Switch vs router | Forwards frames by MAC within a LAN vs packets by IP between networks |
| 8 | ARP | Resolves IP → MAC on a local network via broadcast |
| 9 | DHCP | Auto-assigns IP config via DORA (Discover, Offer, Request, Ack) |
| 10 | DNS | Hierarchical name → IP resolution, UDP/53, cached with TTLs |
| 11 | NAT | Maps private addresses to a public one, usually with port translation (PAT) |
| 12 | HTTP statelessness | Server keeps no memory between requests; cookies/sessions/tokens add state |
| 13 | 401 vs 403 | Not authenticated vs authenticated but not permitted |
| 14 | HTTPS | HTTP over TLS: confidentiality, integrity, server authentication |
| 15 | TLS key model | Asymmetric to authenticate + agree a key, symmetric for bulk data |
| 16 | REST | Resource-oriented, stateless, uniform interface architectural style |
| 17 | WebSocket vs HTTP | Persistent full-duplex channel vs request-response |
| 18 | TTL | Hop counter that prevents routing loops; basis of traceroute |
| 19 | /26 subnet | 64 addresses, 62 usable hosts, mask 255.255.255.192 |
| 20 | Public vs private IP | Routable on the Internet vs 10/8, 172.16/12, 192.168/16 internal only |

---

## 15. Interview question bank with model answers

**Q: Why is TCP called reliable? Name the mechanisms.**
Sequence numbers for ordering and duplicate detection, acknowledgements with retransmission on timeout or 3 duplicate ACKs, checksums for corruption detection, sliding-window flow control, and congestion control. Reliability is *achieved*, not magic — every one of those has a cost in latency or overhead.

**Q: You can ping a server but the website won't load. Debug it.**
Ping (ICMP, layer 3) working means routing and the host are fine, so the problem is above layer 3: the web service may be down or not listening on 80/443, a firewall may block those ports, TLS certificate may be invalid/expired, DNS may resolve to the wrong IP, or the app itself is erroring (5xx). Check with `telnet host 443` / `curl -v`, inspect the status code, and check DNS with `nslookup`/`dig`.

**Q: Your API is slow. Where would you look?**
Split the time budget: DNS resolution → TCP+TLS handshake → network RTT → server processing (app + DB queries) → response size/serialization → client rendering. Measure each. Common fixes: connection reuse/keep-alive, CDN for static assets, database indexing and N+1 query elimination, caching, compression, pagination, and moving to HTTP/2.

**Q: Why does DNS use UDP?**
Queries and responses are small and fit in one datagram, so a TCP handshake would triple the latency for no benefit; if a response is lost, the client simply retries. TCP is used when responses exceed 512 bytes (or with EDNS0, when truncation occurs) and for zone transfers.

**Q: What is head-of-line blocking and how do HTTP/2 and HTTP/3 address it?**
In HTTP/1.1, responses on a connection must return in order, so one slow response blocks the rest. HTTP/2 multiplexes independent streams over one connection, solving it at the HTTP layer — but a single lost TCP segment still stalls every stream, because TCP delivers bytes in order. HTTP/3 runs over QUIC, whose streams are independently ordered, so loss on one stream doesn't block others.

**Q: How would you make a stateless service handle logins?**
Either server-side sessions with a shared store (Redis) — easy to revoke, but adds a dependency — or signed tokens (JWT) that carry claims — no lookup needed, but revocation before expiry is hard, so use short-lived access tokens plus refresh tokens. Mention storing tokens in `HttpOnly`, `Secure`, `SameSite` cookies rather than localStorage.

**Q: Two devices on the same LAN — does the packet reach a router?**
No. The sender compares the destination IP against its own network using the subnet mask; if it's the same subnet, it ARPs for the destination's MAC and the switch delivers the frame directly. Only off-subnet traffic goes to the default gateway.

**Q: What's the difference between a proxy and a reverse proxy?**
A forward proxy sits in front of clients and hides them from servers (corporate filtering, anonymity). A reverse proxy sits in front of servers and hides them from clients — handling TLS termination, load balancing, caching, and rate limiting. Nginx is commonly used as the latter.

---

## 16. Traps that cost marks

1. **TCP is not "faster" or "slower" in the abstract** — it has higher overhead and adds latency for reliability. UDP isn't faster at moving bytes; it just does less.
2. **UDP does have a checksum** (optional in IPv4, mandatory in IPv6). It just discards corrupted datagrams instead of retransmitting.
3. **HTTPS is not "HTTP with a different port."** It is HTTP over TLS; the port is a consequence.
4. **DNS does not "propagate."** Old cached records expire based on TTL.
5. **A page load is not one request.** It's one HTML request plus dozens of subresource requests.
6. Usable hosts = 2^(host bits) **− 2**, not 2^(host bits). (Exception: /31 point-to-point links per RFC 3021.)
7. **Switches don't break broadcast domains; routers do.** Switches break collision domains.
8. **ARP resolves the gateway's MAC** for off-network destinations, not the final destination's.
9. **Flow control ≠ congestion control.** Receiver vs network.
10. **The IP header does not change source/destination across hops; the MAC header does.**
11. **The presentation and session layers don't exist as separate layers in TCP/IP** — they're folded into Application.
12. **REST is not defined by JSON**, and "RESTful" requires statelessness.
13. **A 200 response can still represent an application failure** if the API is badly designed — status codes must be meaningful.
14. **TLS provides authentication of the server**, not of the user; user authentication is a separate application concern.
15. **`localhost`/127.0.0.1 never leaves the machine** — no NIC, no network.

---

## 17. Practice set with solutions

**Q1.** How many usable hosts in `192.168.1.0/27`? List the first three subnets.

<details><summary>Solution</summary>

Host bits = 32 − 27 = 5 → 32 addresses → **30 usable**. Mask 255.255.255.224, block size 32.
Subnets: 192.168.1.0 (usable .1–.30, bcast .31), 192.168.1.32 (.33–.62, bcast .63), 192.168.1.64 (.65–.94, bcast .95).
</details>

**Q2.** For host `10.5.130.77/19`, give the network address, broadcast, and usable range.

<details><summary>Solution</summary>

/19 → mask 255.255.224.0 → block size in 3rd octet = 256 − 224 = **32**. Largest multiple of 32 ≤ 130 is **128**.
Network = **10.5.128.0**, next network = 10.5.160.0 → Broadcast = **10.5.159.255**, usable **10.5.128.1 – 10.5.159.254** (2¹³ − 2 = 8190 hosts).
</details>

**Q3.** A company needs subnets for 100, 50, 20, and 2 hosts from `192.168.1.0/24`. Design with VLSM.

<details><summary>Solution</summary>

Largest first:
- 100 hosts → need 128 addresses → **/25** → 192.168.1.0/25 (usable .1–.126)
- 50 hosts → 64 addresses → **/26** → 192.168.1.128/26 (.129–.190)
- 20 hosts → 32 addresses → **/27** → 192.168.1.192/27 (.193–.222)
- 2 hosts → 4 addresses → **/30** → 192.168.1.224/30 (.225–.226)
</details>

**Q4.** Link bandwidth 1 Mbps, packet size 1000 bytes, propagation delay 20 ms. Total one-way delay ignoring queuing/processing?

<details><summary>Solution</summary>

Transmission delay = (1000 × 8 bits) / (10⁶ bps) = 8000/10⁶ = **8 ms**.
Total = 8 + 20 = **28 ms**. (RTT for a small ACK ≈ 28 + 20 ≈ 48 ms.)
</details>

**Q5.** TCP connection: bandwidth 100 Mbps, RTT 40 ms. What window size is needed for full utilization?

<details><summary>Solution</summary>

BDP = 100 × 10⁶ bits/s × 0.04 s = 4 × 10⁶ bits = **500,000 bytes ≈ 500 KB**.
Since the base TCP window field is 16 bits (max 65,535 bytes), you need the **window scaling** option.
</details>

**Q6.** Why does `DELETE /users/5` returning 404 on the second call still count as idempotent?

<details><summary>Solution</summary>

Idempotency concerns the **server's resulting state**, not the response code. After one or ten DELETEs, user 5 is absent — the state is identical. The differing status code is fine.
</details>

**Q7.** Which layer does each belong to: ARP, ICMP, TLS, BGP, Ethernet, DNS?

<details><summary>Solution</summary>

ARP → Data Link (layer 2, resolving a layer-3 address) · ICMP → Network (3) · TLS → Presentation/Session in OSI terms, sits between Transport and Application in practice · BGP → Application layer protocol carrying layer-3 routing info (runs over TCP 179) · Ethernet → Data Link + Physical · DNS → Application (7).
</details>

**Q8.** A user reports "the site works on mobile data but not on office Wi-Fi." First three hypotheses?

<details><summary>Solution</summary>

(1) Corporate firewall/proxy blocking the destination or port; (2) DNS — the office resolver returns a stale or blocked answer (test with `dig @8.8.8.8`); (3) captive portal / TLS interception with an untrusted certificate. Also consider an office IP being rate-limited or blocklisted by the server.
</details>

---

### Study order recommendation

1. OSI/TCP-IP + devices → 2. IP addressing & subnetting (do 20 subnetting problems — it's pure marks) → 3. TCP vs UDP + handshakes → 4. Flow/congestion control → 5. HTTP methods/status/headers → 6. HTTPS/TLS → 7. DNS/DHCP/ARP/NAT → 8. REST & WebSockets → 9. Rehearse §13 out loud three times.
