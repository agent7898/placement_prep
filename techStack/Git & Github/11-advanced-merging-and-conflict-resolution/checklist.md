# Checklist — 11: Advanced Merging & Conflict Resolution

Track your mastery of merge strategies, 3-way merge mechanics, complex conflict scenarios, and `git rerere`.

---

### Concepts
- [ ] Understand the 3-Way Merge algorithm and the three inputs ($Base$, $Ours$, $Theirs$).
- [ ] Understand how Git determines the common ancestor with `git merge-base`.
- [ ] Differentiate between merge strategies (`ort`, `recursive`, `resolve`, `ours`, `octopus`).
- [ ] Clearly distinguish between strategy `-s ours` and strategy option `-X ours`.
- [ ] Understand whitespace conflict mitigation flags (`-X ignore-all-space`).
- [ ] Master conflict categories: content conflicts, modify/delete conflicts, add/add conflicts, binary conflicts.
- [ ] Understand how `git rerere` (Reuse Recorded Resolution) records and replays conflict resolutions.

---

### Commands & Practice
- [ ] Inspect common ancestor commit: `git merge-base main feature`.
- [ ] Perform a merge favoring local changes during conflicts: `git merge -X ours feature`.
- [ ] Inspect conflict status with `git status` and `git diff --check`.
- [ ] Resolve a binary file conflict using `git checkout --ours <file>` and `git checkout --theirs <file>`.
- [ ] Configure and launch visual merge tool with `git mergetool`.
- [ ] Enable `git rerere` with `git config --global rerere.enabled true`.
- [ ] Test `git rerere` by reproducing and auto-resolving a repeated conflict during branch rebase.
- [ ] Use `git rerere forget <file>` to unlearn an erroneous resolution.

---

### Interview Verification Questions
- [ ] Explain how Git's 3-way merge uses base, ours, and theirs to detect conflicts.
- [ ] What is the default merge strategy in modern Git, and how does it improve over `recursive`?
- [ ] How do you resolve a merge conflict in a binary image or database file where diff markers cannot be embedded?
- [ ] When would you use `git rerere` in an enterprise engineering workflow?
