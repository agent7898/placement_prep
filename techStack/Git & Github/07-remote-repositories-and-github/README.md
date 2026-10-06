# 07 — Remote Repositories, GitHub & SSH Authentication

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Local vs Remote Repositories

While Git operates entirely locally, collaborating with a team requires a shared remote repository:

- **Local Repository:** Exists exclusively on your machine inside `.git`.
- **Remote Repository:** Hosted on a network server or cloud platform (GitHub, GitLab, Bitbucket) accessible by your team.

### Popular Remote Hosting Platforms

| Platform | Best Known For |
|---|---|
| **GitHub** | The de facto standard for open-source projects, developer portfolios, and developer communities. Owned by Microsoft. |
| **GitLab** | Known for powerful built-in CI/CD pipelines and self-hosted enterprise installations. |
| **Bitbucket** | Owned by Atlassian; deeply integrates with Jira, Confluence, and Trello. |

---

## 2. HTTPS vs SSH Authentication

When communicating with GitHub, you can use **HTTPS** or **SSH**:

| Feature | HTTPS | SSH (Recommended) |
|---|---|---|
| **URL Format** | `https://github.com/user/repo.git` | `git@github.com:user/repo.git` |
| **Authentication** | Requires Personal Access Token (PAT) | Uses asymmetric public/private cryptographic key pair |
| **Convenience** | Often prompts or needs credential helpers | Authenticates seamlessly in the background without repeated password prompts |
| **Firewalls** | Rarely blocked (port 443) | Can occasionally be blocked on restrictive corporate networks (port 22) |

### Setting Up SSH Keys Step-by-Step

```bash
# 1. Generate a new SSH key pair
ssh-keygen -t ed25519 -C "your.email@example.com"
# (Or RSA: ssh-keygen -o -t rsa -b 4096 -C "your.email@example.com")

# Press Enter to accept default file location (~/.ssh/id_ed25519 or id_rsa)
# Press Enter twice for empty passphrase (or specify one)

# 2. View and copy your PUBLIC key (ends with .pub)
cat ~/.ssh/id_ed25519.pub
# Or: cat ~/.ssh/id_rsa.pub

# 3. Add to GitHub:
# Go to GitHub -> Settings -> SSH and GPG Keys -> New SSH Key
# Paste the copied public key and save.

# 4. Verify connection:
ssh -T git@github.com
# Expected output: "Hi username! You've successfully authenticated..."
```

> [!CAUTION]
> **Never share your private key (`id_rsa` or `id_ed25519`)!** Only paste the **public key** (`.pub`) into GitHub.

---

## 3. Connecting a Local Repo to GitHub

When creating a new repository on GitHub:

```bash
# 1. Link your local repo to the remote GitHub repository
git remote add origin git@github.com:username/my-repo.git

# 2. Verify configured remotes
git remote -v
# origin  git@github.com:username/my-repo.git (fetch)
# origin  git@github.com:username/my-repo.git (push)

# 3. Push your local branch and set upstream tracking (-u)
git push -u origin main
```

### What does `-u` (`--set-upstream`) do?
The `-u` flag links your local `main` branch to the remote-tracking branch `origin/main`. For all future pushes and pulls on this branch, you only need to type:
```bash
git push
git pull
```

---

## 4. Cloning Existing Repositories

To copy an entire repository (code + full commit history + branches) from GitHub to your machine:

```bash
git clone git@github.com:username/repository-name.git
# Or via HTTPS:
git clone https://github.com/username/repository-name.git
```

### Why Direct Push Fails on Open Source Repos
In the reference video, the author clones Microsoft's VS Code repository, makes a local change, and attempts `git push origin main`. Git immediately rejects it:
```text
Permission to microsoft/vscode.git denied to username.
fatal: unable to access ... The requested URL returned error: 403
```

**Why?** You do not have push permissions to other people's repositories.  
**The Solution (Fork & Pull Request):**
1. Click **Fork** on GitHub to create a personal copy in your account.
2. Clone your fork locally.
3. Make changes on a feature branch and push to your fork.
4. Open a **Pull Request (PR)** on GitHub proposing your changes to the original maintainers.

---

## 5. Command Reference Table

| Command | Purpose |
|---|---|
| `git clone <url>` | Clone a remote repository to local machine |
| `git remote add <name> <url>` | Add a new remote alias (conventionally `origin`) |
| `git remote -v` | List all configured remotes with URLs |
| `git remote set-url <name> <url>` | Change URL of an existing remote |
| `git push -u origin <branch>` | Push branch and set upstream tracking |
| `git push` | Push committed changes to tracked upstream branch |
| `ssh -T git@github.com` | Test SSH authentication with GitHub |
