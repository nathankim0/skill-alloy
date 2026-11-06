# AGENTS.md

Instructions for any coding agent (Claude Code, Codex, others) working in this repository. `CLAUDE.md` is a symlink to this file; edit `AGENTS.md` only.

## What this repo is

`skill-alloy` is an agent skill that finds open-source skills on GitHub, vets and trials them, resolves their conflicts, and publishes one merged skill as a released repo. It is distributed as a Claude Code plugin and as a plain skill folder.

```text
skills/skill-alloy/SKILL.md            the skill (entry point)
skills/skill-alloy/references/*.md     loaded on demand by SKILL.md
.claude-plugin/plugin.json      plugin manifest (holds the version)
.claude-plugin/marketplace.json marketplace manifest (holds the version)
README.md / README.ko.md        English and Korean docs; keep them in sync
assets/banner.png               README hero image
CHANGELOG.md                    one entry per release
```

## Rules for every change

1. **Commit and push after every change.** One logical change per commit. Push to `main` right after committing. Do not leave work uncommitted at the end of a task.
2. **Release only for distribution changes.** A change is a distribution change when it alters what users install or how the skill behaves: anything under `skills/`, `.claude-plugin/`, or install instructions that change the install command. For these:
   - bump the version by one step (patch for fixes and rule tweaks, minor for new modes or references, major for breaking renames), in both `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
   - add a `CHANGELOG.md` entry
   - commit, push, then tag and release:
     ```bash
     git tag -a vX.Y.Z -m "vX.Y.Z"
     git push origin vX.Y.Z
     gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes>
     ```
3. **Docs-only changes do not release.** README edits, `AGENTS.md`, images, and typo fixes are committed and pushed with no version bump, tag, or release.
4. **Keep READMEs in sync.** Any change to `README.md` gets the same change in `README.ko.md`, and the reverse.
5. **Validate before committing** a distribution change: `claude plugin validate .` must pass.

## Writing rules for the skill itself

- Do not copy text from other projects.
- Examples must be real (for example, the `ssul` repo) or clearly marked as placeholders.
- No company names, internal project names, or private information in examples.
- Keep the workflow general: no assumptions about a specific topic, company, or language beyond English and Korean READMEs.
- `SKILL.md` stays under 200 lines; move detail into `references/`.

## Commit messages

Imperative, short subject. Example: `Add spam signals to discovery`.
