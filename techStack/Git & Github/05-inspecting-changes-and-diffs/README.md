# 05 — Inspecting Changes, Diffs & History

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Why Inspect Diffs?

While `git status` tells you *which* files have changed, it does not show *what content* changed inside those files. Reviewing your code before staging and committing prevents bugs, unwanted console logs, and accidental edits from entering your repository.

---

## 2. Using `git diff`

### Diff Between Working Directory and Staging Area
To see unstaged changes in your working tree compared to the staging area (or latest commit):

```bash
git diff
```

### Understanding the Diff Output Format:

```diff
diff --git a/my_first_code.txt b/my_first_code.txt
index 2bb0e71..8d8c3f2 100644
--- a/my_first_code.txt
+++ b/my_first_code.txt
@@ -1,2 +1,4 @@
-Hello world
+Hello world!
+Take input from user
+Add values together
```

- `--- a/file`: Represents the original version (before changes).
- `+++ b/file`: Represents the modified version (after changes).
- `@@ -1,2 +1,4 @@`: Chunk header showing line numbers and counts before and after.
- Lines starting with `-`: Deleted or replaced lines (shown in red).
- Lines starting with `+`: Added lines (shown in green).

---

## 3. The Staging Diff Mystery: `git diff --staged`

A common beginner confusion highlighted in the reference video:

1. You edit `my_first_code.txt`.
2. You run `git diff` and see your changes.
3. You run `git add my_first_code.txt` to stage the file.
4. You run `git diff` again... **and it prints nothing!**

### Why does `git diff` print nothing after staging?
Because `git diff` compares your **Working Directory against the Staging Area**. Once you stage the file, your working directory and staging area are identical!

### How to inspect staged changes:
To compare your **Staging Area against your last commit (`HEAD`)**, use:

```bash
git diff --staged
# (Synonym: git diff --cached)
```

---

## 4. Compact Logs & History Inspection

### Single-Line Logs
Standard `git log` output can become very lengthy. For quick inspection:

```bash
# Clean, one-line summary per commit
git log --oneline

# Formatted single line
git log --pretty=oneline
```

### Exiting the Log Pager
Git uses `less` to page long logs. When viewing logs, **press `q`** to exit back to your command prompt.

### Inspecting a Specific Commit
```bash
# View metadata and full file diff introduced by a specific commit
git show <commit-hash>
```

---

## 5. Command Reference Table

| Command | Compares / Action |
|---|---|
| `git diff` | Working Directory vs Staging Area (unstaged changes) |
| `git diff --staged` | Staging Area vs Last Commit / `HEAD` (staged changes) |
| `git diff <commitA> <commitB>` | Difference between two specific commits |
| `git diff <branchA>..<branchB>` | Difference between tips of two branches |
| `git log --oneline` | Compact view: 7-char hash + commit message |
| `git show <commit-hash>` | Show metadata and patch diff of a commit |
| `q` | Quit log/diff pager |
