# Packaging

## Layout

```text
<repo>/
  skills/<name>/SKILL.md
  skills/<name>/references/*.md
  .claude-plugin/plugin.json
  .claude-plugin/marketplace.json
  assets/banner.png
  README.md
  README.ko.md
  AGENTS.md
  CLAUDE.md -> AGENTS.md        (ln -s AGENTS.md CLAUDE.md)
  ATTRIBUTIONS.md
  CHANGELOG.md
  LICENSE
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

## AGENTS.md template

```markdown
# AGENTS.md

Instructions for any coding agent working in this repository. `CLAUDE.md` is a symlink to this file; edit `AGENTS.md` only.

## What this repo is
<one paragraph + layout block>

## Rules for every change
1. Commit and push after every change. One logical change per commit. Push to `main` right after committing.
2. Release only for distribution changes (anything under `skills/`, `.claude-plugin/`, or a changed install command):
   bump the version one step in both manifests (patch: fixes and rule tweaks, minor: new modes or references,
   major: breaking renames), add a CHANGELOG entry, commit, push, then
   `git tag -a vX.Y.Z -m "vX.Y.Z" && git push origin vX.Y.Z && gh release create vX.Y.Z --title "vX.Y.Z" --notes-file <notes>`.
3. Docs-only changes (README, AGENTS.md, ATTRIBUTIONS.md, images, typos) are committed and pushed with no release.
4. Keep README.md and README.ko.md in sync.
5. `claude plugin validate .` must pass before committing a distribution change.

## Writing rules for the skill itself
<no copying, real examples, no private info, SKILL.md under 200 lines>
```

## README skeleton

Both files start with the banner and a language switch; the current language is bold, the other is a link.

```markdown
<p align="center"><img src="assets/banner.png" alt="<name> banner" width="100%"></p>
<p align="center"><b>English</b> · <a href="README.ko.md">한국어</a></p>

# <name>
**<one-line promise>**

<what it is, 2 sentences>
## Why it exists        the trial failure that motivated it
## What it does         modes table + checks
## Example              a real input and output
## Install              plugin, manual copy (Claude Code and Codex), npx skills add
## Usage
## Repository layout
## Credits              list of source repos, link to ATTRIBUTIONS.md
## License
```

## Banner

Generate with GPT image generation through the Codex CLI. Start it in the background at the beginning of packaging; it takes a few minutes.

```bash
mkdir -p <repo>/assets
codex exec --skip-git-repo-check --sandbox workspace-write -C <repo> \
  "Use your image generation tool to create ONE README hero banner image, wide landscape (about 1600x640). \
Concept: <what the skill does, as a visual metaphor>. Style: warm hand-drawn editorial illustration, cream paper, \
ink lines, one accent color (<color>). Put the word '<name>' in large clean lowercase lettering on the left. \
No other text, no logos, no brand names. Copy the final image to ./assets/banner.png and print the absolute path." \
  > <scratchpad>/banner.log 2>&1
sips -Z 1600 <repo>/assets/banner.png --out <repo>/assets/banner.png
```

Check first: `codex features list | grep image_generation` should say `true`. If Codex is unavailable but `OPENAI_API_KEY` is set, call the OpenAI Images API instead. If neither exists, ship without a banner and say so.

Always open the image and look at it before committing: no misspelled text, no brand logos, readable at README width.
