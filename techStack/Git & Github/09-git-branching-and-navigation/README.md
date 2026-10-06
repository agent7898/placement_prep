# 09 — Git Branching, Navigation & Pointers

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Why Branching is Git's Superpower

In traditional VCS (like SVN), creating a branch makes a complete disk copy of the entire codebase, making branching heavy and slow.

In Git, **branching is virtually instantaneous and lightweight**. A branch is simply a 41-byte file containing the 40-character commit hash it points to.

### Use Cases for Branches:
1. **Feature Isolation:** Develop new features (e.g., `feature-avatar`, `feature-admin`) without risking bugs on the stable `main` branch.
2. **Parallel Experimentation:** Try out risky algorithms or redesigns. If an experiment fails, delete the branch without touching `main`.
3. **Production Bugfixes:** Maintain a clean production branch (`main`) while team members work on separate feature branches.

---

## 2. How Git Branching Works Internally

```text
Initial State:
C1 ─── C2 ─── C3 (main, HEAD)

Creating and switching to 'feature-1':
C1 ─── C2 ─── C3 (main)
               ▲
               └── (feature-1, HEAD)

After committing on 'feature-1':
                 C4 (feature-1, HEAD)
                /
C1 ─── C2 ─── C3 (main)
```

### The Role of `HEAD`
`HEAD` is Git's special pointer indicating where you currently are:
- When on `main`: `HEAD` points to `refs/heads/main`, which points to `C3`.
- When you run `git switch feature-1`: `HEAD` moves to point to `refs/heads/feature-1`.
- When you commit: The commit object records `C3` as its parent, and the `feature-1` pointer moves forward to the new commit `C4`. **`main` remains at `C3` untouched.**

### Working Directory Switching Effect
As demonstrated in the reference video:
- When you are on `feature-1` and create `admin_service.txt`, it is present in your working tree.
- When you run `git switch main`, **Git automatically swaps files on your disk** to reflect the state of `main`. The file `admin_service.txt` disappears from your editor!
- When you switch back with `git switch feature-1`, `admin_service.txt` immediately reappears.

---

## 3. Creating & Switching Branches

### Modern Git Commands (Recommended)
Git 2.23 introduced `git switch` specifically for branch navigation:

```bash
# 1. Create and switch to a new branch in one command (-c for create)
git switch -c feature-1

# 2. Switch to an existing branch
git switch main

# 3. Quick toggle: switch back to the previously active branch
git switch -
```

### Classic Commands (`git checkout`)
Still widely used across legacy scripts and interview discussions:
```bash
# Create and switch using checkout
git checkout -b feature-1

# Switch using checkout
git checkout main
```

---

## 4. Managing Branches

```bash
# List all local branches (* indicates current active branch)
git branch

# List both local AND remote-tracking branches
git branch -a

# Safely delete a branch (Git warns if branch has unmerged work)
git branch -d feature-1

# Force delete a branch (discards unmerged commits on that branch)
git branch -D feature-1

# Rename the current branch
git branch -m new-branch-name
```

---

## 5. Pushing Branches to GitHub

```bash
# Push the branch to remote repository
git push origin feature-1

# Push and set upstream tracking
git push -u origin feature-1
```

Once pushed, GitHub notifies you: *"feature-1 had recent pushes less than a minute ago"* and offers a one-click button to **Compare & pull request**.

---

## 6. Visualizing Branch History

```bash
# Compact ASCII graph of all branches and commit history
git log --oneline --graph --decorate --all
```

In VS Code, extensions like **Git Graph** provide an interactive graphical UI showing branch forks, commits, parents, and merges.

---

## 7. Command Reference Table

| Command | Action |
|---|---|
| `git switch -c <name>` | Create and switch to new branch (Modern) |
| `git checkout -b <name>` | Create and switch to new branch (Classic) |
| `git switch <name>` | Switch to existing branch |
| `git switch -` | Switch back to the previous branch |
| `git branch` | List local branches |
| `git branch -a` | List local and remote branches |
| `git branch -d <name>` | Delete merged branch safely |
| `git branch -D <name>` | Force delete branch |
| `git push -u origin <name>` | Push branch and set upstream tracking |
| `git log --graph --oneline --all` | Visual ASCII DAG graph of all branches |
