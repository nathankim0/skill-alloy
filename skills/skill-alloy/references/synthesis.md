# Synthesis

## 1. Extract

For each source skill, list its rules as one-line claims, tagged with the source:

```text
[threads-skills] uniform rhythm wins; never chop sentences
[storytelling]   vary sentence length (jagged edge)
[gueoche]        no same ending 4 times in a row
```

## 2. Group and find conflicts

Group claims by topic (rhythm, framing, formatting, register, safety, output). Inside each group, a conflict is two rules that would produce different edits on the same input. Common conflict families:

- **style vs truth**: "add specifics" vs "never invent"
- **engagement vs bait**: "end with a question" vs "no engagement bait"
- **structure vs weight**: frameworks add setup and lists; users want lighter output
- **format**: lists, bold, emoji, hashtags allowed or banned
- **register / language**: formulas written in English vs natural target-language style
- **autonomy**: auto-publish, auto-run hooks vs explicit approval
- **output size**: full audit reports vs just the deliverable

## 3. Rule on each

Write `references/conflicts.md` as a table:

| # | Conflict | Ruling |
|---|---|---|
| 1 | **Short name.** Source A says X. Source B says Y. | What the merged skill does, and the condition that decides it. |

Rulings should be conditional when that resolves the conflict ("allowed only when Y is concrete"), not a vote between sources. Use trial findings (Step 5) as the tiebreaker: whichever rule fixed the real failure wins.

## 4. Write the skill

`SKILL.md` structure:

```markdown
---
name: <name>
description: <what it does, then "Use when ..." with real trigger phrases in every language users will type>
argument-hint: "[...]"
---

# <name>
<two sentences: the goal, and the core lesson from the trial>

## Modes            table: request → mode → output
## Step 1..N        ordered, each step one job, pointers to references/
## Output           exactly what the user gets; what never happens (publishing, etc.)
```

Rules:
- under 200 lines; detail goes into `references/*.md`
- every rule checkable (someone could say pass or fail)
- every example real, or marked as a placeholder
- write native-language examples natively, not by translation
- no company names, internal names, or private details

## 5. Self-test

Run the new skill on the same trial task from Step 5 and compare with the source skills' output. If it does not fix the failures you recorded, revise before packaging.
