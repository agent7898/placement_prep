# 02 — Installation, Environment Setup & Configuration

> **Reference Video:** [Git & GitHub Tutorial — Navin Reddy (Telusko)](https://youtu.be/vwj89i2FmG0)

---

## 1. Installation & Environment Verification

### Verifying Git on Your Machine
Open your terminal (PowerShell, Command Prompt, macOS Terminal, or Linux Bash) and run:

```bash
git --version
# Output example: git version 2.41.0
```

If you receive `git: command not found` or `'git' is not recognized`, install Git for your OS:
- **Linux (Debian/Ubuntu):** `sudo apt update && sudo apt install git`
- **macOS:** Install Xcode Command Line Tools via `xcode-select --install` or via Homebrew `brew install git`.
- **Windows:** Download official installer from [git-scm.com](https://git-scm.com/download/win).

> [!NOTE]
> On Windows, restart your Command Prompt or PowerShell after installation so the newly added Git binary in your system `PATH` is detected.

---

## 2. Terminal Options & Editor Setup

- **Terminal Environments:**
  - **Git Bash:** Provides a Unix-like bash shell on Windows with standard utilities (`ls`, `grep`, `ssh`, `cat`).
  - **PowerShell / CMD:** Native Windows shells.
  - **VS Code Integrated Terminal:** Convenient integrated terminal directly inside your code editor.
- **Default Editor:**
  - By default, Git opens Vim for interactive commit messages and merges.
  - You can configure your preferred editor (such as VS Code or Nano):
    ```bash
    # Set VS Code as default editor
    git config --global core.editor "code --wait"
    ```

---

## 3. Configuring User Identity

Every Git commit embeds the author's name and email immutably into the commit object metadata. If unconfigured, Git will prompt or warn you before your first commit.

### Essential Commands

```bash
# 1. Set your full name globally
git config --global user.name "Your Full Name"

# 2. Set your email globally (must match your GitHub email)
git config --global user.email "your.email@example.com"

# 3. Set the default initial branch to 'main'
git config --global init.defaultBranch main
```

---

## 4. Configuration Levels & Precedence

Git supports three levels of configuration settings:

```text
┌────────────────────────────────────────────────────────┐
│ System Level (/etc/gitconfig)                          │  Lowest Priority
│   └─ Applies to all users on the operating system       │
├────────────────────────────────────────────────────────┤
│ Global Level (~/.gitconfig or ~/.config/git/config)    │  Medium Priority
│   └─ Applies to all repositories for your user account  │
├────────────────────────────────────────────────────────┤
│ Local Level (.git/config inside specific repo)         │  Highest Priority
│   └─ Applies ONLY to this specific repository           │
└────────────────────────────────────────────────────────┘
```

### Inspecting Configurations

```bash
# View all active configuration settings (all levels combined)
git config --list

# View only global configurations
git config --global --list

# Check the value of a specific setting
git config user.name
git config user.email

# View where each configuration setting was loaded from
git config --list --show-origin
```

---

## 5. Summary Reference Table

| Goal | Command | Scope |
|---|---|---|
| Check installed Git version | `git --version` | System |
| Set author name | `git config --global user.name "<Name>"` | User / Global |
| Set author email | `git config --global user.email "<Email>"` | User / Global |
| Set repository-specific email | `git config --local user.email "<Work-Email>"` | Current Repo |
| Set default initial branch | `git config --global init.defaultBranch main` | User / Global |
| List global configurations | `git config --global --list` | User / Global |
| Show config source file origin | `git config --list --show-origin` | All Levels |
