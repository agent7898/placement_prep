# 15 — Advanced Git Tooling, Recovery & Internals

> **Interview Focus:** Advanced `git stash`, `git reflog` Disaster Recovery, `git cherry-pick`, Automated Bug Hunting with `git bisect`, Concurrent Development with `git worktree`, Client/Server Git Hooks, History Scrubbing, and Git Database Plumbing (`git gc`, Packfiles).

---

## 1. Deep Dive: `git stash` Mechanics

The stash saves your dirty working directory and index on a stack of unfinished changes that you can reapply anytime.

```text
Working Tree (Dirty) ─── git stash push ───> Stash Stack:
                                             stash@{0}: WIP on main
                                             stash@{1}: WIP on feature
```

### Advanced Stash Commands

```bash
# 1. Stash with a descriptive name
git stash push -m "WIP: Stripe payment gateway integration"

# 2. Stash including untracked new files (-u)
git stash -u

# 3. Stash everything, including .gitignored build artifacts (-a)
git stash -a

# 4. Keep staged files in index while stashing unstaged edits
git stash --keep-index

# 5. List stashes with diff summaries
git stash list

# 6. Inspect specific stash diff without applying
git stash show -p stash@{0}

# 7. Apply vs Pop:
git stash apply stash@{0}   # Re-applies changes, retains entry in stash stack
git stash pop stash@{0}     # Re-applies changes and drops entry from stack

# 8. Convert stash into a brand new branch (resurrects clean context)
git stash branch feature-stripe-fix stash@{0}
```

---

## 2. Emergency Recovery: The `git reflog` Safety Net

> [!IMPORTANT]
> Git almost never deletes committed data from disk immediately. Even after `git reset --hard` or deleting a branch, commits remain in `.git/objects/` for at least 30 to 90 days.

### What is Reflog?
The **Reference Log** (`reflog`) records every single change of `HEAD` and branch references on your local machine (commits, checkouts, merges, resets, rebases).

### Scenario 1: Accidental `git reset --hard`
You ran `git reset --hard HEAD~3` and erased your last 3 commits.

```bash
# 1. View reflog history
git reflog

# Output:
# 1a2b3c4 HEAD@{0}: reset: moving to HEAD~3
# 9f8e7d6 HEAD@{1}: commit: feat: complete order checkout
# 8c7b6a5 HEAD@{2}: commit: feat: cart validation
```

```bash
# 2. Restore repository directly to HEAD@{1} (prior to reset)
git reset --hard HEAD@{1}
# All lost commits are instantly restored!
```

### Scenario 2: Recovering an Accidentally Deleted Branch
```bash
git branch -D feature-billing
# Reflog shows: "7a8b9c0 HEAD@{4}: checkout: moving from feature-billing to main"

# Recreate branch at the lost commit hash
git branch feature-billing 7a8b9c0
```

---

## 3. Precision Integration: `git cherry-pick`

Cherry-picking allows you to choose a commit from one branch and apply it to another without merging the entire branch.

```text
feature:  ─── C1 ─── C2 (Hotfix) ─── C3
                      │ (cherry-pick C2)
                      ▼
main:     ─── M1 ──── C2'
```

### Cherry-Pick Commands
```bash
# Apply a single commit to current branch
git cherry-pick <commit-hash>

# Apply commit without committing immediately (stages changes into index)
git cherry-pick -n <commit-hash>

# Cherry-pick a range of commits (from A to B, exclusive of A)
git cherry-pick A..B

# Cherry-pick a range inclusive of A
git cherry-pick A^..B

# Resolving conflicts during cherry-pick
git cherry-pick --continue
git cherry-pick --abort
```

---

## 4. Automated Debugging: `git bisect`

When a bug is introduced somewhere in the last 200 commits, manual testing takes hours. `git bisect` uses **binary search ($O(\log N)$)** to pinpoint the exact commit that introduced the regression in ~7 steps.

```text
Commits: C1 (Good) ─── C2 ─── C3 ─── C4 (Bug introduced) ─── C5 ─── C6 (Bad)
                Bisect tests C3 -> Good
                Bisect tests C4 -> Bad! Found culprit in 2 steps!
```

### Manual Bisect Flow:
```bash
# 1. Start bisect
git bisect start

# 2. Mark current broken commit as bad
git bisect bad

# 3. Mark known working commit as good
git bisect good v1.0.0

# 4. Git checks out midpoint commit; test your code, then inform Git:
git bisect good   # If test passes
# OR
git bisect bad    # If test fails

# 5. Repeat until Git prints: "<hash> is the first bad commit"
# 6. Reset workspace back to original state:
git bisect reset
```

### Fully Automated Bisect with Test Scripts:
```bash
git bisect start HEAD v1.0.0
# Run automated test script; Git automatically searches and reports the offending commit!
git bisect run npm test
```

---

## 5. Parallel Workspaces: `git worktree`

In standard Git, switching branches requires stashing unfinished work or committing half-baked code. **`git worktree`** allows you to check out **multiple branches simultaneously into separate directories** sharing the same `.git` database.

```text
/my-project/             (Active branch: feature-dashboard)
/my-project-hotfix/      (Active branch: hotfix-login, sharing same .git!)
```

```bash
# 1. Create a new worktree directory on a new branch
git worktree add ../hotfix-worktree -b hotfix-urgent main

# 2. Open ../hotfix-worktree, fix bug, commit, and push!

# 3. View all active worktrees
git worktree list

# 4. Remove worktree when finished
git worktree remove ../hotfix-worktree
```

---

## 6. Automating Quality Gates: Git Hooks

Git hooks are custom shell scripts triggered at key execution points.

### Hook Locations:
Located in `.git/hooks/` (e.g., `pre-commit.sample`). Remove `.sample` to activate.

```text
┌─────────────────┐       ┌──────────────────────┐       ┌────────────────┐
│   git commit    │ ───>  │ pre-commit hook      │ ───>  │ commit-msg     │
│                 │       │ (linter, unit tests) │       │ (enforce format)│
└─────────────────┘       └──────────────────────┘       └────────────────┘
```

### Essential Client-Side Hooks:
1. **`pre-commit`:** Runs before commit object is created. If script exits non-zero (`exit 1`), commit is aborted. Ideal for linting (`eslint`), formatting (`prettier`), and secret leak scanning.
2. **`commit-msg`:** Validates commit message format (e.g., enforcing Conventional Commits like `feat: ...`, `fix: ...`).
3. **`pre-push`:** Runs before pushing to remote; ideal for running full test suites.

### Enterprise Hook Management: Husky & lint-staged
Because `.git/hooks` is not tracked by Git, teams use tools like **Husky** (JavaScript) or **pre-commit** (Python) to commit hooks to repository source control.

---

## 7. History Scrubbing & Removing Leaked Secrets

If an API key or database password was committed in past history, `git rm` only deletes it from the latest commit. The secret remains accessible in historical commits!

### The Modern Standard: `git-filter-repo`
`git filter-branch` is deprecated and slow. Modern engineers use `git-filter-repo`:

```bash
# 1. Install tool
pip install git-filter-repo

# 2. Scrub specific sensitive file completely from ALL historical commits
git filter-repo --invert-paths --path secrets.env

# 3. Scrub matching password strings across all files and history
git filter-repo --replace-text expressions.txt

# 4. Force push rewritten history to GitHub
git push origin --force --all
```
> [!CAUTION]
> Always rotate leaked credentials immediately! Rewriting Git history does not invalidate secrets already scraped by bots.

---

## 8. Git Internals: Database Plumbing & Maintenance

### How Git Compresses Objects: Loose Objects vs Packfiles
- When you commit, Git writes individual compressed files into `.git/objects/xx/` (**Loose Objects**).
- Over time, hundreds of loose objects degrade disk performance.
- Git automatically runs **Garbage Collection (`git gc`)** to compress loose objects into delta-compressed **Packfiles (`.pack`)** with an index (`.idx`).

```bash
# Run manual optimization and prune unreachable commits older than now
git gc --prune=now

# Check repository database integrity
git fsck --full

# Inspect largest objects inside packfiles
git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -5
```

### Submodules vs Subtrees

| Feature | `git submodule` | `git subtree` |
|---|---|---|
| **Mechanism** | Points to a specific commit hash in an external repo. | Copies external repo code directly into your repository. |
| **Complexity** | Requires `git submodule update --init --recursive`. | Transparent to collaborators; behaves like regular files. |
| **Repo Size** | Keeps parent repo lightweight. | Increases parent repo size. |

---

## 9. Placement Interview Verification Questions

1. **How does `git reflog` differ from `git log`?**
   - `git log` traverses the commit DAG starting from the current branch HEAD backward through parents. `git reflog` records chronological local movements of the `HEAD` pointer on your specific machine, including resets, rebases, and checkouts.
2. **How does `git bisect` locate a bug with logarithmic complexity?**
   - It performs a binary search between a known good commit and a known bad commit, halving the search space on each step ($O(\log N)$) until it isolates the regression commit.
3. **What is `git worktree` and when would you use it over `git stash`?**
   - `git worktree` lets you check out multiple branches simultaneously into separate directories on disk. It is superior when you must quickly fix a production hotfix without interrupting your local build server, tests, or uncommitted work.
4. **Why is `git rm` insufficient when credentials are committed accidentally?**
   - `git rm` only removes the file from the current and future commits. The secret remains embedded in all previous historical commit objects and can be extracted by cloning or viewing historical diffs.
5. **What does `git gc` do under the hood?**
   - It packs loose objects into delta-compressed packfiles, updates reference packing, and prunes orphaned/unreachable commits that exceed the retention expiry period.
