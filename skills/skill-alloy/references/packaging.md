# Packaging

Repo conventions (AGENTS.md and the CLAUDE.md symlink, READMEs, banner, LICENSE, CHANGELOG, .gitignore) come from the **ossify** skill in init mode. This file covers only what is specific to a skill repo. Write these first, so ossify detects the project as a Claude Code plugin and fills the README and AGENTS.md with the right version files and validation command.

## Layout

```text
<repo>/
  skills/<name>/SKILL.md
  skills/<name>/references/*.md
  .claude-plugin/plugin.json
  .claude-plugin/marketplace.json
  ATTRIBUTIONS.md
  ...everything else from ossify
```

## plugin.json

```json
{
  "name": "<name>",
  "version": "1.0.0",
  "description": "<one sentence>",
  "author": { "name": "<owner>", "url": "https://github.com/<owner>" },
  "homepage": "https://github.com/<owner>/<repo>",
  "repository": "https://github.com/<owner>/<repo>",
  "license": "MIT",
  "keywords": ["..."]
}
```

## marketplace.json

```json
{
  "name": "<repo>",
  "owner": { "name": "<owner>", "url": "https://github.com/<owner>" },
  "metadata": { "description": "<one line>", "version": "1.0.0" },
  "plugins": [
    { "name": "<name>", "source": "./", "description": "<one line>", "category": "<category>" }
  ]
}
```

Validate with `claude plugin validate .`.

## ATTRIBUTIONS.md

```markdown
# Attributions

<name> was designed by installing and running the projects below on real tasks, then writing new rules where they agreed and ruling where they conflicted (`skills/<name>/references/conflicts.md`). No text was copied. Ideas are credited here.

| Project | License | Idea used |
|---|---|---|
| [owner/repo](https://github.com/owner/repo) | MIT | <one line> |
```

## What to hand to ossify

- **Why it exists**: the trial failures from Step 5, in two or three sentences
- **What it does**: the modes table from SKILL.md
- **Example**: a real input and output from the self-test
- **Credits**: the source repo list, linking to ATTRIBUTIONS.md
- **Project rules** for AGENTS.md: no copying, real examples only, nothing taken from repos with no license, `SKILL.md` under 200 lines
