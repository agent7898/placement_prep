# Checklist — 06: Untracking & Removing Files

Track your skills in safely removing and ignoring files.

---

### Concepts
- [ ] Understand the difference between deleting a file locally and untracking it in Git.
- [ ] Understand what `git rm --cached` does to the Git index and working tree.
- [ ] Understand why adding an already-tracked file to `.gitignore` does not immediately stop Git from tracking it.
- [ ] Know standard `.gitignore` glob patterns (`*`, `/`, `!`).

---

### Commands & Practice
- [ ] Create a dummy file `secrets.txt` and stage it with `git add secrets.txt`.
- [ ] Untrack it with `git rm --cached secrets.txt` and verify it remains on disk.
- [ ] Add `secrets.txt` to `.gitignore`.
- [ ] Verify that `git status` no longer lists `secrets.txt` as untracked.

---

### Interview Verification Questions
- [ ] What is the difference between `git rm file.txt` and `git rm --cached file.txt`?
- [ ] If you accidentally committed an API secret in your last local commit (not yet pushed), how can you remove it cleanly?
- [ ] Why must you run `git rm --cached` before `.gitignore` takes effect for already tracked files?
