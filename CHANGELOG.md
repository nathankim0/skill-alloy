# Changelog

## Unreleased

## v1.1.0 (2026-11-06)

### Changed
- Packaging, READMEs, banner, AGENTS.md with the CLAUDE.md symlink, and releases now go through the [ossify](https://github.com/nathankim0/ossify) skill. skill-alloy installs it if missing.
- `references/packaging.md` keeps only skill-specific files (plugin manifests, ATTRIBUTIONS) and what to hand to ossify.
- `references/publishing.md` keeps only skill-specific checks (extra privacy terms, release-note extras, plugin verification).

## v1.0.0 (2026-11-06)

First release.

- `skill-alloy` workflow in ten steps: scope, discover, vet, install, trial, synthesize, package, privacy scan, publish, verify
- Mandatory trial step: invoke every source skill on a real task before merging
- Conflict table method for resolving contradictions between source skills
- Packaging templates: plugin and marketplace manifests, AGENTS.md with commit and release rules, CLAUDE.md symlink, English and Korean README skeletons
- Banner generation with GPT image generation through the Codex CLI
- Publishing and verification commands
