# Checklist — 07: Remote Repositories & GitHub

Track your mastery of remote syncing and GitHub authentication.

---

### Concepts
- [ ] Understand what a remote repository is and how it differs from a local repository.
- [ ] Differentiate between GitHub, GitLab, and Bitbucket.
- [ ] Understand the differences between HTTPS and SSH authentication.
- [ ] Explain how public/private SSH key pairs work for GitHub authentication.
- [ ] Understand what the name `origin` represents in Git.
- [ ] Explain why direct push to open-source repositories fails and how Fork + PR solves it.

---

### Commands & Practice
- [ ] Generate an SSH key pair using `ssh-keygen`.
- [ ] Add the public key (`.pub`) to your GitHub account settings.
- [ ] Verify connection using `ssh -T git@github.com`.
- [ ] Create a repository on GitHub.
- [ ] Link local repo using `git remote add origin <url>`.
- [ ] Verify with `git remote -v`.
- [ ] Push with upstream tracking using `git push -u origin main`.
- [ ] Clone a public repository using `git clone <url>`.

---

### Interview Verification Questions
- [ ] What is the purpose of the `-u` flag in `git push -u origin main`?
- [ ] What is the difference between a Fork and a Clone?
- [ ] If a company repository rejects your push with a 403 error, what are the possible causes?
