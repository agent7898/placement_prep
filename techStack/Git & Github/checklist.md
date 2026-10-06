# Master Study Checklist — Git & GitHub

A comprehensive progress tracker for mastering Git (CLI, internals, and architecture) and GitHub (collaboration, SSH, PRs, and releases).

> **Reference Video Course:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## Module Progress Tracker

- [ ] **[01. VCS Fundamentals & Git Origin](01-vcs-fundamentals/checklist.md)**
  - [ ] Understand drawbacks of manual folder versioning (`quiz-app-1`, `final`, `final-final`).
  - [ ] Compare Local VCS vs Centralized VCS (CVCS) vs Distributed VCS (DVCS).
  - [ ] Understand the 2005 Linux kernel and BitKeeper dispute leading to Git's creation by Linus Torvalds.
  - [ ] Understand snapshot-based storage vs delta-based storage.

- [ ] **[02. Installation & Configuration](02-installation-and-config/checklist.md)**
  - [ ] Install and verify Git (`git --version`).
  - [ ] Configure global author identity (`user.name` and `user.email`).
  - [ ] Set `init.defaultBranch` to `main`.
  - [ ] Understand configuration scopes (System, Global, Local) and precedence.
  - [ ] Inspect active configs with `git config --list --show-origin`.

- [ ] **[03. Git Architecture & The Three Areas](03-git-architecture-and-areas/checklist.md)**
  - [ ] Differentiate Working Directory vs Git Repository (`.git/`).
  - [ ] Master the 3 local areas: Working Directory, Staging Area (Index), and Local Repo.
  - [ ] Understand repository initialization (`git init -b main`).
  - [ ] Interpret `git status` across untracked, modified, staged, and clean states.

- [ ] **[04. Staging & Committing](04-staging-and-committing/checklist.md)**
  - [ ] Stage files using `git add <file>` and `git add .`.
  - [ ] Commit with atomic messages (`git commit -m "..."`).
  - [ ] Use `git commit -am` shortcut on tracked files.
  - [ ] Understand SHA-1 checksum immutability (40-char full hash vs 7-char short hash).
  - [ ] Inspect commit history and pointers with `git log`.

- [ ] **[05. Inspecting Changes & Diffs](05-inspecting-changes-and-diffs/checklist.md)**
  - [ ] Inspect unstaged modifications with `git diff`.
  - [ ] Inspect staged modifications with `git diff --staged`.
  - [ ] Understand unified diff syntax (`---`, `+++`, `@@`, `+`, `-`).
  - [ ] View compact single-line logs (`git log --oneline`).
  - [ ] Inspect specific commit diffs with `git show <commit-hash>`.

- [ ] **[06. Untracking & Removing Files](06-untracking-and-removing-files/checklist.md)**
  - [ ] Untrack files from Git while preserving on disk (`git rm --cached <file>`).
  - [ ] Delete files from both Git and disk (`git rm <file>`).
  - [ ] Configure `.gitignore` to prevent tracking of secrets, logs, and build artifacts.
  - [ ] Understand why `.gitignore` does not ignore already-tracked files.

- [ ] **[07. Remote Repositories & GitHub](07-remote-repositories-and-github/checklist.md)**
  - [ ] Differentiate local vs remote repositories (GitHub, GitLab, Bitbucket).
  - [ ] Set up passwordless SSH authentication (`ssh-keygen`, adding public key to GitHub).
  - [ ] Verify SSH connectivity with `ssh -T git@github.com`.
  - [ ] Link local repo to remote (`git remote add origin <url>`).
  - [ ] Push with upstream tracking (`git push -u origin main`).
  - [ ] Understand why open-source repositories reject direct pushes and require Forks + PRs.

- [ ] **[08. Git Tags & Releases](08-git-tags-and-releases/checklist.md)**
  - [ ] Understand static Tags vs moving Branches.
  - [ ] Create annotated release tags (`git tag -a v1.0 -m "..."`).
  - [ ] Inspect tag details (`git show v1.0`).
  - [ ] Push tags to GitHub (`git push origin <tag>` and `git push origin --tags`).

- [ ] **[09. Git Branching & Navigation](09-git-branching-and-navigation/checklist.md)**
  - [ ] Understand how Git branches are implemented as lightweight 41-byte pointers.
  - [ ] Create and switch branches using modern `git switch -c <name>` and `git switch <name>`.
  - [ ] Understand how Git swaps disk files when switching branches.
  - [ ] Safely delete branches with `git branch -d <name>`.
  - [ ] Push feature branches to remote (`git push -u origin <branch>`).

- [ ] **[10. Merging & Synchronization](10-merging-and-synchronization/checklist.md)**
  - [ ] Execute the disciplined 5-step merge workflow (switch -> pull -> merge -> verify -> push).
  - [ ] Understand Fast-Forward merges vs 3-way merge commits.
  - [ ] Read conflict markers (`<<<<<<< HEAD`, `=======`, `>>>>>>>`).
  - [ ] Resolve merge conflicts and finalize merges.
  - [ ] Abort conflicting merges safely with `git merge --abort`.

---

## Final Technical Interview Readiness
- [ ] Answer the Top 20 Rapid Revision Questions in under 30 seconds each.
- [ ] Articulate the difference between `git merge` and `git rebase` (with the Golden Rule of Rebase).
- [ ] Articulate the differences between `git reset` (`--soft`, `--mixed`, `--hard`) and `git revert`.
- [ ] Explain how to recover lost commits using `git reflog`.
