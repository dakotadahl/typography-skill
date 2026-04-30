# Typography Skill

A [Claude Code](https://claude.com/claude-code) skill that enforces typographic standards in user-facing text.

Auto-invokes when Claude detects work on UI copy, headings, labels, error messages, or any human-readable content. Can also be triggered manually with `/typography`.

## Install

Clone into your Claude Code skills directory:

```sh
git clone https://github.com/dakotadahl/typography-skill.git ~/.claude/skills/typography
```

## Rules

### Character rules

| Rule | Use | Avoid |
|------|-----|-------|
| **Quotation marks** | `"` `"` (U+201C, U+201D) | `"` (U+0022) straight quotes |
| **Apostrophes** | `'` (U+2019) curly | `'` (U+0027) straight |
| **Em dashes** | `—` (U+2014) no spaces | `--`, ` -- `, ` — ` |

## Extending

Add new rules to `SKILL.md`. Character rules go as `###` sections under `## Character rules`. Broader topics (type scale, font pairing, vertical rhythm) go as new `##` sections.
