# Checklist — 14: GitHub Collaboration & Team Governance

Track your mastery of collaboration models, branching strategies, upstream sync, PR lifecycle, and branch protections.

---

### Concepts
- [ ] Understand the Shared Repository Model vs Fork & Pull Request Model.
- [ ] Differentiate between GitHub Flow, Gitflow, and Trunk-Based Development.
- [ ] Understand how upstream remotes work and why fork synchronization is required.
- [ ] Understand the 3 PR merge methods: Merge Commit, Squash and Merge, Rebase and Merge.
- [ ] Understand the role of Branch Protection Rules and GitHub Rulesets.
- [ ] Understand how `CODEOWNERS` automates code review routing by file paths.
- [ ] Understand the purpose of PR and Issue templates.

---

### Commands & Practice
- [ ] Configure upstream remote: `git remote add upstream <url>`.
- [ ] Verify remotes with `git remote -v`.
- [ ] Execute upstream synchronization flow (`fetch` -> `merge` -> `push`).
- [ ] Open a Pull Request on GitHub with a structured description and issue link.
- [ ] Use GitHub CLI to create a PR: `gh pr create`.
- [ ] Check out a teammate's PR locally: `gh pr checkout <pr-number>`.
- [ ] Set up a `.github/CODEOWNERS` file with team route definitions.
- [ ] Create a `.github/pull_request_template.md` file.

---

### Interview Verification Questions
- [ ] Compare GitHub Flow vs Gitflow: When would an enterprise choose one over the other?
- [ ] What are the tradeoffs between "Squash and Merge" and "Create a Merge Commit" on GitHub?
- [ ] Why is Trunk-Based Development favored in high-velocity CI/CD engineering teams?
- [ ] Explain how to keep a forked repository updated without creating accidental merge bubbles.
