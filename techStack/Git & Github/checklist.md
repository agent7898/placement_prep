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

- [ ] **[11. Advanced Merging & Conflict Resolution](11-advanced-merging-and-conflict-resolution/checklist.md)**
  - [ ] Understand 3-way merge ancestor mechanics and `git merge-base`.
  - [ ] Differentiate merge strategies (`ort`, `recursive`, `ours`, `octopus`).
  - [ ] Master `-s ours` vs `-X ours` and whitespace ignoring flags.
  - [ ] Resolve complex conflicts: content, modify/delete, add/add, and binary files (`--ours`/`--theirs`).
  - [ ] Configure and master `git rerere` (Reuse Recorded Resolution).

- [ ] **[12. Git Rebase & History Rewriting](12-git-rebase-and-history-rewriting/checklist.md)**
  - [ ] Understand commit replaying and new SHA hash generation during rebasing.
  - [ ] Understand why public shared branches must never be rebased (Golden Rule).
  - [ ] Master interactive rebase (`git rebase -i`): `pick`, `squash`, `fixup`, `drop`, `edit`, `exec`.
  - [ ] Automate clean history with `git commit --fixup` and `git rebase -i --autosquash`.
  - [ ] Transplant branches with `git rebase --onto`.
  - [ ] Resolve rebase conflicts (`--continue`, `--skip`, `--abort`).
  - [ ] Force push safely with `git push --force-with-lease`.

- [ ] **[13. Git Protocols & Network Transfers](13-git-protocols-and-network-transfers/checklist.md)**
  - [ ] Compare Git's 4 transfer protocols: Local, Smart HTTPS, SSH, and Git protocol.
  - [ ] Understand Git Wire Protocol v2 improvements over Protocol v1.
  - [ ] Generate and authenticate using Ed25519 SSH keys.
  - [ ] Configure GitHub Personal Access Tokens (Classic vs Fine-Grained) and credential helpers.
  - [ ] Master clone optimizations: Shallow (`--depth`), Blobless (`--filter=blob:none`), Treeless, and Sparse Checkout.

- [ ] **[14. GitHub Collaboration & Workflows](14-github-collaboration-and-workflows/checklist.md)**
  - [ ] Compare the Shared Repository Model vs the Fork & PR Model.
  - [ ] Master industry branching strategies: GitHub Flow, Gitflow, Trunk-Based Development.
  - [ ] Synchronize forked repositories with upstream remotes (`git remote add upstream`).
  - [ ] Differentiate GitHub PR merge methods: Merge Commit vs Squash & Merge vs Rebase & Merge.
  - [ ] Configure branch protection rules and GitHub rulesets.
  - [ ] Automate review routing using `.github/CODEOWNERS`.

- [ ] **[15. Advanced Git Tooling & Internals](15-advanced-git-tooling-and-internals/checklist.md)**
  - [ ] Master advanced stashing (`-u`, `-a`, `--keep-index`, `stash branch`).
  - [ ] Recover lost commits and deleted branches using `git reflog`.
  - [ ] Cherry-pick commits and ranges (`git cherry-pick`).
  - [ ] Automate bug diagnosis using binary search with `git bisect` and test scripts.
  - [ ] Work on multiple branches concurrently using `git worktree`.
  - [ ] Understand client-side and server-side Git hooks and Husky.
  - [ ] Scrub historical credentials using `git filter-repo`.
  - [ ] Understand Git database internals: Loose objects vs Packfiles, `git gc`, and `git fsck`.

---

## Final Technical Interview Readiness
- [ ] Answer the Top 30 Rapid Revision Questions in under 30 seconds each.
- [ ] Articulate the difference between `git merge` and `git rebase` (with the Golden Rule of Rebase).
- [ ] Articulate the differences between `git reset` (`--soft`, `--mixed`, `--hard`) and `git revert`.
- [ ] Explain how to recover lost commits using `git reflog`.
- [ ] Explain how `git bisect` finds regressions with $O(\log N)$ efficiency.
- [ ] Differentiate between GitHub PR merge methods and explain when to use Squash & Merge.
- [ ] Explain why `--force-with-lease` is mandatory when pushing rewritten history in teams.
