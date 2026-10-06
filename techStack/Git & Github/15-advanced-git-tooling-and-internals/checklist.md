# Checklist — 15: Advanced Git Tooling & Internals

Track your mastery of advanced stash workflows, reflog recovery, cherry-picking, bisect debugging, git worktrees, hooks, history scrubbing, and repository maintenance.

---

### Concepts
- [ ] Understand `git stash` options (`-u` untracked, `-a` all, `--keep-index`, `stash branch`).
- [ ] Understand how `git reflog` tracks reference changes and enables disaster recovery.
- [ ] Understand commit cherry-picking and range cherry-picks (`A..B`).
- [ ] Understand binary search debugging with `git bisect` and automated script testing.
- [ ] Master `git worktree` for concurrent multi-branch workspaces without branch switching.
- [ ] Understand Git hooks architecture (client-side vs server-side, pre-commit, commit-msg, Husky).
- [ ] Understand history rewriting and secret purging with `git-filter-repo`.
- [ ] Understand Git database maintenance (`git gc`, packfiles vs loose objects, `git fsck`).
- [ ] Differentiate between Git Submodules and Git Subtrees.

---

### Commands & Practice
- [ ] Stash changes including untracked files: `git stash -u -m "WIP feature"`.
- [ ] Resurrect a stash directly into a new branch: `git stash branch <branch-name>`.
- [ ] Recover an accidentally deleted commit or branch using `git reflog`.
- [ ] Cherry-pick a specific commit onto another branch: `git cherry-pick <hash>`.
- [ ] Run an automated `git bisect` run with a test command.
- [ ] Create and work in a secondary branch worktree: `git worktree add ../hotfix main`.
- [ ] List and clean up worktrees: `git worktree list` and `git worktree remove`.
- [ ] Create a local executable `pre-commit` hook in `.git/hooks/pre-commit`.
- [ ] Optimize repository object storage with `git gc --prune=now`.

---

### Interview Verification Questions
- [ ] How can you recover commits lost due to an accidental `git reset --hard`?
- [ ] What is the time complexity of `git bisect` when searching across 1,000 commits?
- [ ] How does `git worktree` prevent the context switching overhead of `git stash`?
- [ ] Why is `git-filter-repo` preferred over `git filter-branch` for sanitizing repository history?
