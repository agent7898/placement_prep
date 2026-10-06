# Git & GitHub — Placement Preparation Handbook

A comprehensive, interview-focused study guide and reference for **Git (CLI & Internals)** and **GitHub (Collaboration & Workflows)** designed for placement assessments, machine coding rounds, and technical interviews.

> 📺 **Reference Video Course:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)  
> 📋 **Master Module Checklist:** [checklist.md](checklist.md)

---

## Curriculum & Subtopic Index

This guide is organized into 10 modular sub-topics, each containing in-depth explanations, practical CLI commands, real-world examples, and a dedicated checklist:

| # | Subtopic | Topic Guide | Practice Checklist | Key Focus |
|---|---|---|---|---|
| **01** | **VCS Fundamentals & Origin** | [Guide](01-vcs-fundamentals/README.md) | [Checklist](01-vcs-fundamentals/checklist.md) | Manual versioning pain (`quiz-app-1`, `final`), Local vs CVCS vs DVCS, BitKeeper dispute & Linus Torvalds. |
| **02** | **Installation & Config** | [Guide](02-installation-and-config/README.md) | [Checklist](02-installation-and-config/checklist.md) | Git installation, PowerShell/CMD/Git Bash, author identity, `init.defaultBranch main`, config precedence. |
| **03** | **Git Architecture & Areas** | [Guide](03-git-architecture-and-areas/README.md) | [Checklist](03-git-architecture-and-areas/checklist.md) | Working Directory vs `.git` folder, Staging Area (Index), Local Repo, `git init -b main`, `git status`. |
| **04** | **Staging & Committing** | [Guide](04-staging-and-committing/README.md) | [Checklist](04-staging-and-committing/checklist.md) | `git add`, `git commit -m`, `commit -am`, SHA-1 checksum integrity (40 chars vs 7 chars), `git log`. |
| **05** | **Inspecting Changes & Diffs** | [Guide](05-inspecting-changes-and-diffs/README.md) | [Checklist](05-inspecting-changes-and-diffs/checklist.md) | `git diff` vs `git diff --staged`, reading unified diff syntax, `git log --oneline`, pager navigation (`q`). |
| **06** | **Untracking & Removing Files** | [Guide](06-untracking-and-removing-files/README.md) | [Checklist](06-untracking-and-removing-files/checklist.md) | Sensitive credentials leak scenario, `git rm --cached`, deleting vs untracking, `.gitignore` rules. |
| **07** | **Remote Repos & GitHub** | [Guide](07-remote-repositories-and-github/README.md) | [Checklist](07-remote-repositories-and-github/checklist.md) | GitHub/GitLab/Bitbucket, SSH key setup (`ssh-keygen`), `git remote add origin`, `git push -u origin main`. |
| **08** | **Git Tags & Releases** | [Guide](08-git-tags-and-releases/README.md) | [Checklist](08-git-tags-and-releases/checklist.md) | Semantic versioning, annotated tags (`git tag -a`), lightweight tags, `git push origin --tags`. |
| **09** | **Branching & Navigation** | [Guide](09-git-branching-and-navigation/README.md) | [Checklist](09-git-branching-and-navigation/checklist.md) | Experimental branches, `git switch -c` vs `git checkout -b`, pointer DAG internals, working tree swapping. |
| **10** | **Merging & Synchronization** | [Guide](10-merging-and-synchronization/README.md) | [Checklist](10-merging-and-synchronization/checklist.md) | 5-step merge flow, pulling before merging, Fast-Forward vs 3-way merge, conflict anatomy & `git merge --abort`. |

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

1. **Working Directory:** The actual local files and directories on your filesystem that you edit in your IDE.
2. **Staging Area (Index):** A binary file (`.git/index`) listing files prepared for the next commit. Allows atomic, intentional grouping of changes.
3. **Local Repository:** The `.git` directory containing your local commit DAG, branches, tags, and object store.
4. **Remote Repository:** The hosted copy of your repository on a remote server (e.g., GitHub, GitLab, Bitbucket).

---

## 4. Key Placement Comparisons & Mental Models

### Git Merge vs Git Rebase

| Aspect | `git merge` | `git rebase` |
|---|---|---|
| **Mechanism** | Creates a new **3-way merge commit** with two parent pointers (unless fast-forward is possible). | Replays individual commits from the current branch one by one on top of the target branch, generating **new commit hashes**. |
| **History Shape** | Preserves the exact true historical branching topology (non-linear graph). | Creates a clean, linear, single-stream history without merge bubbles. |
| **Commit Hashes** | Keeps existing commit hashes untouched. | Rewrites commit hashes for replayed commits. |
| **Traceability** | Easy to trace when a feature was integrated; easy to revert entire feature merge. | Commit history is simplified, but original context and timestamps can become misleading. |
| **Conflict Resolution** | Resolve all conflicts once inside the merge commit. | You may have to resolve conflicts repeatedly for each individual commit being replayed. |

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
- `--soft`: Moves `HEAD` only. Changes remain staged in the Index.
- `--mixed` *(Default)*: Moves `HEAD` and resets Index. Changes remain in Working Directory as unstaged edits.
- `--hard`: Moves `HEAD`, resets Index, and overwrites Working Directory. *(Permanently discards uncommitted work!)*

---

### Git Fetch vs Git Pull

- **`git fetch`:** Downloads commits, files, and refs from the remote repository into your local `.git` object database. **It does NOT modify your working directory or merge into your local branch.**
- **`git pull`:** Equivalent to running `git fetch` followed immediately by `git merge FETCH_HEAD`. Attempts to merge remote commits into your active local working branch automatically.

---

## 5. Rapid Revision Sheet (20 High-Yield Questions)

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
| **19** | What is a Pull Request (PR)? | A GitHub mechanism proposing changes from one branch into another, enabling code review, CI checks, and discussion. |
| **20** | Why use `git push --force-with-lease` instead of `--force`? | It only force-pushes if no one else has updated the remote branch in the meantime, preventing accidental overwrites. |

---

## 6. Real-World Interview Scenarios & Drills

### Scenario 1: Accidental Secret Leak
```bash
# If not yet pushed:
git rm --cached .env
echo ".env" >> .gitignore
git commit --amend --no-edit

# If already pushed: Rotate the secret immediately, then scrub history with git filter-repo.
```

### Scenario 2: Recovering from Accidental `git reset --hard`
```bash
# 1. Inspect HEAD history
git reflog
# 2. Restore to pre-reset commit
git reset --hard HEAD@{1}
```

### Scenario 3: Cherry-Picking a Single Hotfix Commit
```bash
git switch main
git cherry-pick <commit-hash-of-fix>
```

---

## 7. Traps That Cost Marks in Interviews

1. **Confusing Git with GitHub:** Git is the local CLI DVCS engine; GitHub is the cloud platform for hosting and collaboration.
2. **Assuming `git reset --hard` is always unrecoverable:** Committed work is recoverable via `git reflog`.
3. **Explaining `.gitignore` without mentioning tracked files:** Adding an existing file to `.gitignore` does not ignore it until you run `git rm --cached`.
4. **Blind `git push --force`:** Always use `git push --force-with-lease` in collaborative settings.
5. **Believing Git stores diffs:** Git stores **snapshots** as SHA-addressed blobs in tree hierarchies, not line-by-line deltas.
