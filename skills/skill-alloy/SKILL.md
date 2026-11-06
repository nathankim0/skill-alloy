---
name: skill-alloy
description: Turn a topic into a published open-source agent skill. Finds existing skills on GitHub, vets them for safety and license, installs and actually runs them on a real task, resolves the rules where they conflict, writes one original merged skill, and publishes it as a public repo with a Claude Code plugin manifest, English and Korean READMEs, a generated banner image, AGENTS.md with a CLAUDE.md symlink, a tag, and a GitHub release. Use when the user says "스킬 찾아서 합쳐줘", "오픈소스 스킬로 만들어서 올려줘", "make a skill from what's on GitHub", "merge these skills into one", or "publish this as a skill repo".
argument-hint: "[topic or list of skill repos] [repo name]"
---

# skill-alloy

Many small open-source skills go in. One stronger, original, published skill comes out.

This workflow came from building a real skill (`nathankim0/ssul`) this way. Its two hardest lessons are built into the steps:

1. **Reading a skill is not using it.** Skills only show their gaps when you invoke them on a real task and show the evidence. Step 5 is mandatory.
2. **Merged skills fail where sources disagree.** Write the conflicts down and rule on each one (Step 6). That table is the most valuable part of the result.

## Inputs

Ask nothing that you can default. Defaults:

| Input | Default |
|---|---|
| topic | from the request |
| source repos | discovered in Step 2, plus any the user named |
| repo name | short, lowercase, memorable; check it is free with `gh repo view <owner>/<name>` |
| owner | `gh api user --jq .login` |
| local path | `~/<repo-name>` |
| languages | README in English plus `README.ko.md` (or the user's language) |
| license | MIT |

## Workflow

### 1. Scope
State in one line what the merged skill does and who uses it. If the user has an earlier failed attempt in the conversation, list what went wrong; those become requirements.

### 2. Discover
Search broadly, in English and in the target language. Queries, filters, and spam signals: `references/discovery.md`. Produce a ranked shortlist with stars, last update, license, and one line on fit. Report it to the user only if they asked to choose; otherwise continue.

### 3. Vet
Clone every candidate into its own folder under the scratchpad, never into the target repo. Read every `SKILL.md`, every hook, and every script before installing. Checklist: `references/vetting.md`. Drop anything that runs unexpected network calls, touches credentials, or auto-posts without approval, and say why.

### 4. Install
Install the survivors so they can run. Prefer the repo's plugin marketplace (`claude plugin marketplace add`, `claude plugin install`); otherwise copy skill folders into `~/.claude/skills/`. Rename on name collisions (edit the `name:` field and folder) and report each rename. For huge bundles, install only the relevant skills and say so. Plugins load in the next session; skills in `~/.claude/skills/` load now.

### 5. Trial (mandatory)
Run the installed skills on one real task from the user's domain, invoking each relevant skill for real (Skill tool, or its documented command). For each, record: what it caught, what it missed, and what it got wrong. Save the notes to the scratchpad. If the user reacts to outputs ("너무 무겁다", "뭔 소린지"), those reactions are the most important findings.

### 6. Synthesize
Write the merged skill from scratch. Method and templates: `references/synthesis.md`.

- one `SKILL.md` under 200 lines: modes, ordered workflow, output contract
- `references/` for detail, loaded on demand
- `references/conflicts.md`: every contradiction between sources, with a ruling and a one-line reason
- examples must be real or clearly marked as placeholders
- never copy text; credit ideas in `ATTRIBUTIONS.md`; never take anything from a repo with no license

### 7. Package
Scaffold the repo with the files in `references/packaging.md`: plugin and marketplace manifests, `LICENSE`, `CHANGELOG.md`, `ATTRIBUTIONS.md`, `AGENTS.md` plus `CLAUDE.md -> AGENTS.md`, `README.md` and `README.ko.md` with a language switch, and `assets/banner.png`.

Generate the banner with GPT image generation through the Codex CLI, in the background, at the start of this step. Prompt template and fallback: `references/packaging.md#banner`. Look at the image before using it.

### 8. Privacy scan
Grep the repo for company names, internal project names, emails, internal URLs, home-directory paths, tokens, and anything from the user's private instructions. Fix every hit before the first commit. Commits are public forever.

### 9. Publish
Commands: `references/publishing.md`. Validate, commit with the session's attribution line, create the public repo, push, tag `v1.0.0`, create the release, add topics.

Creating a public repo is outward-facing. Do it without asking only when the user's request explicitly says to upload or publish; otherwise confirm once.

### 10. Verify
- install from GitHub: `claude plugin marketplace add <owner>/<repo>` and `claude plugin install <name>@<marketplace>`, then confirm the skill files exist in the plugin cache
- the banner URL returns 200 on raw.githubusercontent.com
- `CLAUDE.md` is a symlink on GitHub (its raw content is `AGENTS.md`)
- the release and tag exist

## Report

End with: repo URL, release URL, what the skill does in two lines, the conflict count, anything installed or renamed on the user's machine, and every judgment call you made instead of asking. Keep it short.
