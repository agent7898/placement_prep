# Operating Systems — Placement Study Guide

> **How to use this file:** Read a section → close the file → explain the concept out loud in 60 seconds → then attempt the numericals and interview questions at the end of the section. OS is tested in three shapes: **MCQs** (definitions + comparisons), **numericals** (scheduling, page replacement, disk scheduling, address translation), and **interview scenarios** ("two threads update a balance — what breaks?"). Prepare all three shapes, not just the definitions.

**Priority legend:** 🔴 MUST KNOW · 🟠 HIGH · 🟡 IMPORTANT · 🟢 GOOD TO KNOW

---

## Table of contents

1. [What an OS actually is](#1-what-an-os-actually-is-)
2. [Kernel, system calls, user vs kernel mode](#2-kernel-system-calls-and-the-two-modes-)
3. [Processes](#3-processes-)
4. [Threads and concurrency](#4-threads-and-concurrency-)
5. [CPU scheduling](#5-cpu-scheduling-)
6. [Synchronization](#6-synchronization-)
7. [Deadlocks](#7-deadlocks-)
8. [Memory management](#8-memory-management-)
9. [Virtual memory and page replacement](#9-virtual-memory-and-page-replacement-)
10. [File systems](#10-file-systems-)
11. [Disk scheduling](#11-disk-scheduling-)
12. [IPC](#12-inter-process-communication-ipc-)
13. [Rapid revision sheet](#13-rapid-revision-sheet)
14. [Interview question bank with model answers](#14-interview-question-bank-with-model-answers)
15. [Traps that cost marks](#15-traps-that-cost-marks)
16. [Practice set with solutions](#16-practice-set-with-solutions)

---

## 1. What an OS actually is 🔴

An operating system is a **resource manager and an abstraction layer**. It sits between hardware and applications and does four jobs:

| Job | What it means | Concrete example |
|---|---|---|
| Abstraction | Hides ugly hardware behind clean interfaces | You call `read()`, not "move disk head to cylinder 4021" |
| Resource management | Decides who gets CPU, memory, disk, I/O and when | Scheduler picks the next process |
| Isolation & protection | One program cannot corrupt another | Virtual address spaces, kernel mode |
| Convenience services | Common facilities every program needs | File system, networking stack, IPC |

**Interview framing (use this sentence):** "An OS is a program that manages hardware resources and provides an abstraction layer so multiple programs can share a machine safely and efficiently."

### Types of OS (MCQ fodder)

| Type | Key idea | Example use |
|---|---|---|
| Batch | Jobs collected and run without interaction | Old mainframe payroll |
| Multiprogramming | Keep several jobs in memory; when one blocks on I/O, run another | Maximizes CPU utilization |
| Multitasking / Time-sharing | Rapid switching gives illusion of parallelism | Desktop OS |
| Multiprocessing | Multiple CPUs/cores genuinely running in parallel | Modern servers |
| Real-time (RTOS) | Correctness depends on **meeting deadlines** | Airbag controller, industrial control |
| Distributed | Multiple machines appear as one system | Cluster OS |
| Embedded | Small footprint, fixed function | Router firmware |

> **Hard vs soft real-time:** hard = missing a deadline is a system failure; soft = missing a deadline degrades quality. Frequently asked as a one-line MCQ.

### Multiprogramming vs multitasking vs multithreading vs multiprocessing 🔴

This four-way distinction is asked constantly. Learn the *distinguishing variable*:

- **Multiprogramming** — several programs resident in memory; switching happens when a process **blocks**. Goal: CPU utilization.
- **Multitasking** — switching happens on a **timer** (time slice). Goal: responsiveness.
- **Multithreading** — multiple threads *inside one process* sharing an address space. Goal: concurrency within an application.
- **Multiprocessing** — more than one **physical CPU/core**. Goal: real parallelism.

---

## 2. Kernel, system calls, and the two modes 🔴

### Kernel

The kernel is the always-resident core of the OS with full hardware privileges. It handles process management, memory management, device drivers, file systems, and system calls.

**Kernel architectures:**

| Architecture | Structure | Trade-off |
|---|---|---|
| Monolithic | All OS services run in kernel space | Fast (no message passing) but a driver bug can crash the system. Linux is monolithic (modular). |
| Microkernel | Only bare minimum (IPC, scheduling, basic memory) in kernel; drivers/FS in user space | More robust and easier to extend, but more context switches → more overhead. QNX, MINIX. |
| Hybrid | Mixture | Windows NT, macOS (XNU) |
| Exokernel | Exposes hardware almost directly, libraries provide abstractions | Research/specialized |

### User mode vs kernel mode 🔴

The CPU has a **mode bit**.

| | User mode | Kernel mode |
|---|---|---|
| Mode bit | 1 | 0 |
| Privileged instructions | Not allowed | Allowed |
| Direct hardware access | No | Yes |
| Memory reachable | Only the process's own address space | All memory |
| Crash impact | Process dies | System may crash |

**Why two modes exist:** protection. If any program could execute `halt`, modify page tables, or touch device registers, isolation would be impossible.

### System calls 🔴

A **system call** is the controlled doorway from user mode into kernel mode.

**Mechanism (say this in interviews):**
1. Program puts the system call number and arguments in registers.
2. Executes a trap/`syscall` instruction — a **software interrupt**.
3. CPU switches to kernel mode and jumps to a fixed handler address in the kernel.
4. Kernel validates arguments, performs the service.
5. Returns result, switches back to user mode.

**Categories with examples (Linux/POSIX):**

| Category | Examples |
|---|---|
| Process control | `fork()`, `exec()`, `wait()`, `exit()` |
| File management | `open()`, `read()`, `write()`, `close()`, `lseek()` |
| Device management | `ioctl()`, `read()`, `write()` |
| Information maintenance | `getpid()`, `time()`, `sysinfo()` |
| Communication | `pipe()`, `shmget()`, `socket()`, `send()`, `recv()` |
| Protection | `chmod()`, `umask()`, `chown()` |

> **Trap vs interrupt vs exception** (classic MCQ):
> - **Interrupt** — asynchronous, from hardware (keyboard, timer, disk).
> - **Trap** — synchronous, *deliberately* caused by software (a system call).
> - **Exception/fault** — synchronous, caused by an error condition (divide-by-zero, page fault).

### `fork()` 🔴 — a guaranteed interview topic

`fork()` creates a child process that is a copy of the parent.

- Returns **0 in the child**, the **child's PID in the parent**, **-1 on failure**.
- Parent and child have separate address spaces (implemented with **copy-on-write**: pages are shared read-only until one writes, then it's copied).
- `exec()` **replaces** the current process image with a new program — it does not create a process. The shell does `fork()` then `exec()`.

**Classic OA question:** How many processes does this create?

```c
fork();
fork();
fork();
```
**Answer:** 2³ = 8 total processes, so **7 new children**. In general, *n* consecutive `fork()` calls yield 2ⁿ processes.

**Zombie vs orphan** 🔴
- **Zombie** — child has terminated but the parent hasn't called `wait()`, so the exit status entry remains in the process table. Fix: parent must `wait()`.
- **Orphan** — parent terminated first; the child is re-parented to `init`/`systemd` (PID 1), which reaps it.

---

## 3. Processes 🔴

**Process = a program in execution.** A program is a passive file on disk; a process is active, with a program counter, registers, stack, and its own address space.

### Memory layout of a process 🔴

```
High address
┌──────────────────────┐
│        Stack         │  local variables, function frames, return addresses
│          ↓           │  grows downward
├──────────────────────┤
│      (free gap)      │
├──────────────────────┤
│          ↑           │  grows upward
│         Heap         │  dynamic allocation (new / malloc)
├──────────────────────┤
│    Data segment      │  initialized globals/statics (+ BSS for uninitialized)
├──────────────────────┤
│    Text / Code       │  compiled instructions (read-only)
└──────────────────────┘
Low address
```

**Threads of the same process share** Text, Data, Heap, and open files. **Each thread has its own** stack, registers, and program counter. This one line answers half of all thread questions.

### Process states 🔴

```
            admitted            scheduler dispatch
   New ──────────────► Ready ────────────────────► Running
                        ▲                            │
                        │                            ├──► exit ──► Terminated
        I/O or event    │                            │
        completion      └──────── Waiting ◄──────────┘
                                            I/O or event wait
                              (interrupt / time-slice expiry: Running → Ready)
```

- **New** — being created.
- **Ready** — has everything except the CPU; sitting in the ready queue.
- **Running** — executing on a CPU.
- **Waiting/Blocked** — waiting for I/O or an event; **cannot** be selected by the scheduler.
- **Terminated** — finished; PCB awaits cleanup.
- (Sometimes shown) **Suspended ready / Suspended blocked** — swapped out to disk by the medium-term scheduler.

> **Trap:** There is **no** transition from Waiting directly to Running. A process that finishes I/O goes to **Ready** first. This is a very common MCQ.

### Process Control Block (PCB) 🔴

The kernel's per-process record. Contents:

- Process ID (PID), parent PID
- Process state
- Program counter, CPU registers
- CPU scheduling info (priority, queue pointers)
- Memory management info (page table / base & limit registers)
- Accounting info (CPU used, time limits)
- I/O status info (open files, allocated devices)

### Schedulers 🟠

| Scheduler | Also called | What it does | Frequency |
|---|---|---|---|
| Long-term | Job scheduler | Admits jobs from disk into the ready queue; controls **degree of multiprogramming** | Infrequent |
| Short-term | CPU scheduler | Picks which ready process runs next | Very frequent (ms) |
| Medium-term | Swapper | Swaps processes out of / into memory to relieve pressure | Occasional |

### Context switching 🔴

Saving the state of the running process into its PCB and loading another process's state.

**Steps:** save registers + PC → update PCB and process state → move PCB into the appropriate queue → select next process → load its registers/PC → switch memory context (page table base register) → resume.

**Costs:**
- Direct: register save/restore, scheduler runtime.
- Indirect and usually **larger**: cache pollution and **TLB flush**, so the new process starts with cold caches.

**It is pure overhead — no useful work is done during a context switch.** Say this.

> **Thread switch is cheaper than process switch** because the address space does not change → page table base register unchanged → TLB need not be flushed.

---

## 4. Threads and concurrency 🔴

### Process vs thread

| Process | Thread |
|---|---|
| Independent execution unit | Execution unit inside a process |
| Separate address space | Shares the process address space |
| Heavier to create | Lighter to create |
| Context switch more expensive | Cheaper |
| Communicate via IPC | Communicate via shared memory directly |
| One crashing doesn't kill others | One crashing can take down the whole process |

### Benefits of threads
Responsiveness · resource sharing · economy (cheaper than processes) · scalability on multicore.

### Multithreading models 🟡

| Model | Mapping | Note |
|---|---|---|
| Many-to-One | Many user threads → 1 kernel thread | One blocking call blocks all; no true parallelism |
| One-to-One | Each user thread → its own kernel thread | True parallelism; thread creation cost is real. Linux/Windows |
| Many-to-Many | *m* user threads → *n* kernel threads (*n* ≤ *m*) | Flexible, complex to implement |

### User-level vs kernel-level threads

- **User-level:** managed by a library; kernel is unaware. Fast switching, but a blocking system call blocks the entire process and threads can't run on separate cores.
- **Kernel-level:** kernel schedules them. Slower to create/switch, but genuinely parallel and individually blockable.

### Concurrency vs parallelism 🟠
- **Concurrency** — multiple tasks *in progress* over an interval (can be a single core interleaving).
- **Parallelism** — multiple tasks *executing at the same instant* (needs multiple cores).

> "Concurrency is about dealing with many things at once; parallelism is about doing many things at once."

---

## 5. CPU scheduling 🔴

The most numerically tested OS topic. Expect to draw a Gantt chart and compute averages.

### Definitions you must state precisely

| Term | Definition |
|---|---|
| Arrival Time (AT) | When the process enters the ready queue |
| Burst Time (BT) | CPU time the process needs |
| Completion Time (CT) | When the process finishes |
| **Turnaround Time (TAT)** | **CT − AT** (total time in the system) |
| **Waiting Time (WT)** | **TAT − BT** (time spent in the ready queue) |
| **Response Time (RT)** | First time it gets the CPU − AT |
| Throughput | Processes completed per unit time |
| CPU utilization | Fraction of time the CPU is busy |

**Preemptive** = the OS can take the CPU away from a running process. **Non-preemptive** = the process keeps the CPU until it finishes or blocks.

### The algorithms

| Algorithm | Preemptive? | Selection rule | Strength | Weakness |
|---|---|---|---|---|
| **FCFS** | No | Earliest arrival | Simple, fair-ish, no starvation | **Convoy effect** — a long job delays everyone; bad average WT |
| **SJF** | No | Shortest burst | **Provably minimal average waiting time** | Burst time is unknown in practice; starvation of long jobs |
| **SRTF** (Shortest Remaining Time First) | Yes | Least remaining time | Optimal average WT among preemptive | Starvation; heavy context switching |
| **Priority** | Both variants | Highest priority | Supports importance | **Starvation** → fix with **aging** (raise priority with waiting time) |
| **Round Robin** | Yes | FCFS with time quantum *q* | Great **response time**, no starvation, fair | Higher average TAT; performance highly sensitive to *q* |
| **Multilevel Queue** | Usually | Fixed queues by process class, each with its own policy | Separates system/interactive/batch | Rigid; low queues can starve |
| **MLFQ** | Yes | Processes move between queues based on observed behavior | Approximates SJF **without knowing burst times**; favours interactive jobs | Complex tuning |

**Round Robin quantum trade-off (say this exactly):**
- *q* → ∞ ⇒ RR degenerates into **FCFS**.
- *q* → very small ⇒ excellent response time but **context-switch overhead dominates** (processor sharing).
- Rule of thumb: *q* should be larger than the majority of CPU bursts (typically ~80% complete within one quantum).

**MLFQ rules (classic 3 rules):** higher priority runs first; new jobs enter the highest queue; a job using its full quantum is demoted to a lower queue (longer quantum). Periodic **priority boost** prevents starvation.

### Worked example — FCFS

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 8 |

Gantt: `| P1 0–5 | P2 5–8 | P3 8–16 |`

| P | CT | TAT = CT−AT | WT = TAT−BT |
|---|---|---|---|
| P1 | 5 | 5 | 0 |
| P2 | 8 | 7 | 4 |
| P3 | 16 | 14 | 6 |

Average TAT = (5+7+14)/3 = **8.67**, Average WT = (0+4+6)/3 = **3.33**

### Worked example — SRTF (preemptive SJF)

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 7 |
| P2 | 2 | 4 |
| P3 | 4 | 1 |
| P4 | 5 | 4 |

Trace: P1 runs 0–2 (5 left). At t=2, P2 needs 4 < 5 → preempt. P2 runs 2–4 (2 left). At t=4, P3 needs 1 < 2 → preempt. P3 runs 4–5, done. At t=5 remaining: P1=5, P2=2, P4=4 → P2 runs 5–7, done. Then P4 (4) vs P1 (5) → P4 runs 7–11. Then P1 runs 11–16.

Gantt: `P1(0-2) P2(2-4) P3(4-5) P2(5-7) P4(7-11) P1(11-16)`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 16 | 16 | 9 |
| P2 | 7 | 5 | 1 |
| P3 | 5 | 1 | 0 |
| P4 | 11 | 6 | 2 |

Average WT = (9+1+0+2)/4 = **3.0**, Average TAT = (16+5+1+6)/4 = **7.0**

### Worked example — Round Robin, q = 2

| Process | AT | BT |
|---|---|---|
| P1 | 0 | 5 |
| P2 | 1 | 4 |
| P3 | 2 | 2 |

Ready queue evolution → Gantt: `P1(0-2) P2(2-4) P3(4-6) P1(6-8) P2(8-10) P1(10-11)`

| P | CT | TAT | WT |
|---|---|---|---|
| P1 | 11 | 11 | 6 |
| P2 | 10 | 9 | 5 |
| P3 | 6 | 4 | 2 |

Average WT = **4.33**, Average TAT = **8.0**

> **RR queue-ordering rule (where most people lose marks):** when a process's quantum expires, first add any processes that **arrived during that quantum** to the tail of the ready queue, *then* add the preempted process. Get this ordering wrong and every subsequent number is wrong.

### Convoy effect
Under FCFS, one CPU-bound process at the head makes many short I/O-bound processes wait, leaving I/O devices idle. Overall throughput collapses. RR and SJF-family algorithms mitigate it.

---

## 6. Synchronization 🔴

### The problem

**Race condition** — the result depends on the *timing/interleaving* of concurrent accesses to shared data.

Classic demo — two threads doing `balance = balance + 100`, which compiles to three steps:

```
LOAD  R1, balance     ; T1 reads 1000
ADD   R1, 100         ; T1 computes 1100
                      ;   ← context switch
                      ; T2 reads 1000, computes 1100, stores 1100
STORE balance, R1     ; T1 stores 1100 — T2's update is lost
```
Two deposits of 100 on a balance of 1000 produce 1100 instead of 1200. This is the **lost update** problem. It happens because the read-modify-write is **not atomic**.

### Critical section problem

A **critical section** is the code region that accesses shared resources. Any correct solution must satisfy:

1. **Mutual exclusion** — at most one process in the CS at a time.
2. **Progress** — if no one is in the CS, selection of the next entrant cannot be postponed indefinitely by processes not wanting to enter.
3. **Bounded waiting** — a bound exists on how many times others enter before a waiting process gets in (prevents starvation).

Structure:
```
do {
    entry section      // acquire
      critical section
    exit section       // release
      remainder section
} while (true);
```

### Software and hardware solutions 🟡

- **Peterson's solution** — two-process software solution using `flag[2]` and `turn`. Satisfies all three conditions. Assumes atomic loads/stores and no instruction reordering (why it is theoretical on modern CPUs without memory barriers).
- **Disabling interrupts** — works on uniprocessors only; unacceptable on multiprocessors and dangerous in user code.
- **Atomic hardware instructions** — `TestAndSet`, `CompareAndSwap (CAS)`, `Swap`. These make read-modify-write atomic and are the building blocks of real locks.

### Mutex vs Semaphore vs Monitor 🔴 (asked in almost every interview)

| | **Mutex** | **Semaphore** | **Monitor** |
|---|---|---|---|
| Nature | Locking mechanism | Signalling mechanism (integer counter) | Language-level construct (ADT) |
| Value | Locked / unlocked | Counting (0..N) or binary (0/1) | N/A |
| Ownership | **Owned** — only the locking thread may unlock | **No ownership** — any thread may signal | Implicit, enforced by compiler/runtime |
| Purpose | Mutual exclusion | Mutual exclusion *and* ordering/resource counting | Mutual exclusion + condition variables, safely |
| Typical use | Protect one shared variable | Limit N concurrent users of a resource pool; producer-consumer ordering | Java `synchronized` + `wait()`/`notify()` |

**Semaphore operations** (must be atomic):
```
wait(S) / P(S):    while (S <= 0) ; S--;      // conceptually
signal(S) / V(S):  S++;
```
Real implementations block the process on a queue instead of busy-waiting.

**Binary semaphore vs mutex** — behaviourally similar, but a binary semaphore has **no ownership**: thread A can lock and thread B can unlock. That makes it usable for signalling between threads, and makes it *wrong* as a general lock.

**Spinlock vs blocking lock:** a spinlock busy-waits. Good when the critical section is very short and you're on a multiprocessor (avoids context-switch cost). Terrible on a uniprocessor (the spinner holds the CPU the lock-holder needs).

### Classic synchronization problems 🟠

**1. Producer–Consumer (Bounded Buffer)**
```
semaphore empty = N;    // empty slots
semaphore full  = 0;    // filled slots
mutex    m;

Producer:                         Consumer:
  wait(empty);                      wait(full);
  wait(m);                          wait(m);
    add item to buffer                remove item
  signal(m);                        signal(m);
  signal(full);                     signal(empty);
```
> **Trap:** if you swap `wait(empty)` and `wait(m)` in the producer, you get **deadlock** — the producer holds the mutex while sleeping on a full buffer, so no consumer can ever drain it. Always acquire the counting semaphore **before** the mutex.

**2. Readers–Writers** — many readers may read simultaneously; a writer needs exclusive access. Naive solutions starve writers (reader-preference) or readers (writer-preference); fair solutions use an additional queue semaphore.

**3. Dining Philosophers** — 5 philosophers, 5 forks, each needs both neighbours' forks. Everyone grabbing the left fork simultaneously ⇒ **deadlock** (circular wait). Fixes:
- Allow at most 4 philosophers at the table.
- Asymmetric ordering: odd philosophers take left first, even take right first (**breaks circular wait** — the standard answer).
- Pick up both forks only if both are available (atomic acquisition).

**4. Sleeping Barber** — barber sleeps when no customers; customers wait or leave if chairs are full. Tests semaphores + counters.

### Starvation vs deadlock 🔴

| Deadlock | Starvation |
|---|---|
| Processes are blocked forever waiting for each other | A process is *runnable* but never chosen |
| No process in the set makes progress | Others make progress; one is perpetually skipped |
| Caused by circular waiting on resources | Caused by scheduling/priority policy |
| Fix: prevention/avoidance/detection | Fix: **aging**, fair scheduling |

Also know **priority inversion**: a high-priority task waits on a lock held by a low-priority task, which is itself preempted by medium-priority tasks. Fix: **priority inheritance** (the low-priority holder temporarily inherits the high priority). This famously affected the Mars Pathfinder — a good story to mention.

---

## 7. Deadlocks 🔴

**Deadlock** — a set of processes each holding a resource and waiting for a resource held by another in the set, so none can proceed.

### The four Coffman conditions (all four must hold simultaneously)

1. **Mutual exclusion** — at least one resource is non-shareable.
2. **Hold and wait** — a process holds at least one resource while waiting for others.
3. **No preemption** — resources cannot be forcibly taken away.
4. **Circular wait** — a cycle P₁→P₂→…→Pₙ→P₁ of waiting exists.

**Break any one ⇒ deadlock is impossible.**

### The four strategies 🔴

| Strategy | Idea | Cost |
|---|---|---|
| **Prevention** | Structurally negate one Coffman condition | Low resource utilization, possible starvation |
| **Avoidance** | Grant a request only if the system stays in a **safe state** (Banker's algorithm) | Requires knowing maximum needs in advance; expensive |
| **Detection & recovery** | Let deadlocks happen, detect via wait-for graph, then recover (kill a process / preempt a resource / rollback) | Detection overhead; recovery is disruptive |
| **Ignore (Ostrich algorithm)** | Assume it's rare; reboot if it happens | What UNIX/Windows largely do in practice |

**How prevention negates each condition:**
- *Mutual exclusion* — make resources shareable (spooling); often impossible.
- *Hold and wait* — require a process to request all resources up front, or release everything before requesting more. Poor utilization + starvation.
- *No preemption* — if a request can't be met, forcibly release the process's currently held resources. Only workable for state-savable resources (CPU, memory), not printers.
- *Circular wait* — **impose a total ordering on resource types** and require requests in increasing order. This is the practical, real-world technique (also the standard lock-ordering fix in multithreaded code).

### Safe state and Banker's algorithm 🟠

A state is **safe** if there exists a sequence ⟨P₁,…,Pₙ⟩ such that each Pᵢ's remaining need can be satisfied by currently available resources plus everything held by P₁…Pᵢ₋₁.

> **Safe ⇒ no deadlock. Unsafe ⇏ deadlock** — unsafe merely means deadlock is *possible*. This distinction is a favourite MCQ.

**Matrices:** `Allocation`, `Max`, `Need = Max − Allocation`, `Available`.

**Safety algorithm:**
1. `Work = Available`, `Finish[i] = false` for all i.
2. Find an *i* with `Finish[i] == false` and `Need[i] ≤ Work`.
3. `Work = Work + Allocation[i]`; `Finish[i] = true`; repeat step 2.
4. If all `Finish[i] == true`, the state is safe (and the order found is the safe sequence).

**Worked example** — 5 processes, resources A(10) B(5) C(7):

| P | Allocation (A B C) | Max (A B C) | Need (A B C) |
|---|---|---|---|
| P0 | 0 1 0 | 7 5 3 | 7 4 3 |
| P1 | 2 0 0 | 3 2 2 | 1 2 2 |
| P2 | 3 0 2 | 9 0 2 | 6 0 0 |
| P3 | 2 1 1 | 2 2 2 | 0 1 1 |
| P4 | 0 0 2 | 4 3 3 | 4 3 1 |

Total allocated = (7 2 5) ⇒ **Available = (3 3 2)**.

- Need(P1)=(1 2 2) ≤ (3 3 2) ✔ → Work = (3 3 2)+(2 0 0) = (5 3 2)
- Need(P3)=(0 1 1) ≤ (5 3 2) ✔ → Work = (7 4 3)
- Need(P4)=(4 3 1) ≤ (7 4 3) ✔ → Work = (7 4 5)
- Need(P0)=(7 4 3) ≤ (7 4 5) ✔ → Work = (7 5 5)
- Need(P2)=(6 0 0) ≤ (7 5 5) ✔ → Work = (10 5 7)

**Safe sequence: ⟨P1, P3, P4, P0, P2⟩** — the state is safe.

*Resource-request extension:* if P1 requests (1,0,2), check `Request ≤ Need` and `Request ≤ Available`, pretend to allocate, then re-run the safety algorithm. If still safe, grant; else make P1 wait.

### Detection
- **Single instance per resource type:** build a **wait-for graph**; a **cycle ⇒ deadlock**.
- **Multiple instances:** cycle in the resource-allocation graph means deadlock is *possible*, not certain. Use a Banker's-style detection algorithm with the `Request` matrix.

---

## 8. Memory management 🔴

### Address binding and address types

- **Logical (virtual) address** — generated by the CPU / seen by the program.
- **Physical address** — the actual address on the memory unit.
- The **MMU** (Memory Management Unit) translates logical → physical at runtime. In the simplest scheme: `physical = logical + relocation register`.

Binding can happen at **compile time** (absolute code), **load time** (relocatable), or **execution time** (needs MMU — this is what real systems use, and it's what makes swapping and virtual memory possible).

### Contiguous allocation and fragmentation 🔴

**Allocation strategies for a request of size *n*:**

| Strategy | Rule | Note |
|---|---|---|
| First Fit | First hole ≥ n | Fastest |
| Best Fit | Smallest hole ≥ n | Leaves tiny unusable slivers; must search |
| Worst Fit | Largest hole | Generally worst performer |
| Next Fit | First fit, continuing from last position | — |

**Worked example:** holes 100K, 500K, 200K, 300K, 600K; requests 212K, 417K, 112K, 426K.
- **First fit:** 212→500K (288K left); 417→600K (183K left); 112→288K remainder (176K left); 426→**no hole fits** (must wait).
- **Best fit:** 212→300K; 417→500K; 112→200K; 426→600K. **All fit.**
- **Worst fit:** 212→600K (388K); 417→500K (83K); 112→388K (276K); 426→ doesn't fit.

**Fragmentation** 🔴

| Internal | External |
|---|---|
| Wasted space **inside** an allocated block | Free memory exists but is **split into non-contiguous pieces** |
| Caused by fixed-size allocation units | Caused by variable-size allocation over time |
| Occurs in **paging** (last page partially used) | Occurs in **segmentation** / contiguous allocation |
| Fix: smaller pages (increases page table size) | Fix: **compaction** (expensive), or paging |

> **One-liner to memorize:** *Paging suffers internal fragmentation; segmentation suffers external fragmentation.*

### Paging 🔴

Logical memory is divided into fixed-size **pages**; physical memory into equal-size **frames**. Page size = frame size, always a power of 2.

**Address split:** for page size 2ⁿ and logical address of *m* bits:
```
┌──────────────────────┬────────────────┐
│  page number (m−n)   │ offset (n bits)│
└──────────────────────┴────────────────┘
```
Translation: page number → **page table** → frame number; physical address = frame number ‖ offset. **The offset is never translated.**

**Standard numerical setup:**
- Logical address space = 2^m bytes ⇒ *m* address bits.
- Page size = 2^n bytes ⇒ offset = *n* bits.
- Number of pages = 2^(m−n).
- Page table size = (number of pages) × (page table entry size).

**Worked example:** 32-bit logical address, 4 KB pages, PTE = 4 bytes.
- Offset = log₂(4096) = **12 bits** → page number = 32 − 12 = **20 bits**
- Number of pages = 2²⁰ = 1,048,576
- Page table size = 2²⁰ × 4 B = **4 MB per process** — which is exactly why multi-level page tables and inverted page tables exist.

**Page table entry contains:** frame number + valid/invalid bit, protection bits (r/w/x), dirty (modified) bit, reference bit, caching-disabled bit.

**Page table structures 🟡**
- **Single-level** — huge for big address spaces.
- **Hierarchical (multi-level)** — page the page table; only needed sub-tables exist. x86-64 uses 4 levels.
- **Hashed page table** — good for sparse, large address spaces.
- **Inverted page table** — one entry per **frame**, not per page. Small table, but searching by page number is slow (needs a hash); sharing memory becomes hard.

### TLB (Translation Lookaside Buffer) 🔴

Without a TLB, every memory reference needs **two** memory accesses: one for the page table, one for the data. The TLB is a small, fast, fully/set associative cache of recent page→frame translations.

**Effective Access Time (EAT)** — a guaranteed numerical:

$$\text{EAT} = h \times (t_{TLB} + t_{mem}) + (1-h) \times (t_{TLB} + 2 \times t_{mem})$$

where *h* = TLB hit ratio. (Variations exist — some questions ignore TLB lookup time, or assume the TLB is searched in parallel. **Read the question's stated assumptions.**)

**Worked example:** TLB access = 20 ns, memory access = 100 ns, hit ratio = 80%.
- Hit: 20 + 100 = 120 ns (80%)
- Miss: 20 + 100 + 100 = 220 ns (20%)
- EAT = 0.8(120) + 0.2(220) = 96 + 44 = **140 ns**

> On a context switch the TLB must be **flushed** (or entries tagged with an ASID/PCID), because virtual addresses now mean something different. This is a major hidden cost of context switching — good detail to drop in an interview.

### Segmentation 🟠

Memory is divided by **logical units** the programmer thinks in: code, stack, heap, globals — variable-sized. An address is `<segment number, offset>`; the **segment table** stores `<base, limit>` per segment, and the hardware checks `offset < limit` (else trap: segmentation fault).

| Paging | Segmentation |
|---|---|
| Fixed-size pages | Variable-size segments |
| Invisible to the programmer | Visible / logical to the programmer |
| Internal fragmentation | External fragmentation |
| One page table per process | One segment table per process |
| Address: page number + offset | Address: segment number + offset |
| Easier physical memory management | Better logical protection & sharing |

**Segmented paging** combines both: segments are divided into pages — you get segmentation's logical view without external fragmentation.

---

## 9. Virtual memory and page replacement 🔴

**Virtual memory** lets a process run with only part of it in physical memory, so the logical address space can exceed physical RAM. Implemented by **demand paging** — a page is brought in only when referenced.

### Page fault handling (step-by-step — say this in interviews)

1. CPU references a virtual address; MMU finds the PTE **valid bit = 0**.
2. Trap to the OS (page fault).
3. OS checks whether the reference is legal. If illegal → terminate the process (segfault).
4. If legal but not in memory: find a free frame; if none, run **page replacement** to evict a victim.
5. If the victim is **dirty**, write it back to disk first.
6. Schedule a disk read to load the required page into the frame.
7. Update the page table (valid bit = 1, frame number), update the TLB.
8. Restart the instruction that faulted.

> A page fault is **not an error** — it's a normal mechanism. "Segmentation fault" (illegal access) is the error. Interviewers like this distinction.

**Minor vs major page fault:** minor = page is in memory but not mapped in this page table (no disk I/O); major = requires disk I/O.

### Page replacement algorithms 🔴

Notation: reference string, number of frames.

| Algorithm | Victim | Property |
|---|---|---|
| **FIFO** | Oldest loaded page | Simple; suffers **Belady's anomaly** |
| **Optimal (OPT/MIN)** | Page used farthest in the future | Lowest possible fault count; **not implementable** — used as a benchmark |
| **LRU** | Least recently used | Good approximation of OPT; **no Belady's anomaly** (it's a stack algorithm); expensive (counters or a doubly-linked list + hash map) |
| **LFU / MFU** | Least/most frequently used | Poor with changing locality |
| **Clock (Second Chance)** | FIFO but skip pages with reference bit = 1 (clearing it) | Cheap, practical LRU approximation |

**Belady's anomaly** 🔴 — for some reference strings, **increasing the number of frames increases the number of page faults**. Occurs in **FIFO**; does **not** occur in stack algorithms (**LRU, OPT**).

**Canonical Belady example** — string `1 2 3 4 1 2 5 1 2 3 4 5` with FIFO: **9 faults with 3 frames, 10 faults with 4 frames.**

### Worked example — reference string `7 0 1 2 0 3 0 4 2 3 0 3 2`, **3 frames**

**FIFO:**

| Ref | Frames after | Fault? |
|---|---|---|
| 7 | 7 | ✔ |
| 0 | 7,0 | ✔ |
| 1 | 7,0,1 | ✔ |
| 2 | 2,0,1 (evict 7) | ✔ |
| 0 | 2,0,1 | — |
| 3 | 2,3,1 (evict 0) | ✔ |
| 0 | 2,3,0 (evict 1) | ✔ |
| 4 | 4,3,0 (evict 2) | ✔ |
| 2 | 4,2,0 (evict 3) | ✔ |
| 3 | 4,2,3 (evict 0) | ✔ |
| 0 | 0,2,3 (evict 4) | ✔ |
| 3 | 0,2,3 | — |
| 2 | 0,2,3 | — |

**FIFO faults = 10**

**LRU** (evict the least recently *used*, not the oldest loaded): faults on 7, 0, 1, 2, 3, 4, 2, 3, 0 → **LRU faults = 9**.

**Optimal** (evict the page whose next use is farthest away): 7,0,1,2 (4 faults) → 0 hit → 3 evicts 1, which is never used again (5) → 0 hit → 4 evicts 0, next used at position 11 while 2 and 3 are used sooner (6) → 2,3 hit → 0 evicts 4, never used again (7) → 3,2 hit. **OPT faults = 7.**

Ordering always holds: **OPT ≤ LRU ≈ Clock ≤ FIFO**.

### Thrashing 🔴

The system spends more time **paging than executing**. Symptom: CPU utilization collapses while disk activity saturates.

**The vicious cycle:** low CPU utilization → the OS thinks it should admit *more* processes → less memory per process → more faults → even lower CPU utilization.

**Causes:** degree of multiprogramming too high; a process's **working set** doesn't fit in its allocated frames.

**Solutions:**
- **Working set model** — track the set of pages referenced in the last Δ references; allocate enough frames for it; suspend processes if ΣWSS > total frames.
- **Page-Fault Frequency (PFF)** — measure fault rate per process; if too high, give it more frames; if too low, take frames away.
- Reduce the degree of multiprogramming (swap processes out); add RAM.

**Locality of reference** is *why* virtual memory works at all: **temporal locality** (recently used will be used again) and **spatial locality** (nearby addresses will be used).

### Frame allocation
- **Equal allocation** — frames split evenly.
- **Proportional allocation** — frames ∝ process size.
- **Local replacement** — a process replaces only its own frames (performance is predictable).
- **Global replacement** — can steal any process's frame (better throughput, unpredictable per-process behaviour).

### Demand paging vs pre-paging vs swapping
- **Demand paging** — load on fault (lazy).
- **Pre-paging** — load pages expected to be needed soon.
- **Swapping** — move an entire process between memory and backing store (medium-term scheduler).
- **Copy-on-write** — after `fork()`, share pages read-only; copy only on write. Makes `fork()` cheap.

---

## 10. File systems 🟠

### File attributes and operations
Attributes: name, identifier (inode number), type, location, size, protection, timestamps.
Operations: create, write, read, reposition (seek), delete, truncate, open, close.

### Access methods
- **Sequential** — read/write in order (tape-like).
- **Direct/Random** — jump to any block number.
- **Indexed** — an index block maps logical records to blocks.

### Allocation methods 🔴

| Method | How | Pros | Cons |
|---|---|---|---|
| **Contiguous** | Consecutive blocks | Excellent sequential *and* direct access | External fragmentation; file growth is hard |
| **Linked** | Each block points to the next | No external fragmentation; easy growth | **No efficient direct access**; pointer overhead; a broken pointer loses the tail. (FAT keeps the links in a table, which improves random access.) |
| **Indexed** | An index block holds all block pointers | Direct access, no external fragmentation | Index block overhead for small files; needs multi-level/linked index for huge files |

### Inodes (UNIX) 🟠
An **inode** stores metadata + block pointers, typically: 12 direct pointers, 1 single-indirect, 1 double-indirect, 1 triple-indirect. This gives small files fast access and still supports enormous files.

**Numerical:** block size 4 KB, pointer size 4 B ⇒ one block holds 1024 pointers.
- Direct: 12 × 4 KB = 48 KB
- Single indirect: 1024 × 4 KB = 4 MB
- Double indirect: 1024² × 4 KB = 4 GB
- Triple indirect: 1024³ × 4 KB = 4 TB
- **Max file size ≈ 4 TB + 4 GB + 4 MB + 48 KB**

> Note: the inode does **not** store the file name. The name lives in the **directory entry**, which maps name → inode number. That's exactly why **hard links** work: several names, one inode. **Soft/symbolic links** store a *path* and break if the target is deleted; hard links keep the file alive until the link count drops to 0.

### Free space management
Bit vector/bitmap · linked list of free blocks · grouping · counting.

### Journaling 🟡
A journaling file system writes intended changes to a **log** before applying them, so after a crash the FS can replay/roll back and recover consistency fast (ext4, NTFS). Without it, you'd need a full `fsck` scan.

### Directory structures
Single-level → two-level → tree → acyclic graph (sharing via links) → general graph (needs cycle handling for deletion/garbage collection).

---

## 11. Disk scheduling 🟠

**Disk access time = seek time + rotational latency + transfer time.** Seek time dominates, so scheduling aims to minimize **total head movement**.

| Algorithm | Rule | Note |
|---|---|---|
| **FCFS** | Serve in arrival order | Fair, poor performance |
| **SSTF** | Shortest seek time first | Good throughput, **starvation** of far requests |
| **SCAN (elevator)** | Move in one direction servicing all, then reverse **at the end of disk** | No starvation |
| **C-SCAN** | Service in one direction, then **jump back to start** without servicing | More **uniform** wait times |
| **LOOK** | Like SCAN but reverses at the **last request**, not the disk end | Less wasted movement |
| **C-LOOK** | C-SCAN version of LOOK | Practical default |

**Worked example** — queue `98, 183, 37, 122, 14, 124, 65, 67`, head at **53**, disk 0–199, moving toward larger cylinders.

- **FCFS:** 53→98→183→37→122→14→124→65→67 = **640** cylinders.
- **SSTF:** 53→65→67→37→14→98→122→124→183 = **236** cylinders.
- **SCAN:** 53→65→67→98→122→124→183→**199**→37→14 = (199−53) + (199−14) = 146 + 185 = **331**.
- **C-SCAN:** 53→…→183→199→**0**→14→37 = 146 + 199 + 37 = **382** (counting the return jump).
- **LOOK:** 53→…→183→37→14 = 130 + 169 = **299**.
- **C-LOOK:** 53→…→183→**14**→37 = 130 + 169 + 23 = **322** (with jump counted).

> Always state your assumption about the head's **direction** and whether the **return jump is counted** — examiners accept either convention if you declare it.

**RAID (one-liners) 🟡:** RAID 0 = striping (speed, no redundancy) · RAID 1 = mirroring (redundancy, 50% capacity) · RAID 5 = striping + distributed parity (survives 1 disk failure) · RAID 6 = double parity (survives 2) · RAID 10 = mirrored stripes (speed + redundancy, expensive).

---

## 12. Inter-Process Communication (IPC) 🔴

Two fundamental models:

| **Shared memory** | **Message passing** |
|---|---|
| Processes map a common memory region | Processes exchange messages via the kernel |
| **Fast** — after setup, no kernel involvement | Slower — every message is a system call |
| Application must handle synchronization itself | Kernel handles synchronization |
| Same machine only | Works across machines (sockets) |

### Mechanisms

| Mechanism | Description |
|---|---|
| **Pipe** | Unidirectional byte stream between **related** processes (parent/child). `ls \| grep x` |
| **Named pipe (FIFO)** | Like a pipe but has a filesystem name; works between **unrelated** processes; can be bidirectional (two FIFOs) |
| **Message queue** | Kernel-maintained queue of discrete messages, with priorities; persists beyond sender |
| **Shared memory** | `shmget/shmat`; fastest IPC; needs semaphores for correctness |
| **Semaphore** | Synchronization rather than data transfer |
| **Socket** | Endpoint for communication, local (UNIX domain) or over a network |
| **Signal** | Asynchronous notification (`SIGKILL`, `SIGTERM`, `SIGSEGV`) |
| **Memory-mapped file** | `mmap` a file into address space; also usable for sharing |

**Blocking vs non-blocking:** blocking send/receive waits; non-blocking returns immediately. Also know **direct vs indirect** communication (name the process vs use a mailbox), and **bounded vs unbounded** buffering.

---

## 13. Rapid revision sheet

Be able to answer each in under 30 seconds:

| # | Question | One-line answer |
|---|---|---|
| 1 | Process vs thread | Separate address space vs shared address space within a process |
| 2 | What's saved on a context switch | Registers, PC, stack pointer, memory-management info → into the PCB |
| 3 | System call | Controlled entry into kernel mode via a trap |
| 4 | User vs kernel mode | Restricted instruction set vs full hardware privilege |
| 5 | Race condition | Outcome depends on interleaving of concurrent accesses to shared data |
| 6 | Mutex vs semaphore | Locking with ownership vs signalling counter without ownership |
| 7 | Deadlock | Circular waiting where no process in the set can proceed |
| 8 | Coffman conditions | Mutual exclusion, hold-and-wait, no preemption, circular wait |
| 9 | Prevention vs avoidance | Structurally break a condition vs stay in a safe state dynamically |
| 10 | Virtual memory | Illusion of a larger, private address space via demand paging |
| 11 | Paging vs segmentation | Fixed pages/internal fragmentation vs logical segments/external fragmentation |
| 12 | Page fault | Referenced page not in memory → trap → load from disk → restart instruction |
| 13 | FIFO vs LRU | Oldest loaded vs least recently used; FIFO can show Belady's anomaly |
| 14 | Thrashing | More time paging than executing; fix with working set / reduce multiprogramming |
| 15 | Why Round Robin | Bounded response time and no starvation for interactive workloads |
| 16 | Starvation vs deadlock | Runnable but never scheduled vs blocked forever |
| 17 | IPC | Mechanisms for processes to exchange data: shared memory vs message passing |
| 18 | Internal vs external fragmentation | Waste inside a block vs free memory split into unusable pieces |
| 19 | Zombie vs orphan | Dead child unreaped vs live child whose parent died |
| 20 | TLB | Cache of page-table entries that avoids a second memory access per reference |

---

## 14. Interview question bank with model answers

**Q: What happens from the moment you double-click an application?**
Shell/OS locates the executable → creates a process (`fork` + `exec` on UNIX) → allocates a PCB and a virtual address space → loads the program headers, mapping text/data segments (lazily, via demand paging) → sets up stack and heap → links shared libraries → the process enters the Ready queue → the scheduler dispatches it → pages fault in on demand as execution proceeds.

**Q: Two threads increment a shared counter 1,000,000 times each. Final value?**
Anywhere from 1,000,000 to 2,000,000. `count++` is a non-atomic read-modify-write; concurrent interleavings lose updates. Fixes: a mutex, an atomic instruction (CAS), or `AtomicInteger` in Java. Mention that the fix has a cost — lock contention — and that atomics are cheaper for a single variable.

**Q: Why is a thread context switch cheaper than a process context switch?**
The address space is unchanged, so the page table base register isn't reloaded and the TLB isn't flushed; only registers, PC, and stack pointer are swapped. Caches also stay warm.

**Q: Can a system be deadlock-free without any deadlock-handling code?**
Yes — if the resource-allocation design structurally prevents one Coffman condition, e.g. a global lock-ordering discipline (kills circular wait) or single-lock designs.

**Q: A server slows to a crawl, CPU utilization near zero, disk at 100%. Diagnose.**
Thrashing. Working sets exceed physical memory, so processes fault constantly. Confirm with page-fault rate and swap activity. Fix: reduce the degree of multiprogramming, add RAM, fix memory leaks, or use working-set-based allocation. Note the trap: adding *more* processes makes it worse, though naive CPU-utilization-driven admission would do exactly that.

**Q: Why can't we just use SJF in a real OS?**
Burst times are unknown in advance. Real systems approximate it: exponential averaging of past bursts (τₙ₊₁ = α·tₙ + (1−α)·τₙ), or MLFQ, which demotes CPU-heavy jobs and thus naturally prioritizes short ones.

**Q: Why is the page size a power of two?**
So the logical address splits into page number and offset by simple bit slicing — no division needed in hardware.

**Q: Trade-off in choosing page size?**
Large pages → smaller page tables, better TLB coverage, fewer faults for sequential access, but **more internal fragmentation** and wasted I/O. Small pages → less internal fragmentation but bigger page tables and more TLB misses.

**Q: What is priority inversion and how do you fix it?**
A high-priority task blocks on a lock held by a low-priority task that is itself preempted by medium-priority tasks. Fix with **priority inheritance** (holder temporarily inherits the highest waiter's priority) or **priority ceiling**.

**Q: Explain how LRU is implemented in O(1).**
HashMap (key → node) + doubly linked list ordered by recency. Access → move node to head. Evict → remove tail. Both O(1). This is also the LeetCode "LRU Cache" problem — being able to code it is a strong signal.

---

## 15. Traps that cost marks

1. **Waiting → Running does not exist.** It must pass through Ready.
2. **WT = TAT − BT**, not CT − BT. And TAT = CT − AT, not CT.
3. In **Round Robin**, add newly arrived processes to the queue *before* re-adding the preempted one.
4. **Belady's anomaly occurs in FIFO**, never in LRU or Optimal.
5. **Unsafe state ≠ deadlock.** It means deadlock is possible.
6. **Banker's algorithm is avoidance**, not prevention and not detection.
7. **A page fault is not an error**; an illegal memory access (segfault) is.
8. **Paging → internal** fragmentation; **segmentation → external**. Don't swap these.
9. A **binary semaphore is not exactly a mutex** — no ownership.
10. **Threads share the heap and globals, not the stack.**
11. **`exec()` does not create a new process**; `fork()` does.
12. Deadlock requires **all four** Coffman conditions simultaneously.
13. **Mutual exclusion alone is not enough** — progress and bounded waiting are also required.
14. In the producer-consumer solution, **the mutex must be the inner lock**, acquired after the counting semaphores.
15. Spinlocks on a **uniprocessor** are counterproductive.

---

## 16. Practice set with solutions

**Q1.** Processes: P1(AT=0, BT=4), P2(AT=1, BT=3), P3(AT=2, BT=1). Compute average WT under SRTF.

<details><summary>Solution</summary>

t=0: only P1 → runs. t=1: P1 remaining 3, P2 needs 3 → tie, no preemption (keep running). t=2: P1 remaining 2, P3 needs 1 → preempt. P3 runs 2–3. Then P1 (2 left) vs P2 (3) → P1 runs 3–5. Then P2 runs 5–8.
Gantt: `P1(0-2) P3(2-3) P1(3-5) P2(5-8)`
CT: P1=5, P2=8, P3=3. TAT: 5, 7, 1. WT: 1, 4, 0. **Average WT = 5/3 ≈ 1.67**
</details>

**Q2.** Reference string `1 2 3 4 1 2 5 1 2 3 4 5`. Compute FIFO faults with 3 and with 4 frames.

<details><summary>Solution</summary>

3 frames → **9 faults**. 4 frames → **10 faults**. Increasing frames increased faults: **Belady's anomaly**.
</details>

**Q3.** 16-bit logical address, page size 1 KB. How many bits for page number, and how many pages?

<details><summary>Solution</summary>

Offset = log₂(1024) = 10 bits → page number = 16 − 10 = **6 bits** → **2⁶ = 64 pages**.
</details>

**Q4.** Memory access 80 ns, TLB access 10 ns, hit ratio 90%. EAT?

<details><summary>Solution</summary>

Hit: 10+80 = 90; Miss: 10+80+80 = 170. EAT = 0.9(90) + 0.1(170) = 81 + 17 = **98 ns**.
</details>

**Q5.** Head at 50, queue `82, 170, 43, 140, 24, 16, 190`, disk 0–199, moving up. Total head movement under **C-LOOK**?

<details><summary>Solution</summary>

Upward: 82, 140, 170, 190 → 50→190 = 140. Jump to lowest pending: 190→16 = 174. Then 16→24→43 = 27.
Total = 140 + 174 + 27 = **341** (counting the jump).
</details>

**Q6.** A system has 3 processes and 4 instances of one resource type. Each process needs a maximum of 2. Can deadlock occur?

<details><summary>Solution</summary>

No. Worst case each process holds 1 (using 3), leaving 1 free, which lets some process reach its maximum, finish, and release. Generally, deadlock is impossible when **Σ(max needs) < (number of resources) + (number of processes)**: 6 < 4 + 3 = 7 ✔.
</details>

**Q7.** `fork(); fork(); printf("Hi");` — how many times is "Hi" printed?

<details><summary>Solution</summary>

4 processes exist after two forks, each executes the `printf` once → **4 times**.
</details>

**Q8.** Why does a `while(1);` loop in a user program not freeze a modern OS?

<details><summary>Solution</summary>

Preemptive scheduling: a hardware **timer interrupt** fires each quantum, transferring control to the kernel, which reschedules. Under a purely non-preemptive/cooperative scheduler, it *would* freeze the system.
</details>

---

### Study order recommendation

1. Processes & threads → 2. Scheduling (do 15 numericals) → 3. Synchronization → 4. Deadlock (do Banker's twice by hand) → 5. Memory & paging → 6. Virtual memory & replacement → 7. File systems & disk → 8. IPC.

Then: 40 MCQs timed, 10 numericals timed, and one full verbal run through §13 and §14.
