# Checklist — 12: Git Rebase & History Rewriting

Track your mastery of standard rebase, interactive rebasing, autosquash, onto, and safe force-push practices.

---

### Concepts
- [ ] Understand how `git rebase` replays commits onto a new base commit DAG.
- [ ] Understand why rebased commits receive new SHA commit hashes.
- [ ] Differentiate the visual history and audit tradeoffs between merge and rebase.
- [ ] Master the Golden Rule of Rebasing and why public branches must never be rebased.
- [ ] Understand interactive rebase commands (`pick`, `reword`, `edit`, `squash`, `fixup`, `drop`, `exec`).
- [ ] Understand the fixup and autosquash workflow (`--fixup` + `--autosquash`).
- [ ] Understand branch transplanting with `git rebase --onto`.
- [ ] Understand why `git push --force-with-lease` is safe while `git push --force` is dangerous.

---

### Commands & Practice
- [ ] Rebase a local branch onto main: `git rebase main`.
- [ ] Clean up local commit history using interactive rebase: `git rebase -i HEAD~3`.
- [ ] Squash multiple WIP commits into a single descriptive commit.
- [ ] Create a fixup commit using `git commit --fixup <hash>`.
- [ ] Run an autosquash rebase using `git rebase -i --autosquash <base>`.
- [ ] Transplant a branched feature using `git rebase --onto main branch-a branch-b`.
- [ ] Resolve a rebase conflict using `git add` and `git rebase --continue`.
- [ ] Push rebased history safely with `git push --force-with-lease origin <branch>`.

---

### Interview Verification Questions
- [ ] What is the fundamental difference between `git merge` and `git rebase`?
- [ ] Why should you never run `git rebase` on a shared collaborative branch like `main`?
- [ ] What is the difference between `squash` and `fixup`?
- [ ] How does `git push --force-with-lease` protect against destroying a teammate's commits?
