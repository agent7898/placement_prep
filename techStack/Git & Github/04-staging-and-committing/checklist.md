# Checklist — 04: Staging & Committing

Track your mastery of staging, committing, and commit metadata.

---

### Concepts
- [ ] Understand how `git add` moves files from the working directory to the index.
- [ ] Understand what a commit object stores (tree pointer, parent pointer, author, date, message).
- [ ] Understand why commit messages are mandatory.
- [ ] Understand what SHA-1 checksums are and how they guarantee repository immutability.
- [ ] Understand why Git abbreviates 40-character commit hashes to 7 characters.
- [ ] Understand the limitations of `git commit -am` (only works on tracked files).

---

### Commands & Practice
- [ ] Stage a file with `git add <file>`.
- [ ] Commit with `git commit -m "feat: description"`.
- [ ] View commits using `git log`.
- [ ] Make modifications to a tracked file and commit using `git commit -am "..."`.
- [ ] Unstage a staged file using `git restore --staged <file>`.

---

### Interview Verification Questions
- [ ] What is the difference between `git add .` and `git commit -a`?
- [ ] Can someone secretly alter a committed file in Git without altering the commit hash? Why or why not?
- [ ] What does `(HEAD -> main)` signify in the output of `git log`?
