# Discovery

## Queries

`gh search repos` matches all words, so short queries work better than long ones. Run many in one shell call and print a compact table.

```bash
for q in "<topic> skill" "<topic> skills" "<topic>-skill" "claude <topic>" "<synonym> skill" "<target-language term>"; do
  echo "## $q"
  gh search repos "$q" --limit 8 --sort stars \
    --json fullName,stargazersCount,description,updatedAt \
    -q '.[]|"\(.stargazersCount)\t\(.fullName)\t\(.updatedAt[:10])\t\(.description[:100])"'
done
```

Also search:
- topics: `gh search repos --topic claude-skills --limit 50 --sort stars`, then grep descriptions
- large bundles that contain one relevant skill: list their tree with
  `gh api 'repos/<owner>/<repo>/git/trees/HEAD?recursive=1' --jq '.tree[].path' | grep 'SKILL.md$'`
- the target language: native words for the topic (for Korean, try 스킬, 윤문, 말투, 구어체, 후킹, 바이럴 plus the topic)

## Filters

Keep a repo when most of these hold:
- it contains a `SKILL.md` (or a plugin manifest with skills)
- it was updated in the last 6 months
- it has a license file
- its description matches the topic, not just a keyword

## Spam and low-quality signals

- star counts in the hundreds with near-zero forks and a creation date weeks ago
- several repos from one owner with similar star counts, all updated the same day
- descriptions that are pasted page text or unrelated to the code
- READMEs that are mostly marketing for a paid service, where the skill is a thin API wrapper (fine to include, but say it needs a paid key)

## Shortlist format

| Repo | ★ | Updated | License | Fit |
|---|---|---|---|---|
| owner/name | 125 | 2026-11-05 | MIT | one line on what it adds |

Note which candidates need paid API keys, which auto-publish, and which are video-only or another language.
