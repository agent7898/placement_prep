# 11 — Advanced Merging, Merge Strategies & Conflict Resolution

> **Interview Focus:** 3-Way Merge Algorithm, Merge Bases, Merge Strategies (`ort`, `recursive`, `ours`, `theirs`), Complex Conflict Resolution, `--ours` vs `--theirs`, and `git rerere`.

---

## 1. The Anatomy of a 3-Way Merge

While a Fast-Forward merge simply moves a branch pointer along a straight commit chain, real-world development branches diverge. To merge diverged branches, Git performs a **3-Way Merge**.

### How the 3-Way Merge Works Under the Hood
Git does not simply compare Branch A against Branch B. Doing so would make it impossible to determine whether a difference was an addition, a deletion, or an edit.

Instead, Git inspects **three distinct commits**:
1. **$B$ (Base):** The **Common Ancestor** — the commit where the two branches originally diverged.
2. **$O$ (Ours):** The tip commit of the current checked-out branch (e.g., `main` or `HEAD`).
3. **$T$ (Theirs):** The tip commit of the incoming branch being merged (e.g., `feature`).

```text
                  C3 ─── C4 (feature / Theirs)
                 /
 C1 ─── C2 (Base)
         \
          C5 ─── C6 (main / Ours / HEAD)
```

### Finding the Merge Base
You can inspect the common ancestor Git will use with the `git merge-base` plumbing command:

```bash
git merge-base main feature
# Outputs: commit hash of C2
```

### The 3-Way Comparison Logic Table

| Base ($B$) | Ours ($O$) | Theirs ($T$) | Git Resolution | Result in Merge |
|---|---|---|---|---|
| Value $X$ | Value $X$ | Value $Y$ | Only "Theirs" changed the line | Takes Value $Y$ automatically |
| Value $X$ | Value $Y$ | Value $X$ | Only "Ours" changed the line | Keeps Value $Y$ automatically |
| Value $X$ | Value $Y$ | Value $Y$ | Both changed to identical values | Takes Value $Y$ automatically |
| Value $X$ | Value $Y$ | Value $Z$ | Both changed the line differently | **MERGE CONFLICT!** Manual resolution required |
| Exists | Deleted | Modified | One branch deleted, one edited | **Conflict: Modify/Delete** |
| Does not exist | Created File A | Created File A | Both created same file with diff content | **Conflict: Add/Add** |

---

## 2. Git Merge Strategies

Git provides configurable merge strategies via `git merge -s <strategy>` and strategy options via `-X <option>`.

```bash
git merge -s <strategy> -X <strategy-option> <branch>
```

### Common Merge Strategies

| Strategy | Description | When Used |
|---|---|---|
| **`ort` (Ostensibly Recursive's Twin)** | The default strategy in Git (since v2.33). Written from scratch to replace `recursive`. Vastly faster, handles renames and conflict detection with high performance. | Default for all 2-branch merges. |
| **`recursive`** | The former default strategy for decades. Handles situations where two branches have more than one common ancestor by creating a virtual merge base. | Legacy default prior to Git 2.33. |
| **`resolve`** | Traditional 3-way merge algorithm using a single common ancestor. Fast, but cannot handle multi-ancestor criss-cross merges. | Specialized workflows. |
| **`ours`** | Ignores all changes from the incoming branch completely. Forces the merge to adopt the current branch's tree entirely while still recording the incoming branch as a parent commit. | Deprecating an old branch while uniting histories. |
| **`octopus`** | Merges more than two branches simultaneously in a single commit. Refuses to merge if conflicts occur (all merges must be clean). | Automated integration merges of multiple clean feature branches. |
| **`subtree`** | A modified recursive strategy that adjusts directory prefixes so one branch matches a subdirectory of another. | Integrating external repositories before Git submodules/subtrees were standardized. |

### Powerful Strategy Options (`-X`)

Strategy options are passed to the default strategy (such as `ort`):

#### 1. Prioritize One Side in Conflicts (`-X ours` / `-X theirs`)
```bash
# In any conflicting chunk, automatically choose our version (HEAD)
git merge -X ours feature-branch

# In any conflicting chunk, automatically choose their version
git merge -X theirs feature-branch
```
> [!WARNING]
> Do NOT confuse the strategy `-s ours` with the strategy option `-X ours`!
> - `-s ours` completely **throws away all changes** from the incoming branch, even non-conflicting ones.
> - `-X ours` performs a normal 3-way merge, but if and only if there is a conflict on a specific line, it resolves it using our branch's version.

#### 2. Ignore Whitespace Differences
Whitespace changes (tabs vs spaces, Windows CRLF vs Linux LF) often trigger false merge conflicts:
```bash
# Ignore all whitespace changes when resolving merges
git merge -X ignore-all-space feature-branch

# Ignore whitespace changes at end of lines
git merge -X ignore-space-at-eol feature-branch
```

---

## 3. Deep Dive: Types of Merge Conflicts & Resolution

### Conflict Type 1: Content Conflicts (Same File, Different Lines/Edits)
When both branches modify overlapping lines:

```text
<<<<<<< HEAD
export const API_TIMEOUT = 5000;
=======
export const API_TIMEOUT = 10000;
>>>>>>> feature/timeout-bump
```

#### Step-by-Step Resolution:
1. Identify all files in conflict:
   ```bash
   git status
   # Conflicted files are labeled: "both modified: <filename>"
   ```
2. Open the file, choose the correct value, and delete markers:
   ```typescript
   export const API_TIMEOUT = 10000;
   ```
3. Mark as resolved:
   ```bash
   git add <filename>
   ```
4. Complete the merge:
   ```bash
   git commit -m "Merge branch 'feature/timeout-bump' - updated API_TIMEOUT"
   ```

### Conflict Type 2: File Rename / Delete Conflicts
Happens when Developer A renames or modifies a file, while Developer B deletes it:
```text
CONFLICT (modify/delete): src/utils/helpers.js deleted in HEAD and modified in feature.
Version feature of src/utils/helpers.js left in tree.
```

**To keep the file from feature:**
```bash
git add src/utils/helpers.js
git commit
```

**To accept the deletion:**
```bash
git rm src/utils/helpers.js
git commit
```

### Conflict Type 3: Binary File Conflicts
Git cannot insert text conflict markers into images, PDFs, or compiled `.jar` / `.so` binaries.
```text
warning: Cannot merge binary files: assets/logo.png (HEAD vs. feature)
```

To resolve, explicitly select which binary version to retain:
```bash
# Keep our local version (HEAD)
git checkout --ours assets/logo.png
git add assets/logo.png

# OR keep the incoming version
git checkout --theirs assets/logo.png
git add assets/logo.png

git commit
```

---

## 4. Visual Merge Tools (`git mergetool`)

Instead of manually editing markers, you can launch a dedicated diff and merge utility:

```bash
# Configure preferred mergetool (e.g., vscode)
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait --merge "$LOCAL" "$REMOTE" "$BASE" "$MERGED"'

# Launch tool when conflict occurs
git mergetool
```

Common supported tools: VS Code, Meld, KDiff3, Beyond Compare, IntelliJ IDEA.

---

## 5. `git rerere`: Reuse Recorded Resolution

One of Git's most powerful yet underutilized features is **`rerere`** (**RE**use **RE**corded **RE**solution).

### The Problem It Solves
If you are rebasing a long-running feature branch with 10 commits against an evolving `main`, you might have to resolve the **exact same conflict 10 times in a row**, or repeat the resolution every time you rebase weekly.

### How `rerere` Works
When enabled, Git:
1. Records pre-merge conflict images (snapshots of the conflict markers).
2. Records your post-merge resolution image (how you resolved it in `.git/rr-cache`).
3. Whenever Git encounters the identical conflict in the future, it **automatically applies your previous resolution**!

### Enabling and Using `rerere`
```bash
# Enable globally
git config --global rerere.enabled true

# Check rerere status during merge conflict
git rerere status

# Inspect the recorded diff that rerere will apply
git rerere diff

# Clear recorded resolution cache if you made an error
git rerere forget <filepath>
```

---

## 6. Placement Interview Verification Questions

1. **How does Git find the common ancestor in a 3-way merge?**
   - It runs the `git merge-base branchA branchB` algorithm on the commit DAG to find the lowest common ancestor commit.
2. **What is the difference between `-s ours` and `-X ours`?**
   - `-s ours` discards all incoming tree changes and strictly keeps HEAD's tree. `-X ours` runs a standard merge and only uses HEAD's version when individual line conflicts occur.
3. **What is the `ort` merge strategy?**
   - Introduced in Git 2.33 as the default merge strategy; stands for "Ostensibly Recursive's Twin". It is a complete rewrite of the `recursive` strategy with order-of-magnitude performance gains and robust rename detection.
4. **How do you resolve a conflict on a binary file like an image?**
   - Use `git checkout --ours <file>` or `git checkout --theirs <file>`, stage with `git add`, and commit.
5. **What is `git rerere` and why is it useful?**
   - It stands for Reuse Recorded Resolution. It memorizes how you resolved a conflict hunk and automatically applies that exact resolution if that conflict recurs during rebasing or branch synchronization.
