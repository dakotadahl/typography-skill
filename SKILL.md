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

### Quotation marks

Use **curly (smart) double quotes** for all quotations in user-facing strings.

| Use | Avoid |
|-----|-------|
| `"` (U+201C) and `"` (U+201D) | `"` (U+0022) straight/dumb quotes |

```jsx
// correct
<p>"This is a quote."</p>

// wrong
<p>"This is a quote."</p>
```

### Apostrophes

Use the **curly (smart) apostrophe** for all contractions and possessives.

| Use | Avoid |
|-----|-------|
| `'` (U+2019) | `'` (U+0027) straight/typewriter apostrophe |

```jsx
// correct
<p>It's Kody's project.</p>

// wrong
<p>It's Kody's project.</p>
```

### Em dashes

Use a **real em dash character** with no surrounding spaces.

| Use | Avoid |
|-----|-------|
| `—` (U+2014) | `--`, ` -- `, ` — ` (hyphens or spaced em dashes) |

```jsx
// correct
<p>Typography matters—every detail counts.</p>

// wrong
<p>Typography matters -- every detail counts.</p>
<p>Typography matters — every detail counts.</p>
```
