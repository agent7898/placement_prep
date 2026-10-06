# Checklist — 10: Merging & Synchronization

Track your mastery of branch merging, pull workflows, and conflict handling.

---

### Concepts
- [ ] Understand the 5-step professional merge workflow (switch -> pull -> merge -> verify -> push).
- [ ] Understand why pulling remote updates before merging is critical in teams.
- [ ] Differentiate between a Fast-Forward merge and a 3-way merge commit.
- [ ] Understand what causes merge conflicts and how Git identifies them.
- [ ] Know how to read conflict markers (`<<<<<<< HEAD`, `=======`, `>>>>>>>`).
- [ ] Know how to safely abort a merge using `git merge --abort`.

---

### Commands & Practice
- [ ] Switch to main branch with `git switch main`.
- [ ] Pull latest changes with `git pull origin main`.
- [ ] Merge a feature branch using `git merge feature-1`.
- [ ] Inspect the merged commit history with `git log --oneline --graph`.
- [ ] Push the merged branch to remote with `git push origin main`.
- [ ] Simulate and resolve a merge conflict in a test file.

---

### Interview Verification Questions
- [ ] What is the difference between `git merge` and `git rebase`?
- [ ] What happens during a fast-forward merge, and how can you prevent it?
- [ ] How do you cancel an active merge conflict without losing your pre-merge work?
