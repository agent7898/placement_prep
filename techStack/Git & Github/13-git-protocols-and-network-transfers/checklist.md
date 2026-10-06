# Checklist — 13: Git Protocols & Network Transfers

Track your mastery of network protocols, wire protocol mechanics, cryptographic authentication, credential management, and optimized repository clones.

---

### Concepts
- [ ] Differentiate between Git's 4 transfer protocols: Local (`file://`), Smart HTTPS, SSH, and Git (`git://`).
- [ ] Understand why "Dumb HTTP" was replaced by "Smart HTTP" (`git-upload-pack` and `git-receive-pack`).
- [ ] Understand why Git Wire Protocol v2 dramatically improves fetch efficiency on large repos.
- [ ] Understand Ed25519 asymmetric cryptography vs RSA for SSH authentication.
- [ ] Understand GitHub token-based authentication (Classic PAT vs Fine-Grained PAT).
- [ ] Compare Git Credential Manager, cache helpers, and store helpers.
- [ ] Master modern clone strategies: Shallow (`--depth`), Blobless (`--filter=blob:none`), Treeless, and Sparse Checkout.

---

### Commands & Practice
- [ ] Generate an Ed25519 SSH key pair: `ssh-keygen -t ed25519 -C "..."`.
- [ ] Register SSH key with agent: `ssh-add ~/.ssh/id_ed25519`.
- [ ] Verify SSH connection to GitHub: `ssh -T git@github.com`.
- [ ] Check active wire protocol version: `git config protocol.version`.
- [ ] Configure Git Credential cache timeout: `git config --global credential.helper 'cache --timeout=3600'`.
- [ ] Perform a shallow clone: `git clone --depth 1 <url>`.
- [ ] Convert shallow clone to full clone: `git fetch --unshallow`.
- [ ] Perform a blobless clone on a large repository: `git clone --filter=blob:none <url>`.
- [ ] Configure sparse checkout for a single monorepo directory.

---

### Interview Verification Questions
- [ ] How does Git Wire Protocol v2 eliminate ref advertisement overhead?
- [ ] Why did GitHub deprecate account password authentication over HTTPS?
- [ ] What is the difference between a shallow clone and a sparse checkout?
- [ ] How does `git-upload-pack` negotiate minimal packfile transfer during a fetch?
