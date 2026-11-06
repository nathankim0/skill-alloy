# Publishing

## Privacy scan (before the first commit)

```bash
grep -rniE '<company>|<internal-project>|<email-domain>|/Users/|@gmail|token|secret' . --exclude-dir=.git
```

Build the pattern from what you know about the user: employer, internal tool names, private skills, email domains. Zero hits before committing.

## First release

```bash
cd <repo>
claude plugin validate .
git init -q -b main
git add -A
git commit -m "Add <name> skill v1.0.0

<attribution line from the session, if any>"
gh repo create <owner>/<repo> --public --source . --push --description "<one line>"

cat > <scratchpad>/notes-v1.0.0.md <<'EOF'
First release of **<name>**: <one line>.

### Highlights
- ...

### Install
/plugin marketplace add <owner>/<repo>
/plugin install <name>@<repo>
EOF

git tag -a v1.0.0 -m "v1.0.0"
git push origin v1.0.0
gh release create v1.0.0 --title "v1.0.0" --notes-file <scratchpad>/notes-v1.0.0.md
gh repo edit <owner>/<repo> --add-topic claude-code,agent-skills,<topic>
```

Release notes end with the session's PR or release attribution line when one is configured.

## Verify

```bash
claude plugin marketplace add <owner>/<repo>
claude plugin install <name>@<repo>
claude plugin list | grep -A3 '<name>@<repo>'
ls ~/.claude/plugins/cache/<repo>/<name>/*/skills/<name>/
curl -sI https://raw.githubusercontent.com/<owner>/<repo>/main/assets/banner.png | head -1   # 200
curl -s  https://raw.githubusercontent.com/<owner>/<repo>/main/CLAUDE.md                       # AGENTS.md
gh release view v1.0.0 --repo <owner>/<repo> --json tagName -q .tagName
```

## Later changes

Follow the repo's own `AGENTS.md`: commit and push every change; release (version bump, CHANGELOG, tag, GitHub release) only for distribution changes.
