# Checklist — 09: Git Branching & Navigation

Track your mastery of branching, switching, and Git's pointer internals.

---

### Concepts
- [ ] Understand why branching in Git is virtually instantaneous compared to older VCS.
- [ ] Understand what a branch physically is inside the `.git` folder (`.git/refs/heads/`).
- [ ] Understand how `HEAD` moves between branches.
- [ ] Understand why uncommitted files disappear and reappear when switching branches.
- [ ] Distinguish between `git checkout -b` and modern `git switch -c`.
- [ ] Understand safe branch deletion (`-d`) versus force branch deletion (`-D`).

---

### Commands & Practice
- [ ] Create and switch to a branch with `git switch -c feature-1`.
- [ ] Verify active branch using `git branch`.
- [ ] Switch between branches using `git switch main` and `git switch -`.
- [ ] Delete a merged branch using `git branch -d feature-1`.
- [ ] Push a feature branch to GitHub with `git push -u origin feature-1`.
- [ ] View branch topology with `git log --oneline --graph --all`.

---

### Interview Verification Questions
- [ ] How does Git implement branches under the hood?
- [ ] What is the difference between `git checkout <branch>` and `git switch <branch>`?
- [ ] What is a "detached HEAD" state, and how do you recover from it?
