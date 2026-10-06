# Checklist — 08: Git Tags & Releases

Track your mastery of tagging and release management.

---

### Concepts
- [ ] Understand why tags are used in software versioning.
- [ ] Differentiate between a moving Branch pointer and a static Tag reference.
- [ ] Differentiate between Annotated tags and Lightweight tags.
- [ ] Understand why `git push origin main` does not upload tags to GitHub.
- [ ] Understand how to safely inspect code at a past release tag without creating detached HEAD issues.

---

### Commands & Practice
- [ ] Create an annotated tag using `git tag -a v1.0 -m "..."`.
- [ ] List all tags with `git tag`.
- [ ] Inspect tag metadata and commit details with `git show v1.0`.
- [ ] Push a tag to remote using `git push origin v1.0`.
- [ ] Push multiple tags with `git push origin --tags`.
- [ ] Delete a local tag with `git tag -d <tag>`.

---

### Interview Verification Questions
- [ ] What is the structural difference between a branch and a tag in Git?
- [ ] When should you use an annotated tag versus a lightweight tag?
- [ ] How does semantic versioning (`MAJOR.MINOR.PATCH`) typically map to Git tags?
