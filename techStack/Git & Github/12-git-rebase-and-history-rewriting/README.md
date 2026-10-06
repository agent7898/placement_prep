# 12 — Git Rebase & History Rewriting

> **Interview Focus:** Linear History, DAG Commit Replaying, Merge vs Rebase Tradeoffs, The Golden Rule of Rebase, Interactive Rebase (`-i`), Autosquash & Fixup, `git rebase --onto`, and `--force-with-lease`.

---

## 1. What is Git Rebase?

Rebasing is the process of moving or combining a sequence of commits to a new base commit. Under the hood, Git **re-creates each commit on top of the new base**, generating completely new commit hashes (SHA-1s).

### Visualizing Rebase Mechanics

Imagine your branch diverged from `main` at commit `C2`:

```text
Before Rebase:
          C3 ─── C4 (feature)
         /
C1 ─── C2 ─── C5 ─── C6 (main)
```

When you run:
```bash
git switch feature
git rebase main
```

Git executes the following steps:
1. Identifies the common ancestor (`C2`).
2. Temporarily stores the diffs of `C3` and `C4` into memory/temporary files.
3. Resets the `feature` branch pointer to match the tip of `main` (`C6`).
4. Re-applies each saved diff one by one as **brand new commits** (`C3'` and `C4'`).
5. Updates `feature` pointer to point to `C4'`.

```text
After Rebase:
C1 ─── C2 ─── C5 ─── C6 (main) ─── C3' ─── C4' (feature)
```

Now, fast-forward merging `feature` into `main` is completely clean with zero merge commits!

---

## 2. In-Depth Comparison: `git merge` vs `git rebase`

| Dimension | `git merge` | `git rebase` |
|---|---|---|
| **Graph Structure** | Preserves branch topology; non-linear, branching DAG with merge commits. | Flattens branches into a clean, perfectly linear commit history. |
| **Commit Hashes** | Preserves original commit hashes unchanged. | Rewrites commit hashes (`C3` becomes `C3'`). |
| **Commit Context** | Keeps original author and commit timestamps intact. | Author date remains, but commit date and parent pointers are updated. |
| **Traceability** | Easy to identify when a feature was merged; single command `git revert -m 1 <merge-commit>` reverts entire feature. | Individual commits are inlined; difficult to distinguish where a feature started/ended without tags. |
| **Conflict Handling** | Conflicts are resolved once in the merge commit. | Conflicts must be resolved sequentially per replayed commit. |
| **Safety** | Completely safe on shared/public branches. | Dangerous on public branches; history rewriting can corrupt collaborators' work. |

### Which One Should You Use?
- **Rebase:** For local, unpushed feature branches to keep feature commits clean and up-to-date with `main` before submitting a PR.
- **Merge:** For integrating long-running team branches or public releases where true audit history and provenance matter.

---

## 3. The Golden Rule of Rebasing

> [!CAUTION]
> **NEVER rebase commits that have already been pushed to a public, shared repository branch!**

### Why?
Rebasing replaces commits with brand-new ones. If Teammate A rebases commits on `main` and force-pushes, Teammate B’s local `main` now references old commits that no longer exist on the remote. Teammate B will end up with duplicate commits, conflicting histories, and mass confusion when merging.

**Rule of Thumb:**
- **Rebase your local private branch against shared `main`:** ✅ SAFE.
- **Rebase a shared branch (`main`, `develop`, staging):** ❌ FORBIDDEN.

---

## 4. Interactive Rebase (`git rebase -i`)

Interactive rebasing gives you full control over past commits before making them public.

```bash
git rebase -i HEAD~4
```

This launches your editor with an interactive todo list (listed oldest to newest):

```text
pick 1a2b3c4 feat: add user authentication
pick 2b3c4d5 fix typo in auth controller
pick 3c4d5e6 debug print statements
pick 4d5e6f7 test: unit tests for auth
```

### Available Interactive Rebase Commands

| Command | Action | Use Case |
|---|---|---|
| `pick` (or `p`) | Use commit as is. | Keep commit untouched. |
| `reword` (or `r`) | Use commit, but edit commit message. | Fix spelling, clarify description. |
| `edit` (or `e`) | Pause rebase at this commit to amend changes. | Split commit, add missing files. |
| `squash` (or `s`) | Melt commit into the previous commit; combine commit messages. | Combine "WIP" commits into one atomic commit. |
| `fixup` (or `f`) | Melt commit into previous commit; discard this commit's message. | Absorb bug fixes into earlier feature commit cleanly. |
| `drop` (or `d`) | Delete the commit entirely. | Remove accidental debug commits or temporary files. |
| `exec` (or `x`) | Run a shell command after this commit. | Run tests (`exec npm test`) to verify every single commit passes CI. |
| `break` (or `b`) | Stop rebase at this position. | Pause and inspect repository state. |

---

## 5. Automated Fixup & Autosquash Workflow

Instead of manually editing the `git rebase -i` file, modern professional Git supports automated autosquashing:

### The Problem:
You are working on a feature and find a typo in commit `a1b2c3d` from 3 commits ago.

### The Autosquash Solution:
```bash
# 1. Stage the fix
git add src/auth.js

# 2. Commit as a targeted fixup to the target commit
git commit --fixup a1b2c3d
# Creates a commit titled: "fixup! feat: user login"

# 3. Automatically reorder and squash during rebase
git rebase -i --autosquash HEAD~5
```
Git automatically places the fixup commit directly underneath `a1b2c3d`, marks it as `fixup`, and squashes it without opening an interactive editor prompt!

---

## 6. Advanced Rebase: `git rebase --onto`

`git rebase --onto` lets you transplant a branch from one parent branch to another.

### Scenario: Branching off the Wrong Branch
You started `feature-b` off `feature-a`, but `feature-a` is blocked in review. You want to transplant `feature-b` directly onto `main`.

```text
Before:
main ─── C1 ─── C2
                 \
   feature-a ───── A1 ─── A2
                           \
             feature-b ───── B1 ─── B2
```

### Command Syntax:
```bash
git rebase --onto <new-base> <old-base> <branch-to-move>
```

```bash
git rebase --onto main feature-a feature-b
```

```text
After:
main ─── C1 ─── C2 ─── B1' ─── B2' (feature-b)
```
Commits `B1` and `B2` are now directly based on `main`, completely detangling them from `feature-a`!

---

## 7. Handling Conflicts During Rebase

When a conflict occurs during a rebase, Git pauses at the specific replayed commit:

```text
error: could not apply 3c4d5e6... feat: update payment API
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase": run "git rebase --abort".
```

### Steps to Resolve:
1. Inspect conflicts: `git status`.
2. Fix conflict markers in the file.
3. Stage resolved file: `git add <file>`.
4. Continue the rebase:
   ```bash
   git rebase --continue
   ```
   *(Note: Do NOT run `git commit` here! Git handles creating the new replayed commit).*

### Emergency Bailouts:
- **Abort:** Completely restores the branch to its exact pre-rebase state:
  ```bash
  git rebase --abort
  ```
- **Skip:** Discards the changes of the currently failing commit and proceeds to the next:
  ```bash
  git rebase --skip
  ```

---

## 8. Safe Force Pushing: `--force-with-lease`

Once you rewrite local branch history, Git will reject normal `git push` because the remote branch and local branch have diverged.

```bash
# ❌ DANGEROUS: Blindly overwrites remote branch, destroying teammates' commits
git push --force origin my-feature

# ✅ SAFE & PROFESSIONAL:
git push --force-with-lease origin my-feature
```

### Why `--force-with-lease` is Superior
`--force-with-lease` checks your local remote-tracking reference (`origin/my-feature`). If another developer pushed commits to that remote branch since your last fetch, Git **aborts the push**, preventing accidental overwrites of their work.

---

## 9. Placement Interview Verification Questions

1. **Why does rebasing change commit hashes?**
   - A commit hash is a cryptographic SHA hash that includes the parent commit's hash. Changing the base commit alters the parent reference, which fundamentally cascades into a new SHA hash.
2. **Explain the difference between `squash` and `fixup` in interactive rebase.**
   - Both merge the commit into the previous commit. `squash` prompts you to combine or edit the commit messages; `fixup` discards the current commit's message and keeps only the parent's message.
3. **What is the Golden Rule of Rebasing?**
   - Never rebase a public, shared branch. Only rebase private, local branches before integrating them.
4. **How does `git push --force-with-lease` prevent data loss compared to `git push --force`?**
   - It checks whether the remote ref matches the expected ref in your local tracking cache. If someone pushed unexpected commits to the remote branch, the force push is refused.
5. **How does `git rebase --onto` work?**
   - It cuts a sequence of commits starting from `<old-base>` to `<branch>` and grafts them onto a completely different base branch `<new-base>`.
