# Checklist — 02: Installation & Configuration

Track your setup and configuration tasks.

---

### Setup & Environment
- [ ] Verify Git is installed on your operating system using `git --version`.
- [ ] Understand the differences between Git Bash, PowerShell, and integrated terminals.
- [ ] Configure `init.defaultBranch` to `main`.
- [ ] Configure `core.editor` to your preferred editor if needed.

---

### Identity & Config Inspection
- [ ] Set global username with `git config --global user.name "..."`.
- [ ] Set global email with `git config --global user.email "..."`.
- [ ] List all active configs with `git config --list`.
- [ ] List global configs with `git config --global --list`.
- [ ] Understand the 3 configuration scopes (System, Global, Local) and their order of precedence.
- [ ] Inspect configuration file locations using `git config --list --show-origin`.

---

### Interview Verification Questions
- [ ] What happens if you run `git commit` before configuring `user.name` and `user.email`?
- [ ] How do you configure a corporate/work email for a single project while keeping your personal email globally?
- [ ] Where is the global configuration stored on Linux/macOS vs Windows?
