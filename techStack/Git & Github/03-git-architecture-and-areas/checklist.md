# Checklist — 03: Git Architecture & Areas

Track your understanding of Git's three local areas.

---

### Core Concepts & Mental Model
- [ ] Understand the difference between the Working Directory and the Git Repository.
- [ ] Understand what the `.git` directory contains (`objects`, `refs`, `HEAD`, `index`, `config`).
- [ ] Explain what the Staging Area (Index) is and why Git includes it between the working directory and commits.
- [ ] Understand what happens when `.git` is deleted.
- [ ] Interpret the output of `git status` across all states (Untracked, Modified, Staged, Clean).

---

### Practical Exercises
- [ ] Create a new directory and initialize it with `git init -b main`.
- [ ] Check `.git` contents using `ls -la .git`.
- [ ] Create a file `example.txt` and check `git status`.
- [ ] Verify that Git reports the file under `Untracked files`.

---

### Interview Verification Questions
- [ ] Why doesn't Git automatically commit files as soon as you save them in your text editor?
- [ ] What is stored inside `.git/index`?
- [ ] If you delete the `.git` folder, what happens to your source files?
