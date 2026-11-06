# Publishing

Committing, creating the public repo, tagging `v1.0.0`, and the GitHub release all follow the **ossify** skill (its commit-and-push step, then release mode). This file adds what is specific to a merged skill repo.

## Privacy scan (before the first commit)

ossify runs a generic scan. Add the terms you learned during discovery and trial:

```bash
grep -rniE '<company>|<internal-project>|<private skill names>|<email-domain>|<home path>' . --exclude-dir=.git
```

Trial notes often contain private context (the user's real posts, internal tool names). None of it goes into examples unless the user said it is fine to publish.

## Release notes extras

Add to the notes ossify writes:

- how many source repos were studied and how many conflicts were ruled on
- a link to `references/conflicts.md`

## Topics

```bash
gh repo edit <owner>/<repo> --add-topic claude-code,agent-skills,claude-skills,<topic>
```

## Verify the plugin from GitHub

```bash
claude plugin marketplace add <owner>/<repo>
claude plugin install <name>@<repo>
claude plugin list | grep -A3 '<name>@<repo>'
ls ~/.claude/plugins/cache/<repo>/<name>/*/skills/<name>/
curl -sI https://raw.githubusercontent.com/<owner>/<repo>/main/assets/banner.png | head -1   # 200
curl -s  https://raw.githubusercontent.com/<owner>/<repo>/main/CLAUDE.md                       # AGENTS.md
```

## Later changes

Follow the new repo's `AGENTS.md` (written by ossify): commit and push every change; release only for distribution changes, using ossify's release mode.
