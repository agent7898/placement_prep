# 03 — Git Architecture & The Three Local Areas

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Initializing a Repository

A folder on your machine is simply an ordinary directory until you initialize Git within it. 

```bash
# Navigate to your project folder
mkdir first-project
cd first-project

# Initialize a Git repository with 'main' as the default branch
git init -b main
```

### What `git init` Actually Does
Running `git init` creates a hidden folder named `.git/` in your root project directory:

```text
first-project/
├── .git/                  <-- Git internal database and metadata
│   ├── HEAD               <-- Pointer to current branch (ref: refs/heads/main)
│   ├── config             <-- Local repo configuration file
│   ├── description        <-- GitWeb description file
│   ├── hooks/             <-- Client/server hook scripts
│   ├── info/              <-- Additional repo info (e.g. exclude)
│   ├── objects/           <-- Content-addressable object store (blobs, trees, commits)
│   └── refs/              <-- Pointers to branches and tags
└── (your project files)   <-- Working directory
```

> [!TIP]
> **How to completely remove Git from a project:**  
> Simply delete the hidden `.git` folder (`rm -rf .git` on Linux/macOS or `rmdir /s /q .git` on Windows). All your code files remain intact, but Git's entire version history is removed.

---

## 2. The Three Local Areas (Mental Model)

A crucial concept emphasized in the reference video is that **your working directory is NOT your Git repository**:

```text
 ┌──────────────────────────┐         git add          ┌──────────────────────────┐
 │    Working Directory     │ ───────────────────────> │   Staging Area (Index)   │
 │                          │                          │                          │
 │  Files you create/edit   │ <─────────────────────── │  Pre-commit cache area   │
 │   in your IDE/editor     │       git restore        │       (.git/index)       │
 └──────────────────────────┘                          └──────────────────────────┘
                                                                     │
                                                                     │ git commit
                                                                     ▼
                                                       ┌──────────────────────────┐
                                                       │     Local Repository     │
                                                       │                          │
                                                       │    Permanent DAG of      │
                                                       │      commit objects      │
                                                       └──────────────────────────┘
```

### 1. Working Directory
- The actual directory on your filesystem containing your source code, images, and documents.
- Any edits, deletions, or new files created here are completely untracked or ignored by Git until explicitly told.

### 2. Staging Area (The Index)
- A binary file located at `.git/index`.
- Serves as the **staging dock** or preview of what will go into your next commit.
- **Why is it essential?** It allows you to craft focused, atomic commits. If you edited 5 files for two different bug fixes, you can stage and commit 2 files for Bug A, and then stage and commit 3 files for Bug B.

### 3. Local Repository
- The object database (`.git/objects/`) and reference pointers (`.git/refs/`).
- Once committed, snapshots are immutably recorded with a unique SHA checksum.

---

## 3. Checking Repository State with `git status`

`git status` is the most frequently run command in Git. It answers three questions:
1. Which branch are you currently on?
2. What changes are staged for the next commit?
3. What changes are in the working directory but not yet staged (or untracked)?

```bash
git status
```

### Interpreting Output:

- **Untracked files:** New files that Git has never tracked before:
  ```text
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
          my_first_code.txt
  ```
- **Changes not staged for commit:** Tracked files that have been modified in the working tree:
  ```text
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
          modified:   my_first_code.txt
  ```
- **Changes to be committed:** Files currently in the staging area:
  ```text
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
          new file:   my_first_code.txt
  ```
- **Working tree clean:** All files in the working directory match the latest commit:
  ```text
  nothing to commit, working tree clean
  ```

---

## 4. Command Summary

| Command | Purpose |
|---|---|
| `git init` | Initialize a new Git repository in current folder |
| `git init -b main` | Initialize repository with default initial branch `main` |
| `git status` | Show current branch and status of working tree & staging area |
| `git status -s` | Show concise short-format status (`??` untracked, `M` modified, `A` added) |
| `rm -rf .git` | Completely uninitialize Git and delete all local version history |
