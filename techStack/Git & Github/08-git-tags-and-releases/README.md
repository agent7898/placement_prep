# 08 — Git Tags, Releases & Semantic Versioning

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. What are Git Tags?

In software development, applications undergo continuous updates, but specific milestones represent public releases (e.g., `v1.0.0`, `v2.1.0`).

- **Branches** are *moving pointers*: as you make new commits, the branch pointer automatically moves forward.
- **Tags** are *static bookmarks*: a tag permanently references a specific commit hash in history. Even as development continues on the branch, the tag never moves.

```text
Commit History:
C1 ─── C2 ─── C3 ─── C4 (main)
        ▲              ▲
        │              │
      v1.0           v1.1 (tags mark exact release points)
```

---

## 2. Types of Tags: Annotated vs Lightweight

| Feature | Lightweight Tag | Annotated Tag (Recommended) |
|---|---|---|
| **What is it?** | A pointer pointing directly to a commit hash. | A full object stored in Git's object database. |
| **Metadata** | None (no author, no date, no message). | Includes tagger name, email, date, GPG signature, and message. |
| **Command** | `git tag v1.0` | `git tag -a v1.0 -m "Release version 1.0"` |
| **Typical Use** | Quick temporary local bookmarks. | Official production releases and public versions. |

---

## 3. Creating & Inspecting Tags

```bash
# 1. Create an annotated release tag
git tag -a v1.0 -m "feat: first stable production release"

# 2. List all existing tags
git tag

# 3. Filter tags using wildcard patterns
git tag -l "v1.*"

# 4. View detailed tag metadata, author, and commit details
git show v1.0
```

### Tagging Past Commits
If you forgot to tag a release earlier, you can tag any past commit by specifying its hash:
```bash
git tag -a v0.9 <commit-hash> -m "beta: initial closed beta release"
```

---

## 4. Pushing Tags to GitHub

A major trap highlighted in the reference video:
> [!WARNING]
> By default, `git push` or `git push origin main` **does NOT push tags** to remote repositories! You must push tags explicitly.

```bash
# Push a single specific tag to GitHub
git push origin v1.0

# Push all local tags to GitHub at once
git push origin --tags
```

### Viewing Releases on GitHub
Once pushed, GitHub automatically indexes your tags in the **Releases / Tags** tab and provides downloadable `.zip` and `.tar.gz` archives of the codebase at that exact commit.

---

## 5. Working with Code at a Tag

If you need to view or fix bugs in an older tagged version:

```bash
# Checking out a tag directly places you in a "detached HEAD" state:
git checkout v1.0

# Recommended: create a new branch starting from that tag to safely work:
git switch -c hotfix-v1.0 v1.0
```

---

## 6. Command Reference Table

| Command | Purpose |
|---|---|
| `git tag` | List all tags in the repository |
| `git tag -a <tag> -m "<msg>"` | Create an annotated tag with message |
| `git tag <tag>` | Create a lightweight tag |
| `git show <tag>` | Show tag details, tagger info, and commit diff |
| `git push origin <tag>` | Push a single tag to remote |
| `git push origin --tags` | Push all local tags to remote |
| `git tag -d <tag>` | Delete a tag locally |
| `git push origin --delete <tag>` | Delete a tag from remote repository |
