# 04 — Staging, Committing & Cryptographic Integrity

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Staging Changes (`git add`)

Before changes can be committed to the repository's history, they must be added to the staging area (index).

```bash
# Stage a single specific file
git add my_first_code.txt

# Stage all modified and untracked files in the current folder
git add .

# Stage changes interactively hunk by hunk (advanced)
git add -p
```

---

## 2. Making Commits (`git commit`)

A **commit** in Git represents a complete snapshot of your staged files at a specific moment in time.

```bash
# Standard commit with a message
git commit -m "feat: implement user authentication endpoint"
```

> [!IMPORTANT]
> **Commit Early and Commit Often:**  
> A commit should represent an atomic unit of work (e.g., adding a feature, fixing a specific bug, or adding tests). Do not wait until an entire multi-week project is done to make one giant commit.

### What Happens Without a Message?
If you run `git commit` without `-m`, Git opens your configured text editor (Vim or Notepad). If you exit the editor with an empty message, Git **aborts the commit** because every commit requires an explanatory message.

### Skipping the Staging Step (`-a` / `-am`)
If files are **already tracked** by Git (i.e., not newly created files), you can combine staging and committing:

```bash
# Stages all modified tracked files and commits in one command
git commit -am "fix: correct calculation in discount service"
```

> [!WARNING]
> `git commit -am` will **NOT** stage untracked (newly created) files. New files must always be staged at least once using `git add`.

---

## 3. Cryptographic Integrity: SHA Checksums

A cornerstone of Git's architecture is cryptographic data integrity:

```text
Commit Hash Example:
40-character SHA-1 checksum:  fd914d528b9c6a1e3b092a48ef61a298c4391b01
Shortened display (7 chars):  fd914d5
```

### How Checksums Work
1. Git calculates a **SHA-1 (or SHA-256)** hash for every object (blob, tree, commit).
2. The commit checksum is computed from:
   - The tree hash (all files and folders in that snapshot)
   - Parent commit hash
   - Author and committer names, emails, and exact timestamps
   - Commit message
3. **Immutability:** Even changing a single whitespace character in a file completely changes its blob hash, which cascades up to change the tree hash and the commit hash. It is mathematically impossible to change anything in Git history without Git knowing.
4. Git displays the **first 7 hexadecimal characters** by default because 7 hex digits ($16^7 \approx 268$ million combinations) provide ample uniqueness for almost all repositories.

---

## 4. Viewing History with `git log`

`git log` displays your repository's chronological commit history in reverse order (most recent first):

```bash
git log
```

### Example Log Output:
```text
commit fd914d528b9c6a1e3b092a48ef61a298c4391b01 (HEAD -> main)
Author: Navin Reddy <navin@example.com>
Date:   Tue Oct 6 10:30:00 2026 +0530

    feat: my first commit
```

- `commit <40-char-hash>`: Unique identifier of this commit snapshot.
- `(HEAD -> main)`: Indicates that `HEAD` (your current location) points to branch `main`, which points to this commit.
- `Author`: Identity configured in `git config`.
- `Date`: Timestamp when the commit was created.

---

## 5. Command Reference Table

| Command | Purpose |
|---|---|
| `git add <file>` | Stage specific file for next commit |
| `git add .` | Stage all modified and new files in directory |
| `git commit -m "<msg>"` | Create commit with inline message |
| `git commit -am "<msg>"` | Stage modified tracked files and commit in one step |
| `git log` | Show full chronological commit history |
| `git restore --staged <file>` | Unstage a file while keeping its changes in working tree |
