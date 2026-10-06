# 13 — Git Network Protocols, GitHub Wire Protocols & Transfers

> **Interview Focus:** Git Transport Protocols (Local, HTTP/S, SSH, Git), Smart vs Dumb HTTP, Git Wire Protocol v2, SSH Key Cryptography (Ed25519 vs RSA), Git Credential Manager, and Optimized Clones (Shallow & Blobless).

---

## 1. The 4 Git Transport Protocols

Git can transfer data across 4 primary network protocols:

```text
┌─────────────────────────────────────────────────────────────┐
│                 Git Network Transport Protocols             │
├───────────────┬────────────────┬──────────────┬─────────────┤
│     Local     │      HTTP/S    │     SSH      │     Git     │
│   (file://)   │ (Smart HTTP/S) │ (ssh://, git@)│  (git://)   │
└───────────────┴────────────────┴──────────────┴─────────────┘
```

### Protocol Comparison Matrix

| Protocol | URL Syntax | Port | Encryption | Authentication | Best Use Case |
|---|---|---|---|---|---|
| **Local (File)** | `/path/to/repo.git` or `file:///path/to/repo.git` | N/A (Disk/NFS) | None | Filesystem permissions | Local shared mounts, offline backups |
| **Smart HTTPS** | `https://github.com/user/repo.git` | `443` | SSL / TLS | PAT (Personal Access Token) / OAuth | Strict enterprise firewalls; read-only public clones |
| **SSH** | `git@github.com:user/repo.git` or `ssh://git@host/repo.git` | `22` | SSH tunnel | Asymmetric Key Pair (Ed25519, RSA) | Standard developer daily workflow; automated CI/CD deploy keys |
| **Git Protocol** | `git://github.com/user/repo.git` | `9418` | ❌ None | ❌ None (No auth) | Legacy high-speed public read-only mirrors (mostly deprecated) |

---

## 2. Git Smart HTTP vs Dumb HTTP

In Git's early days, HTTP transfer was **"Dumb"**:
- The client directly requested raw object files (`.git/objects/xx/yy`) via standard HTTP GET requests.
- Slow, inefficient, and required public servers to expose raw `.git` directories.

**Smart HTTP (Modern Standard):**
- Uses specialized server-side CGI binaries: `git-upload-pack` (for fetch/clone) and `git-receive-pack` (for push).
- The client negotiates with the server to find the minimal set of packfiles needed to transfer only missing objects over standard HTTPS (Port 443).
- Supports bidirectional streaming, compression, and fine-grained authentication.

---

## 3. Git Wire Protocol: Protocol v1 vs Protocol v2

When you run `git fetch` or `git push`, the client and server communicate via the **Git Wire Protocol**.

### The Problem with Protocol v1
In Protocol v1, whenever a client connected to a repository, the server had to advertise **all existing references** (every branch, tag, and pull request ref).
- For massive repositories with tens of thousands of tags and branches (e.g., Chromium, Linux kernel), a simple `git fetch` sent megabytes of reference advertisements before transferring a single commit!

### Protocol v2 (Modern Default)
Introduced in Git 2.18 and fully adopted by GitHub in late 2018.

**Key Innovations:**
1. **Command Multiplexing:** Clients explicitly send requests with capabilities before receiving refs.
2. **Filtered Ref Advertisements:** The client can ask for only the specific branch it cares about (e.g., `git fetch origin main` fetches only `main`'s ref, skipping 50,000 other tags).
3. **Extensibility:** Allows adding new server features without breaking backward compatibility.

```bash
# Verify or force Protocol v2
git config --global protocol.version 2
```

---

## 4. SSH Authentication: Ed25519 vs RSA

When authenticating via SSH, Git uses asymmetric cryptography:

```text
Developer Machine                      GitHub Cloud Server
┌───────────────────────────┐         ┌───────────────────────────┐
│ Private Key (id_ed25519)  │ <~~~~~> │ Public Key (id_ed25519.pub│
│ (Never leaves your disk!) │         │ (Uploaded to your account)│
└───────────────────────────┘         └───────────────────────────┘
```

### Ed25519 vs RSA 4096:

| Parameter | Ed25519 (Recommended) | RSA 4096 |
|---|---|---|
| **Cryptographic Scheme** | Twisted Edwards Curve (EdDSA) | Prime factorization (RSA) |
| **Key Length** | 256 bits (short string: ~68 characters) | 4096 bits (large file) |
| **Security Level** | ~128-bit security margin, immune to timing attacks | Comparable, but susceptible to legacy side-channels |
| **Performance** | Extremely fast key generation and signing | Slower verification |

### Key Generation Command:
```bash
# Modern standard (Ed25519)
ssh-keygen -t ed25519 -C "developer@company.com"

# Start ssh-agent and add key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Test GitHub authentication
ssh -T git@github.com
```

---

## 5. GitHub Personal Access Tokens (PAT) & Credential Helpers

GitHub deprecated account password authentication for Git operations over HTTPS in August 2021. You must use **Personal Access Tokens (PAT)** or the **Git Credential Manager (GCM)**.

### Token Types:
1. **Fine-Grained Personal Access Tokens:** Scoped to specific repositories with granular permissions (e.g., Read-only Contents on Repo X). Recommended for production.
2. **Tokens (Classic):** Blanket permissions across all user repositories.

### Credential Helpers:
To avoid entering tokens repeatedly:

```bash
# 1. Git Credential Manager (Cross-platform GUI / OAuth popups)
git config --global credential.helper manager

# 2. In-Memory Cache (Stores token in RAM for 1 hour)
git config --global credential.helper 'cache --timeout=3600'

# 3. Plaintext Disk Storage (Not recommended on shared machines)
git config --global credential.helper store
```

### GitHub CLI (`gh`) Authentication:
The fastest way to authenticate on Linux or CI environments:
```bash
gh auth login
# Prompts for GitHub.com -> SSH/HTTPS -> Web browser or token authentication
```

---

## 6. High-Performance Clone Strategies for Massive Repositories

When cloning enterprise codebases with gigabytes of historical blobs, standard `git clone` can take hours. Modern Git provides targeted clone optimizations:

### 1. Shallow Clone (`--depth`)
Clones only the most recent $N$ commits, omitting ancient history:
```bash
git clone --depth 1 https://github.com/torvalds/linux.git

# Later fetch full history if needed:
git fetch --unshallow
```

### 2. Blobless Clone (`--filter=blob:none`)
Clones the entire commit tree and history, but **downloads file contents (blobs) on demand** only when you check out or view them:
```bash
git clone --filter=blob:none <repo-url>
```
*Best for:* Working on daily feature branches in massive codebases while keeping full commit history accessible locally.

### 3. Treeless Clone (`--filter=tree:0`)
Downloads only commit objects. Downloads trees and blobs on demand when switching revisions:
```bash
git clone --filter=tree:0 <repo-url>
```
*Best for:* CI/CD build agents that only need to checkout a single commit and run a build.

### 4. Sparse Checkout
Check out only a specific subfolder of a monorepo while ignoring the rest of the workspace:
```bash
git clone --filter=blob:none --no-checkout <repo-url> my-project
cd my-project
git sparse-checkout init --cone
git sparse-checkout set services/auth-service
git checkout main
```

---

## 7. Placement Interview Verification Questions

1. **Why is the Git protocol (`git://`) rarely used in modern corporate environments?**
   - It provides zero authentication and zero encryption (everything is plaintext). Anyone on the network can snoop or tamper with transfers.
2. **What major problem does Git Wire Protocol v2 solve over Protocol v1?**
   - Protocol v1 required advertising all refs unconditionally, causing severe performance degradation on large repos. Protocol v2 allows filtered ref requests and command multiplexing.
3. **Why should developers choose Ed25519 over RSA keys for SSH authentication?**
   - Ed25519 provides stronger mathematical security with much shorter key sizes, resistance to side-channel timing attacks, and significantly faster signature verification.
4. **What is the difference between a shallow clone and a blobless clone?**
   - A shallow clone truncates commit history by commit depth (`--depth 1`). A blobless clone (`--filter=blob:none`) preserves the entire commit history graph, but defers downloading file contents (blobs) until files are actually opened or checked out.
5. **How does Git Credential Manager store credentials securely?**
   - It integrates directly with native operating system keyrings (Windows Credential Manager, macOS Keychain, Linux Secret Service / libsecret).
