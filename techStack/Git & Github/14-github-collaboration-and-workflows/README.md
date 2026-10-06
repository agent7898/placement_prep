# 14 — GitHub Collaboration Workflows & Team Governance

> **Interview Focus:** Shared Repo vs Fork & PR Model, Branching Strategies (GitHub Flow vs Gitflow vs Trunk-Based Development), Upstream Synchronization, PR Merge Methods, Branch Protection Rules, and `CODEOWNERS`.

---

## 1. Collaboration Models: Shared Repo vs Fork & Pull Request

In professional software development, teams collaborate using one of two primary architectural models:

```text
1. Shared Repository Model (Internal Enterprise Teams)
   Team Dev A ─── push feature branch ───┐
                                          ├──> [ Company Central Repo ] ──> PR into main
   Team Dev B ─── push feature branch ───┘

2. Fork & Pull Request Model (Open Source & Multi-Org Collaborations)
   [ Upstream Repo (Original) ]
              │ (Fork)
              ▼
   [ Contributor's Origin Fork ] <── push ── Contributor Local Machine
              │
              └────── Pull Request (Cross-Repository) ──────> [ Upstream Repo ]
```

### Model Comparison Matrix

| Feature | Shared Repository Model | Fork & Pull Request Model |
|---|---|---|
| **Write Access** | Developers have direct push permissions to non-protected branches. | Contributors have zero write access to the upstream repo. |
| **Branch Location** | Feature branches live inside the main repository. | Feature branches live on contributor's personal forked copy. |
| **Primary Use** | Internal company repositories, trusted development teams. | Open-source public projects, contractor/vendor engagements. |
| **Branch Clutter** | Repository contains branches from all developers. | Main repo remains completely clean of work-in-progress branches. |

---

## 2. Industry Branching Strategies

Choosing a branching model directly impacts team velocity, release frequency, and deployment stability:

### 1. GitHub Flow (Lightweight & Continuous Deployment)
- **Philosophy:** Anything in the `main` branch is always deployable to production.
- **Workflow:**
  1. Branch off `main` with a descriptive name (`feature/oauth-login` or `fix/nav-bar`).
  2. Commit changes locally and push to remote.
  3. Open a Pull Request for discussion and automated CI testing.
  4. Address review comments.
  5. Merge into `main` and deploy immediately.
- **Best For:** Web apps, SaaS products, startups, teams practicing Continuous Delivery.

### 2. Gitflow (Strict Scheduled Releases)
- **Philosophy:** Multiple long-lived parallel branches for different release stages.
- **Key Branches:**
  - `main`: Reflects official production releases (tagged with release versions).
  - `develop`: Nightly integration branch for latest completed features.
  - `feature/*`: Branched from `develop`, merged back to `develop`.
  - `release/*`: Forked from `develop` for final polish, merged to both `main` and `develop`.
  - `hotfix/*`: Forked directly from `main` to patch critical production bugs, merged to both `main` and `develop`.
- **Best For:** Enterprise software with scheduled version releases (mobile apps, desktop software, medical/banking systems).

### 3. Trunk-Based Development (High Velocity & Modern Enterprise Standard)
- **Philosophy:** All developers merge small, frequent commits into a single shared branch ("trunk" or `main`) at least once a day.
- **Techniques:** Short-lived branches (< 1 day lifetime), Feature Flags / Feature Toggles to hide uncompleted features in production.
- **Best For:** High-performing engineering teams at Google, Meta, Netflix; prevents large, painful merge conflicts ("merge hell").

---

## 3. The Fork & Upstream Synchronization Workflow

When working with forks, your local clone has two distinct remotes:
- **`origin`:** Your personal fork on GitHub.
- **`upstream`:** The original primary repository you want to contribute to.

```text
[ Upstream (Original Repo) ]
        ▲
        │ 3. Fetch updates / Open PR
        │
[ Local Workspace ] ────── 1. Push changes ─────> [ Origin (Your Fork) ]
```

### Complete Upstream Sync Command Flow:

```bash
# 1. Clone your fork
git clone git@github.com:my-username/open-source-project.git
cd open-source-project

# 2. Add upstream remote pointer
git remote add upstream git@github.com:original-owner/open-source-project.git

# 3. Verify remotes
git remote -v
# origin    git@github.com:my-username/open-source-project.git (fetch & push)
# upstream  git@github.com:original-owner/open-source-project.git (fetch & push)

# 4. Fetch the latest changes from upstream
git fetch upstream

# 5. Sync your local main with upstream main
git switch main
git merge upstream/main
# (Or rebase: git rebase upstream/main)

# 6. Push synchronized main to your personal fork
git push origin main
```

---

## 4. The 3 GitHub Pull Request Merge Methods

When merging a PR on GitHub, repository maintainers can choose between 3 distinct merge actions:

| Merge Method | Git Command Equivalent | Resulting History | Best Use Case |
|---|---|---|---|
| **Create a Merge Commit** | `git merge --no-ff feature` | Preserves all individual feature commits plus an explicit 2-parent merge commit. | Complex feature branches where full incremental commit history and author timeline matter. |
| **Squash and Merge** | `git merge --squash feature` + `git commit` | Collapses all 15 WIP commits into a single clean commit on `main`. | Cleaning up noisy feature commits (`wip`, `fixed typo`, `formatting`) into one atomic change. |
| **Rebase and Merge** | `git rebase main feature` + fast-forward | Replays feature commits onto the tip of `main` without a merge commit. | Maintaining a strict, single-track linear history while preserving individual commit separation. |

```text
1. Merge Commit:
   main ─── C1 ────── C4 (Merge Commit)
             \       /
              C2 ── C3 (Feature)

2. Squash and Merge:
   main ─── C1 ────── C_squashed (Single atomic commit on main)

3. Rebase and Merge:
   main ─── C1 ────── C2' ───── C3' (Linear commits on main)
```

---

## 5. Enterprise Governance: Branch Protection & Rulesets

Production branches (`main`, `production`) must never accept accidental direct pushes or broken code.

### Standard Branch Protection Configuration:
1. **Require a pull request before merging:**
   - Disables direct `git push origin main`.
   - Requires minimum $N$ approvals (e.g., 1 or 2 senior engineers).
   - Dismisses stale approvals when new commits are pushed.
2. **Require status checks to pass before merging:**
   - CI build, linter, and unit test suites must return green (`0` exit code).
   - "Require branches to be up to date before merging" prevents merging code tested against stale codebases.
3. **Require linear history:**
   - Rejects merge commits; forces squash-and-merge or rebase workflows.
4. **Require signed commits:**
   - Enforces GPG/SSH cryptographically signed commits to prevent identity spoofing.
5. **Do not allow bypassing the above settings:**
   - Ensures administrators and repository owners must also follow the review gates.

---

## 6. Automating Ownership with `CODEOWNERS`

GitHub supports automatic reviewer assignment via a `.github/CODEOWNERS` file:

```text
# .github/CODEOWNERS

# Default fallback owner for the entire repo
* @lead-maintainer

# Frontend files must be reviewed by frontend team
/frontend/ @my-org/frontend-core

# Security critical auth files require security lead review
/backend/src/auth/ @alice-security @bob-security

# Build & CI configurations
/.github/workflows/ @devops-team
```

When a PR touches any file matching the pattern, GitHub automatically tags the specified code owners as required reviewers!

---

## 7. Accelerating Workflows with GitHub CLI (`gh`)

```bash
# Clone repository
gh repo clone owner/repo

# Create a pull request interactively
gh pr create --title "feat: implement JWT auth" --body "Resolves #102"

# View status of PRs
gh pr status

# Check out a colleague's PR locally for testing
gh pr checkout 42

# Approve and merge a PR
gh pr review --approve -b "LGTM!"
gh pr merge 42 --squash --delete-branch
```

---

## 8. Placement Interview Verification Questions

1. **What is the difference between GitHub Flow and Gitflow?**
   - GitHub Flow is a lightweight, continuous delivery branching model centered around `main` and short-lived feature branches. Gitflow is an enterprise branching model with multiple long-lived branches (`main`, `develop`, `release/*`, `hotfix/*`) suited for scheduled periodic releases.
2. **Why do open-source projects require contributors to fork rather than cloning directly?**
   - Direct push requires write credentials to the repository. Forking isolates contributor experiments in their own repo, allowing maintainers to review code via Pull Requests before any changes touch upstream.
3. **What happens during a "Squash and Merge" on GitHub?**
   - All commits on the feature branch are combined into a single new commit that is applied to the base branch, discarding micro-commit noise and keeping `main` history clean.
4. **How do you keep your local fork in sync with changes made to the upstream repository?**
   - Add upstream remote (`git remote add upstream <url>`), fetch updates (`git fetch upstream`), merge upstream main into local main (`git merge upstream/main`), and push to your fork (`git push origin main`).
5. **What is the purpose of a `CODEOWNERS` file?**
   - It defines individuals or teams responsible for specific paths in a repository, automatically requesting their review whenever a PR modifies those files.
