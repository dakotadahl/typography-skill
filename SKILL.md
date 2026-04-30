---
name: typography
description: >
  Enforces typographic standards in user-facing strings, copy, and markup.
  Use when writing or editing UI text, marketing copy, headings, labels,
  paragraphs, tooltips, error messages, or any human-readable content in
  React/JSX, HTML, Markdown, or plain text files.
  Applies to Tailwind + React and general web projects.
---

# Typography

Apply every rule below whenever you write or edit user-facing text.

## Character rules

<!-- Add new character rules as siblings of these sections -->

### Apostrophes

Use the **curly (smart) apostrophe** for all contractions and possessives.

| Use | Avoid |
|-----|-------|
| `'` (U+2019) | `'` (U+0027) straight/typewriter apostrophe |

### Quotation marks

Use **curly (smart) quotes** for all quotations in user-facing strings.

| Use | Avoid |
|-----|-------|
| `"` `"` (U+201C / U+201D) | `"` (U+0022) straight double quotes |
| `'` `'` (U+2018 / U+2019) | `'` (U+0027) straight single quotes used as quotation marks |

**Opening vs. closing:** `'word'` needs left U+2018 before and right U+2019 after. A bulk `'` → `'` replacement handles contractions but breaks opening quotes. Fix quoted-word pairs first, then bulk-replace the rest.

### Em dashes

Use a **real em dash** with **no surrounding spaces**.

| Use | Avoid |
|-----|-------|
| `—` (U+2014) | `--`, ` -- `, ` — ` (hyphens or spaced em dashes) |

## How to audit

### 1. Determine scope

Check the target file **and any data/content files whose strings render in it** (e.g., a data module exporting titles, descriptions, or copy that the page imports). Grep for the target's imports to find these.

### 2. Detect violations

Run this Python script. It reports every violation with file, line number, and the offending text:

```sh
python3 -c "
import sys, re

files = sys.argv[1:]
for path in files:
    with open(path) as f:
        lines = f.readlines()
    for i, line in enumerate(lines, 1):
        # Skip pure-comment lines for em dash check (section dividers, etc.)
        stripped = line.lstrip()
        is_comment = stripped.startswith('//') or stripped.startswith('*') or stripped.startswith('/*')

        # Straight apostrophes in strings (between double-quote delimiters)
        for m in re.finditer(r'\"[^\"]*' + chr(0x27) + r'[^\"]*\"', line):
            print(f'{path}:{i}: straight apostrophe: {m.group()[:80]}')

        # Spaced em dashes in strings only
        if not is_comment:
            for m in re.finditer(r'\"[^\"]*' + ' — ' + r'[^\"]*\"', line):
                print(f'{path}:{i}: spaced em dash: {m.group()[:80]}')

        # Straight single quotes used as quotation marks: 'word' pattern in strings
        for m in re.finditer(r'\"[^\"]*' + chr(0x27) + r'\\w+' + chr(0x27) + r'[^\"]*\"', line):
            print(f'{path}:{i}: straight single quotes: {m.group()[:80]}')
" FILE1 FILE2 ...
```

### 3. Fix violations

**Critical: the Edit tool and shell string literals cannot reliably distinguish straight from curly quotes.** They look identical and silently fail. Always use Python with explicit Unicode code points:

```python
chr(0x27)    # ' straight apostrophe (what you're replacing)
chr(0x2018)  # ' left single quotation mark
chr(0x2019)  # ' right single quotation mark (also the curly apostrophe)
chr(0x201C)  # " left double quotation mark
chr(0x201D)  # " right double quotation mark
chr(0x2014)  # — em dash
```

Fix in this order to avoid clobbering:

1. **Quoted-word pairs first** — e.g., `'word'` → `'word'` (U+2018 + U+2019). Target each one individually since left vs. right depends on position.
2. **Bulk-replace remaining straight apostrophes** — all remaining `chr(0x27)` → `chr(0x2019)`. Safe because step 1 already handled opening quotes.
3. **Spaced em dashes** — `' — '` → `'—'` (remove flanking spaces). Only in strings, not section-divider comments.

Example fix script:

```sh
python3 -c "
with open('TARGET.ts', 'r') as f:
    content = f.read()

# Step 1: Fix specific quoted-word pairs (opening + closing)
content = content.replace(chr(0x27) + 'word' + chr(0x27), chr(0x2018) + 'word' + chr(0x2019))

# Step 2: Bulk-replace remaining straight apostrophes
content = content.replace(chr(0x27), chr(0x2019))

# Step 3: Unspace em dashes
content = content.replace(' — ', '—')

with open('TARGET.ts', 'w') as f:
    f.write(content)
"
```

After fixing, re-run the detection script to confirm zero violations. Then rebuild to verify no syntax was broken.

### Gotchas

- **Comments get caught by bulk replace.** The apostrophe replace is harmless in comments. The em dash replace can break section dividers like `// ─── Title — subtitle ───`. Restore any mangled comments manually, or scope the em dash fix to only lines containing `"` string delimiters.
- **JSX entities are fine.** `&rsquo;`, `&ldquo;`, `&rdquo;` are already correct — don't double-convert them.
- **Template literals** use backticks, not quotes. Apostrophes inside template literals are still `chr(0x27)` and need replacing.
