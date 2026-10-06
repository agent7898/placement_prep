# 10 — Merging, Remote Synchronization & Conflict Management

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. The Merging Lifecycle

Once development and testing on a feature branch are complete, those changes must be integrated back into your primary branch (typically `main`).

```text
Before Merge:
                 C3 ─── C4 (feature-1)
                /
C1 ─── C2 ───── (main)

After Merge:
                 C3 ─── C4 (feature-1)
                /         \
C1 ─── C2 ───────────────── M (main: merged!)
```

---

## 2. Step-by-Step Professional Merging Workflow

As highlighted in the reference video, always follow this disciplined sequence:

### Step 1: Switch to the target branch
Make sure you are on the branch that will *receive* the changes:
```bash
git switch main
```

### Step 2: Pull the latest remote updates first
> [!IMPORTANT]
> **Why always pull before merging?**  
> In collaborative team environments, teammates may have pushed new commits to `main` while you were working on your branch. Running `git pull origin main` ensures your local `main` is completely up-to-date before merging.

```bash
git pull origin main
```

### Step 3: Merge the feature branch
```bash
git merge feature-1
```

### Step 4: Verify the result
```bash
git status
git log --oneline --graph -5
```

### Step 5: Push the merged updates to GitHub
```bash
git push origin main
```

---

## 3. Fast-Forward vs 3-Way Merge

### Fast-Forward Merge
- Occurs when there are **no new commits** on `main` since you created `feature-1`.
- Git does not create a new merge commit object; it simply slides the `main` branch pointer forward to the tip of `feature-1`.
- If you prefer to always retain a merge commit for audit history:
  ```bash
  git merge --no-ff feature-1
  ```

### 3-Way Merge Commit
- Occurs when both `main` and `feature-1` have diverged with independent commits.
- Git looks at:
  1. The common ancestor commit
  2. The tip of `main`
  3. The tip of `feature-1`
- Git combines the changes and automatically creates a new **merge commit** with two parent pointers.

---

## 4. Understanding Merge Conflicts

A merge conflict occurs when the exact same lines of a file were modified differently in both branches.

### Conflict Markers Anatomy:
```text
<<<<<<< HEAD
const API_BASE = "https://api.production.com";
=======
const API_BASE = "https://api.v2.staging.com";
>>>>>>> feature-1
```

- `<<<<<<< HEAD`: Content from your active branch (`main`).
- `=======`: The boundary line between conflicting changes.
- `>>>>>>> feature-1`: Content from the branch being merged in.

### Resolving the Conflict:
1. Open the file and manually edit it to the desired final state.
2. Remove all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Stage the resolved file:
   ```bash
   git add <resolved-file>
   ```
4. Commit the resolution:
   ```bash
   git commit -m "chore: resolve merge conflict in API configuration"
   ```

### Emergency Abort:
If you are unsure or need to consult your teammate:
```bash
git merge --abort
# Restores repository to the clean state before the merge attempt
```

---

## 5. Command Reference Table

| Command | Action |
|---|---|
| `git merge <branch>` | Merge specified branch into current active branch |
| `git merge --no-ff <branch>` | Force creation of a merge commit (no fast-forward) |
| `git merge --abort` | Abort an in-progress merge conflict and reset |
| `git pull origin <branch>` | Fetch and merge remote changes into current branch |
| `git fetch origin` | Download remote objects without touching working tree |
| `git push origin <branch>` | Push local branch updates to remote |
