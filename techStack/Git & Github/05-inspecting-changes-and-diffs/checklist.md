# Checklist — 05: Inspecting Changes & Diffs

Track your mastery of diff inspection and log navigation.

---

### Concepts
- [ ] Understand why `git status` is not enough to verify changes before committing.
- [ ] Understand how to read unified diff format (`---`, `+++`, `@@`, `+`, `-`).
- [ ] Explain why `git diff` produces empty output after running `git add`.
- [ ] Distinguish between `git diff` and `git diff --staged`.
- [ ] Understand how the terminal pager works and know how to exit with `q`.

---

### Commands & Practice
- [ ] Modify a file and run `git diff` to view line additions/deletions.
- [ ] Stage the file with `git add` and verify that `git diff` returns empty.
- [ ] Run `git diff --staged` to verify the staged changes against `HEAD`.
- [ ] Run `git log --oneline` and `git log --pretty=oneline`.
- [ ] Run `git show <commit-hash>` to view an individual commit's diff.

---

### Interview Verification Questions
- [ ] What is the exact difference between `git diff` and `git diff --staged`?
- [ ] What is the alias for `git diff --staged`?
- [ ] How would you view the differences between two branches without switching branches?
