# Git & GitHub — Placement Preparation Handbook

A comprehensive, interview-focused study guide and reference for **Git (CLI & Internals)** and **GitHub (Collaboration & Workflows)** for placement assessments, machine coding rounds, and technical interviews.

---

## Table of Contents

1. [Mental Model & Git Architecture](#1-mental-model--git-architecture)
2. [Git Internals: How Git Stores Data](#2-git-internals-how-git-stores-data)
3. [The 4 Stages of Git](#3-the-4-stages-of-git)
4. [Essential Command Reference](#4-essential-command-reference)
5. [Key Interview Comparisons](#5-key-interview-comparisons)
   - [Git Merge vs Git Rebase](#git-merge-vs-git-rebase)
   - [Git Reset vs Git Revert](#git-reset-vs-git-revert)
   - [Git Fetch vs Git Pull](#git-fetch-vs-git-pull)
   - [Git Switch & Restore vs Git Checkout](#git-switch--restore-vs-git-checkout)
6. [Merge Conflicts: Anatomy & Resolution](#6-merge-conflicts-anatomy--resolution)
7. [GitHub & Collaborative Workflows](#7-github--collaborative-workflows)
8. [Rapid Revision Sheet (20 High-Yield Questions)](#8-rapid-revision-sheet-20-high-yield-questions)
9. [Real-World Interview Scenarios & Drills](#9-real-world-interview-scenarios--drills)
10. [Traps That Cost Marks in Interviews](#10-traps-that-cost-marks-in-interviews)

---

## 1. Mental Model & Git Architecture

### Centralized VCS vs Distributed VCS (DVCS)

| Feature | Centralized VCS (SVN, CVS, Perforce) | Distributed VCS (Git, Mercurial) |
|---|---|---|
| **Repository Storage** | Only on a single central server. | Every developer has a complete local clone of the entire repository history. |
| **Offline Work** | Limited. Commits and branching require server access. | Full offline capability. Commits, branching, diffs, and logs run locally. |
| **Single Point of Failure** | Server failure halts development; disk failure risks losing history. | Any developer's local clone can serve as a full backup of the codebase. |
| **Speed** | Network-bound for most actions. | Blazing fast (local disk I/O for 90% of operations). |

---

## 2. Git Internals: How Git Stores Data

A common interview question in product-based companies is: *"How does Git actually store data under the `.git` directory?"*

Git is fundamentally a **content-addressable filesystem with a directed acyclic graph (DAG) of commit objects**.

```text
Working Directory → Hash SHA-1/SHA-256 → Git Object Database (.git/objects/)
```

### The 4 Primary Git Objects

1. **Blob (Binary Large Object):**
   - Stores raw file **content** only (no file name, permissions, or timestamps).
   - If two files in different directories have identical content, Git stores only one blob.
2. **Tree:**
   - Represents a directory.
   - Points to blobs (with their filenames and file modes) and nested child trees (subdirectories).
3. **Commit:**
   - Contains a pointer to the top-level root tree object.
   - Contains metadata: author, committer, timestamp, commit message.
   - Points to zero, one, or more parent commit hashes (forms the DAG).
4. **Annotated Tag:**
   - A permanent reference pointing to a specific commit, containing tagger metadata and a message.

### Pointers and References

- **Branch:** A lightweight, movable pointer to a specific commit hash (stored in `.git/refs/heads/<branch-name>`). It weighs only 41 bytes (40-char SHA + newline).
- **`HEAD`:** A pointer to the currently checked-out branch or commit (stored in `.git/HEAD`).
- **Detached `HEAD` State:** Occurs when `HEAD` points directly to a commit hash rather than a named branch. Any new commits made in this state have no branch reference and will eventually be garbage-collected unless a new branch is created pointing to them.

---

## 3. The 4 Stages of Git

```text
 ┌────────────────┐      git add      ┌───────────────┐     git commit    ┌──────────────────┐
 │    Working     │ ────────────────> │ Staging Area  │ ────────────────> │ Local Repository │
 │   Directory    │ <──────────────── │   (Index)     │                   │     (.git/)      │
 └────────────────┘   git restore     └───────────────┘                   └──────────────────┘
         │                                                                          │
         │                              git push                                    │
         │                         ───────────────────>                             │
         │                                                                          ▼
         │                              git pull                          ┌──────────────────┐
         └─────────────────────────────────────────────────────────────── │ Remote Repo      │
                                (git fetch + git merge)                   │ (GitHub/GitLab)  │
                                                                          └──────────────────┘
```

1. **Working Directory:** The actual local files and directories on your file system that you edit in your IDE.
2. **Staging Area (Index):** A binary file (`.git/index`) listing files prepared for the next commit. Allows atomic, intentional grouping of changes.
3. **Local Repository:** The `.git` directory containing your local commit DAG, branches, tags, and object store.
4. **Remote Repository:** The hosted copy of your repository on a remote server (e.g., GitHub, GitLab, Bitbucket).

---

## 4. Essential Command Reference

### Repository Setup & Configuration

```bash
# Set global identity (required on first setup)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Initialize a new local repository
git init

# Clone an existing remote repository
git clone <remote-url> [optional-folder-name]

# Manage remotes
git remote -v                          # List configured remotes with URLs
git remote add origin <remote-url>     # Link local repo to remote named 'origin'
git remote set-url origin <new-url>    # Update remote URL
```

### Checking Status, History & Inspection

```bash
# Check status of working tree and staging area
git status
git status -s                          # Short format (?? untracked, M modified, A added)

# View commit history
git log                                # Full commit log
git log --oneline                      # Compact single-line summary
git log --oneline --graph --decorate --all # Beautiful ASCII graph of all branches
git log -p -2                          # Show patch (code diff) of last 2 commits
git log --stat                         # Show file change statistics

# Inspect diffs
git diff                               # Diff between Working Directory and Staging Area
git diff --staged                      # Diff between Staging Area and HEAD commit
git diff branchA..branchB              # Diff between tips of two branches

# Inspect specific objects & line history
git show <commit-hash>                 # Detailed view of a commit and its changes
git blame <filename>                   # Shows who changed each line and when
```

### Staging & Committing

```bash
# Staging changes
git add <filename>                     # Stage a single file
git add .                              # Stage all modified and untracked files in directory
git add -p                             # Interactive patch mode (stage specific hunks)

# Committing
git commit -m "feat: descriptive message"
git commit -am "fix: stage tracked files and commit" # Skips explicit 'git add' (tracked only)
git commit --amend -m "new message"    # Modify the most recent commit (hash changes!)
```

### Branching & Navigating

```bash
# Branch management
git branch                             # List local branches
git branch -a                          # List local and remote-tracking branches
git branch <new-branch>                # Create branch without switching
git branch -d <branch-name>            # Safe delete (only if merged)
git branch -D <branch-name>            # Force delete unmerged branch
git branch -m <old-name> <new-name>    # Rename branch

# Switching branches (Modern Git 2.23+)
git switch <branch-name>               # Switch to existing branch
git switch -c <new-branch>             # Create and immediately switch to new branch

# Legacy switching (still widely seen in interviews)
git checkout <branch-name>
git checkout -b <new-branch>
```

### Synchronizing with Remote

```bash
# Fetch updates without modifying working directory
git fetch origin

# Fetch and merge into current local branch
git pull origin <branch-name>

# Fetch and rebase current branch onto remote branch
git pull --rebase origin <branch-name>

# Push local commits to remote
git push -u origin <branch-name>       # Push and set upstream tracking
git push                               # Push current tracking branch
git push --force-with-lease            # Safe force push (checks if remote has changed)
```

### Stashing (Temporarily Shelving Work)

```bash
git stash                              # Save uncommitted changes to stash stack
git stash save "WIP on auth feature"   # Save with custom message
git stash list                         # View list of saved stashes
git stash pop                          # Apply newest stash and remove it from list
git stash apply                        # Apply newest stash but keep it in list
git stash apply stash@{1}              # Apply specific stash entry
git stash drop stash@{0}               # Delete specific stash
git stash clear                        # Wipe all stashes
```

---

## 5. Key Interview Comparisons

### Git Merge vs Git Rebase

This is among the **top 3 most asked Git questions** in technical interviews.

| Aspect | `git merge` | `git rebase` |
|---|---|---|
| **Mechanism** | Creates a new **3-way merge commit** with two parent pointers (unless fast-forward is possible). | Replays individual commits from the current branch one by one on top of the target branch, generating **new commit hashes**. |
| **History Shape** | Preserves the exact true historical branching topology (non-linear graph). | Creates a clean, linear, single-stream history without merge bubbles. |
| **Commit Hashes** | Keeps existing commit hashes untouched. | Rewrites commit hashes for replayed commits. |
| **Traceability** | Easy to trace when a feature was integrated; easy to revert entire feature merge. | Commit history is simplified, but original context and timestamps can become misleading. |
| **Conflict Resolution** | Resolve all conflicts once inside the merge commit. | You may have to resolve conflicts repeatedly for each individual commit being replayed. |

#### Merge Visual:
```text
      C1 ─── C2 (feature)
     /         \
A ── B ───────── M (main: 3-way merge commit)
```

#### Rebase Visual:
```text
A ── B ── C1' ── C2' (feature commits rewritten sequentially on top of B)
```

> [!IMPORTANT]
> **The Golden Rule of Rebasing:**
> **NEVER rebase commits that have been pushed to a public, shared branch** (such as `main` or `develop`). Rebasing rewrites history and changes commit hashes. Anyone else collaborating on that branch will encounter severe divergence and corrupted history. Use rebase for local feature cleanup before pushing.

---

### Git Reset vs Git Revert

| Aspect | `git reset` | `git revert` |
|---|---|---|
| **Action** | Moves the branch pointer backward in time; rewrites local commit history. | Creates a **new forward commit** that applies the inverse diff of the target commit. |
| **Safe for Public Branches?** | ❌ **No**. If already pushed, resetting requires force push, breaking teammates' branches. | ✅ **Yes**. Completely safe because it appends history rather than erasing it. |
| **Scope** | Manipulates `HEAD`, Index, and Working Directory depending on flag. | Only targets specific commits to invert changes. |

#### The 3 Modes of `git reset`:

```bash
git reset <mode> HEAD~1
```

| Mode | `HEAD` Moved? | Staging Area (Index) | Working Directory | Typical Use Case |
|---|---|---|---|---|
| `--soft` | Yes | **Preserved** (changes stay staged) | **Untouched** | Combine/squash last few commits into one, or fix commit message. |
| `--mixed` *(Default)* | Yes | **Reset** (changes unstaged) | **Untouched** | You committed too early; want to re-stage files selectively. |
| `--hard` | Yes | **Reset** (erased) | **Overwritten** (erased) | Completely discard uncommitted changes and recent commits. **(Dangerous!)** |

---

### Git Fetch vs Git Pull

- **`git fetch`:** Downloads commits, files, and refs from the remote repository into your local `.git` object database. **It does NOT modify your working directory or merge into your local branch.**
  - Updates remote tracking branches like `origin/main`.
  - Completely safe to run at any time to inspect incoming changes before merging.
- **`git pull`:** Equivalent to running `git fetch` followed immediately by `git merge FETCH_HEAD`.
  - Automatically attempts to merge remote commits into your active local working branch.
  - Can trigger merge conflicts while you are in the middle of other work.
  - `git pull --rebase` runs `git fetch` followed by `git rebase` instead of merge.

---

### Git Switch & Restore vs Git Checkout

In Git 2.23+, `git checkout` was split into two focused commands to reduce confusion:

| Operation | Legacy Command | Modern Command (Recommended) |
|---|---|---|
| Switch branches | `git checkout <branch>` | `git switch <branch>` |
| Create and switch branch | `git checkout -b <new-branch>` | `git switch -c <new-branch>` |
| Discard working tree changes | `git checkout -- <file>` | `git restore <file>` |
| Unstage a staged file | `git reset HEAD <file>` | `git restore --staged <file>` |

---

## 6. Merge Conflicts: Anatomy & Resolution

### Why Conflicts Occur
A conflict occurs when Git attempts a 3-way merge or rebase and finds that **the exact same lines of code were modified differently** in both branches since their common ancestor commit.

### Anatomy of Conflict Markers

When a conflict occurs, Git marks the affected file with special delimiter lines:

```text
<<<<<<< HEAD
const API_URL = "https://api.production.company.com/v1";
=======
const API_URL = "https://api.staging.company.com/v2";
>>>>>>> feature-auth
```

- `<<<<<<< HEAD`: Marks the start of changes from your **currently checked-out branch**.
- `=======`: The separator between conflicting versions.
- `>>>>>>> feature-auth`: Marks the end of changes from the **incoming branch being merged**.

### Step-by-Step Resolution Process

1. **Detect conflicting files:**
   ```bash
   git status
   # Shows "both modified: config/api.js"
   ```
2. **Open the conflicting files and resolve:**
   - Review both sides with teammates or code context.
   - Edit the file: keep the correct code, remove unwanted code, and **delete all conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`).
3. **Stage the resolved files:**
   ```bash
   git add config/api.js
   ```
4. **Complete the merge or rebase:**
   ```bash
   # If in a merge:
   git commit -m "chore: resolve merge conflict in api config"

   # If in a rebase:
   git rebase --continue
   ```
5. **How to abort if things go wrong:**
   ```bash
   git merge --abort    # Reverts repository to state before git merge started
   git rebase --abort   # Reverts repository to state before git rebase started
   ```

---

## 7. GitHub & Collaborative Workflows

### GitHub vs Git

- **Git:** The local command-line distributed version control system tool.
- **GitHub:** A cloud-based hosting platform for Git repositories providing collaboration tools (Pull Requests, Issue tracking, Actions CI/CD, Project Boards, Releases).

### Common Branching Workflows

#### 1. GitHub Flow (Standard in Startups & Agile Teams)
1. Branch off `main` with a descriptive name (`feat/user-login`, `fix/typo`).
2. Make commits with clear, atomic messages.
3. Open a **Pull Request (PR)** on GitHub early for discussion and review.
4. Run automated tests (CI).
5. Address peer review comments.
6. Merge into `main` and deploy immediately.

#### 2. Git Flow (Classic for Scheduled Releases)
- `main`: Production-ready code only.
- `develop`: Ongoing integration for the next release.
- `feature/*`: Branched from `develop`, merged back to `develop`.
- `release/*`: Branched from `develop` when preparing a release; merged to both `main` and `develop`.
- `hotfix/*`: Branched directly from `main` to patch urgent production bugs; merged to both `main` and `develop`.

### Conventional Commits Standard

Many tech companies enforce structured commit messages:

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Common types:
- `feat`: A new feature for the user
- `fix`: A bug fix
- `docs`: Documentation changes only
- `style`: Formatting, missing semi-colons (no code logic changes)
- `refactor`: Refactoring code without adding features or fixing bugs
- `test`: Adding or correcting tests
- `chore`: Build tasks, package updates, tooling configurations

---

## 8. Rapid Revision Sheet (20 High-Yield Questions)

Answerable in under 30 seconds each:

| # | Question | 30-Second Model Answer |
|---|---|---|
| **1** | What is Git? | A distributed version control system tracking file changes via a content-addressable commit graph (DAG). |
| **2** | What are the 3 main local states in Git? | Modified (Working Tree), Staged (Index), and Committed (Local Repository). |
| **3** | What does `git add .` do? | Stages all modified and new files in the current working directory into the index. |
| **4** | What is a commit hash? | A 40-character SHA-1 (or 64-char SHA-256) checksum uniquely identifying a commit object and its contents. |
| **5** | What is `HEAD`? | A reference pointer pointing to the current checked-out branch or commit. |
| **6** | What is a detached `HEAD`? | When `HEAD` points directly to a specific commit hash rather than a named branch reference. |
| **7** | What is a fast-forward merge? | When the target branch has no new commits since branching; Git simply moves the branch pointer forward without creating a merge commit. |
| **8** | What is a 3-way merge? | A merge combining changes from two branches and their common ancestor commit, creating a new merge commit with two parents. |
| **9** | What is the key difference between merge and rebase? | Merge creates a merge commit preserving historical topology; rebase replays commits linearly on top of another branch, rewriting hashes. |
| **10** | When should you avoid `git rebase`? | Never rebase commits that have already been pushed to a public/shared branch. |
| **11** | Difference between `git reset --soft` and `--hard`? | `--soft` moves HEAD only, keeping changes staged; `--hard` resets HEAD, index, and wipes working tree changes. |
| **12** | Difference between `git reset` and `git revert`? | `reset` erases commits from local history; `revert` creates a new forward commit that inverses the changes safely. |
| **13** | Difference between `git fetch` and `git pull`? | `fetch` downloads remote objects without touching your working files; `pull` fetches and immediately merges into your working branch. |
| **14** | What is `git stash`? | Temporarily shelves uncommitted changes so you have a clean working directory without making a premature commit. |
| **15** | How do you recover a deleted commit or lost branch? | Run `git reflog` to find the lost commit hash, then `git checkout <hash>` or `git branch <name> <hash>`. |
| **16** | What is `git cherry-pick`? | Applies the changes introduced by a specific single commit from one branch onto the current working branch. |
| **17** | How does `.gitignore` work? | Specifies glob patterns of files/directories Git should ignore from tracking. Tracked files must be unstaged via `git rm --cached`. |
| **18** | What does `git blame` do? | Annotates each line of a file with the author, timestamp, and commit hash of who last modified it. |
| **19** | What is a Pull Request (PR)? | A GitHub/GitLab mechanism proposing changes from one branch into another, enabling code review, CI checks, and discussion. |
| **20** | Why use `git push --force-with-lease` instead of `--force`? | It only force-pushes if no one else has updated the remote branch in the meantime, preventing accidental overwrites. |

---

## 9. Real-World Interview Scenarios & Drills

### Scenario 1: "I accidentally committed an API secret or password. How do I fix it?"
- **If not yet pushed:**
  1. Remove secret from the file.
  2. If it was the most recent commit:
     ```bash
     git add <file>
     git commit --amend --no-edit
     ```
- **If already pushed to remote:**
  1. **Immediately revoke / rotate the secret** (assume it is already compromised).
  2. Use `git filter-repo` or BFG Repo-Cleaner to scrub the secret from all history, then force-push.

### Scenario 2: "I accidentally ran `git reset --hard` and lost my work! Can I get it back?"
- **Answer:** Yes! As long as the work was committed at least once:
  1. Run `git reflog` (reference log records every movement of `HEAD` for 30–90 days).
  2. Identify the commit hash right before the reset (e.g. `HEAD@{1}`).
  3. Restore it:
     ```bash
     git reset --hard HEAD@{1}
     # Or create a recovery branch:
     git branch recovery-branch HEAD@{1}
     ```

### Scenario 3: "I started coding on `main` instead of creating a feature branch. I haven't committed yet. What do I do?"
- **Answer:**
  ```bash
  # Option A (simplest): Git allows switching branches while keeping dirty working tree
  git switch -c feature/my-new-feature
  # All uncommitted files move directly to the new branch!

  # Option B (using stash):
  git stash
  git switch -c feature/my-new-feature
  git stash pop
  ```

### Scenario 4: "I need a bugfix commit from branch `dev-2.0` into `production-1.0`, but not the other 20 commits."
- **Answer:** Use `git cherry-pick`:
  ```bash
  git switch production-1.0
  git cherry-pick <commit-hash-of-bugfix>
  ```

---

## 10. Traps That Cost Marks in Interviews

1. **Confusing Git with GitHub:** Treating them as synonyms in an interview reveals a lack of core tooling fundamentals. Always state: Git is the DVCS engine; GitHub is the hosting and collaboration platform.
2. **Believing `git reset --hard` destroys everything:** Uncommitted changes that were never added to the index are permanently lost, but any changes that were committed once are recoverable via `git reflog`.
3. **Explaining `.gitignore` without mentioning tracked files:** Adding an existing file to `.gitignore` does **not** stop Git from tracking it if it was already committed in the past. You must explicitly run `git rm --cached <file>`.
4. **Blind `git push --force`:** Never recommend plain `--force` in an interview answer; always demonstrate senior hygiene by recommending `git push --force-with-lease`.
5. **Saying Git stores diffs:** A very common misconception! VCS like SVN store file deltas/diffs; **Git stores snapshots of files as complete objects (blobs) organized in tree hierarchies**.
