# 01 — Version Control Systems (VCS) Fundamentals & Git Origin

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Overview & Concept of Version Control

When developing software, writing books, or managing complex projects, work evolves through continuous edits and updates. 

### The Problem: Manual Versioning Pain
Before dedicated tools, developers used manual versioning—saving copies under increasingly desperate names:
```text
quiz-app/
quiz-app-1/
quiz-app-2/
quiz-app-final/
quiz-app-final-final/
quiz-app-final-very-final/
```
As highlighted in the reference video:
- Every time a new feature was attempted, developers created a copy of the folder to preserve the last working state.
- If a bug appeared, going back required guessing which folder had the correct, working code.
- Frequent `Ctrl + S` saves the active file, but overwrites previous states.

### What is a Version Control System (VCS)?
A **Version Control System (VCS)** is software that automatically records changes to files over time, allowing you to:
1. Revert selected files or the entire project back to any previous state.
2. Review who made what modifications and why.
3. Compare changes across different points in time.
4. Collaborate with multiple team members without overwriting each other's work.

---

## 2. Evolution of Version Control Architectures

Version control systems are categorized into three historical models:

```text
1. Local VCS            2. Centralized VCS (CVCS)          3. Distributed VCS (DVCS)
   ┌───────────────┐        ┌─────────────────┐               ┌─────────────────┐
   │ Local Machine │        │ Central Server  │               │ Remote Repo     │
   │ ┌───────────┐ │        │ (Single Source) │               │ (GitHub/GitLab) │
   │ │Database / │ │        └────────┬────────┘               └────────┬────────┘
   │ │Versions   │ │                 │ (Network Bound)                 │ (Sync)
   │ └───────────┘ │        ┌────────┴────────┐               ┌────────┴────────┐
   └───────────────┘        │ Local Machine   │               │ Local Clone     │
                            │ (Working file)  │               │ (Full History)  │
                            └─────────────────┘               └─────────────────┘
```

### Detailed Architecture Comparison

| Feature | Local VCS (e.g., RCS) | Centralized VCS (e.g., SVN, CVS, Perforce) | Distributed VCS (e.g., Git, Mercurial) |
|---|---|---|---|
| **Repository Location** | Local machine only | Central server only | Every local machine has a full clone |
| **History Availability** | Single machine | Central server | Every developer holds complete commit history |
| **Network Dependency** | None | Network required for commits, diffs, logs, and branches | Only required when synchronizing (push/pull/fetch) |
| **Offline Productivity** | High (local only) | Low (cannot commit or branch offline) | Full offline capabilities (commit, branch, log, diff) |
| **Single Point of Failure (SPOF)** | Machine disk failure loses everything | Central server crash halts all work; server disk failure risks losing history | None. Any developer's clone can restore the repository |
| **Performance** | Local disk speed | Network-latency bound | Blazing fast (local I/O for 90%+ operations) |

---

## 3. The Origin & History of Git

A frequent placement interview question tests your understanding of *why* Git was created:

1. **1991 – 2002:** The Linux kernel was maintained by sending raw patch files and compressed archives via email to Linus Torvalds.
2. **2002:** The Linux team adopted **BitKeeper**, a proprietary DVCS that allowed free usage for Linux kernel development.
3. **2005:** Relations broke down when a developer reverse-engineered the BitKeeper protocol. BitKeeper revoked the free license.
4. **The Creation of Git:** Linus Torvalds stepped away from kernel development to build a new tool from scratch with these strict principles:
   - **Speed:** Capable of handling thousands of patches daily on millions of lines of code.
   - **Simplicity of Design:** Content-addressable storage using cryptographic hashes (SHA-1).
   - **Strong Support for Non-linear Development:** Thousands of parallel branches that merge smoothly.
   - **Fully Distributed:** No central authority required to create commits.
   - **Snapshot-based (Not Delta-based):** Git stores full file snapshots over time rather than lists of line deltas.

---

## 4. Key Placement Takeaways & Interview FAQ

> **Q1: Why is Git called "Distributed"?**  
> **Answer:** Because there is no single central repository required for Git to function. Every developer has an identical, complete copy of the repository, including all files, branches, and the complete commit history.

> **Q2: Does Git store diffs/deltas or snapshots?**  
> **Answer:** Git stores **snapshots**. While older VCS (like SVN) store base files and incremental changes (deltas), Git captures a snapshot of the entire project tree at each commit, using pointers to unchanged blobs to save space.

> **Q3: What makes Git faster than SVN?**  
> **Answer:** In SVN, viewing logs, switching branches, or creating commits requires contacting the remote server over the network. In Git, all these operations run entirely on your local machine using local disk I/O.
