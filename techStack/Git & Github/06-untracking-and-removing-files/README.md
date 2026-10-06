# 06 — Untracking, Removing Files & Handling Secrets

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. The Sensitive File Dilemma

A common and dangerous mistake in real projects:

1. A developer creates a local configuration file with sensitive credentials (e.g., `all_my_credits.txt` or `.env` with API keys and database passwords).
2. The developer casually runs `git add .` and commits:
   ```bash
   git commit -m "feat: database connectivity setup"
   ```
3. Before pushing to GitHub, they realize: *"If I push this, anyone on GitHub can read my private credentials!"*

---

## 2. Removing from Git vs Deleting from Disk

### Option A: Deleting the File Completely (`git rm`)
If you want to delete the file from both Git tracking and your physical disk:

```bash
git rm all_my_credits.txt
git commit -m "fix: delete credentials file"
```

### Option B: Untracking from Git but Keeping on Disk (`git rm --cached`)
Often, you need the file locally to run your app, but you do **NOT** want Git to track it or push it to remote servers:

```bash
# 1. Remove the file from Git's index/tracking, leaving the local disk file intact
git rm --cached all_my_credits.txt

# 2. Verify with git status:
# The file now shows under "Changes to be committed: deleted: all_my_credits.txt"
# AND simultaneously under "Untracked files: all_my_credits.txt"
git status

# 3. Commit the removal from Git
git commit -m "chore: stop tracking local credentials file"
```

---

## 3. Ignoring Files Permanently with `.gitignore`

Once untracked with `git rm --cached`, Git will list the file as an untracked file. To permanently prevent Git from ever prompting or staging it again, add it to a `.gitignore` file.

### Creating `.gitignore`:
```gitignore
# Ignore specific files
all_my_credits.txt
.env
config/secrets.json

# Ignore folders and build outputs
node_modules/
target/
build/
dist/

# Ignore log files and OS metadata
*.log
.DS_Store
Thumbs.db
```

### Crucial Interview Trap: Why `.gitignore` "Doesn't Work"
> [!CAUTION]
> If a file is **already tracked** in Git, adding its name to `.gitignore` will **NOT** stop Git from tracking changes to it!  
> You must untrack it first:
> ```bash
> git rm --cached <already-tracked-file>
> git commit -m "chore: untrack file now covered by .gitignore"
> ```

---

## 4. Command Reference Table

| Command | Action on Git Index | Action on Local Disk |
|---|---|---|
| `git rm <file>` | Removes from tracking | **Deletes file from disk** |
| `git rm --cached <file>` | Removes from tracking | **Keeps file on disk** (now untracked) |
| `git rm -r --cached <dir>` | Recursively untracks folder | **Keeps folder on disk** |
