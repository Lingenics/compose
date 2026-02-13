# md2compose

A converter from Markdown to ｢‍｣ Lingenic Compose.

---

## Purpose

Convert Markdown documents to Compose. Straightforward mapping—Markdown's simplicity makes this nearly 1:1.

---

## Usage

```
md2compose input.md -o output.compose
md2compose input.md                      # outputs to stdout
md2compose --flavor=gfm input.md         # GitHub-Flavored Markdown
md2compose --flavor=commonmark input.md  # strict CommonMark
```

---

## Conversion Rules

### Headings

| Markdown | Compose |
|----------|---------|
| `# Title` | `.set-reference SectionLevel 1` `.begin-text-title` Title `.end-text-title` |
| `## Title` | `.set-reference SectionLevel 2` `.begin-text-title` Title `.end-text-title` |
| `### Title` | `.set-reference SectionLevel 3` `.begin-text-title` Title `.end-text-title` |
| `#### Title` | `.set-reference SectionLevel 4` `.begin-text-title` Title `.end-text-title` |
| `##### Title` | `.set-reference SectionLevel 5` `.begin-text-title` Title `.end-text-title` |
| `###### Title` | `.set-reference SectionLevel 6` `.begin-text-title` Title `.end-text-title` |

Alternative syntax:

| Markdown | Compose |
|----------|---------|
| `Title` + `===` underline | Level 1 heading |
| `Title` + `---` underline | Level 2 heading |

### Text Formatting

| Markdown | Compose |
|----------|---------|
| `**bold**` | `.font bold` bold `.font -reset` |
| `__bold__` | `.font bold` bold `.font -reset` |
| `*italic*` | `.font italic` italic `.font -reset` |
| `_italic_` | `.font italic` italic `.font -reset` |
| `***bold italic***` | `.font bold` `.font italic` text `.font -reset` `.font -reset` |
| `` `code` `` | `.font monospace` code `.font -reset` |
| `~~strikethrough~~` | `.text-decoration strikethrough` text `.text-decoration none` |

### Paragraphs and Line Breaks

| Markdown | Compose |
|----------|---------|
| Blank line | `.space 12p` |
| Two trailing spaces + newline | `.break-format` |
| `<br>` | `.break-format` |

### Horizontal Rules

| Markdown | Compose |
|----------|---------|
| `---` | `.horizontal-rule` |
| `***` | `.horizontal-rule` |
| `___` | `.horizontal-rule` |

### Blockquotes

| Markdown | Compose |
|----------|---------|
| `> quoted text` | `.indent-left 0.5i` text `.indent-left -0.5i` |

Nested blockquotes increase indent.

### Lists

#### Unordered

| Markdown | Compose |
|----------|---------|
| `- item` | `.indent-left 0.25i` `.undent-left 0.25i` • item |
| `* item` | `.indent-left 0.25i` `.undent-left 0.25i` • item |
| `+ item` | `.indent-left 0.25i` `.undent-left 0.25i` • item |

#### Ordered

| Markdown | Compose |
|----------|---------|
| `1. item` | `.indent-left 0.25i` `.undent-left 0.25i` 1. item |
| `2. item` | `.indent-left 0.25i` `.undent-left 0.25i` 2. item |

#### Nested Lists

Increase indent by 0.25i per nesting level.

#### Task Lists (GFM)

| Markdown | Compose |
|----------|---------|
| `- [ ] unchecked` | `.indent-left 0.25i` ☐ unchecked |
| `- [x] checked` | `.indent-left 0.25i` ☑ checked |

### Code Blocks

#### Indented

```
    code here
```

Converts to:

```
.fill-off
.font monospace
code here
.font -reset
.fill-on
```

#### Fenced

````
```language
code here
```
````

Converts to:

```
.set-reference CodeLanguage "language"
.fill-off
.font monospace
code here
.font -reset
.fill-on
```

### Links

| Markdown | Compose |
|----------|---------|
| `[text](url)` | text (url) |
| `[text](url "title")` | text (url) |
| `<url>` | url |
| `[ref][id]` + `[id]: url` | text (url) |

Note: Compose is a print-oriented format. Links convert to visible URLs or footnotes depending on output target.

### Images

| Markdown | Compose |
|----------|---------|
| `![alt](path)` | `.insert-graphic path` |
| `![alt](path "title")` | `.insert-graphic path` `.begin-text-caption` title `.end-text-caption` |

### Tables (GFM)

```markdown
| A | B |
|---|---|
| 1 | 2 |
| 3 | 4 |
```

Converts to:

```
.table-define 2
.table-on
A .table-column B
.break-format
1 .table-column 2
.break-format
3 .table-column 4
.table-off
```

### Footnotes (Extended)

```markdown
Text with footnote[^1].

[^1]: Footnote content.
```

Converts to:

```
Text with footnote.block-begin-footnote
Footnote content.
.block-end-footnote
```

### Math (Extended)

#### Inline

| Markdown | Compose |
|----------|---------|
| `$expression$` | `%math: expression %` |

#### Block

````markdown
$$
expression
$$
````

Converts to:

```
.begin-math
expression
.end-math
```

Math content converts using the same rules as latex2compose (Greek letters → Unicode, operators → Unicode, etc.).

### HTML (Inline)

| Markdown | Compose |
|----------|---------|
| `<br>` | `.break-format` |
| `<hr>` | `.horizontal-rule` |
| `<b>text</b>` | `.font bold` text `.font -reset` |
| `<i>text</i>` | `.font italic` text `.font -reset` |
| `<code>text</code>` | `.font monospace` text `.font -reset` |
| `<sup>text</sup>` | (Unicode superscript if possible) |
| `<sub>text</sub>` | (Unicode subscript if possible) |

Other HTML is stripped with warning.

### Comments

```markdown
<!-- comment -->
```

Converts to:

```
.* comment
```

### YAML Front Matter

```markdown
---
title: Document Title
author: Author Name
date: 2025-01-01
---
```

Converts to:

```
.set-reference DocumentTitle "Document Title"
.set-reference DocumentAuthor "Author Name"
.set-reference DocumentDate "2025-01-01"
```

---

## Flavor Support

### CommonMark

Strict CommonMark parsing. No extensions.

### GFM (GitHub-Flavored Markdown)

Adds:
- Tables
- Task lists
- Strikethrough
- Autolinks
- Fenced code blocks with language

### Extended

Adds:
- Footnotes
- Math (`$...$` and `$$...$$`)
- Definition lists
- Abbreviations

---

## Output Options

| Flag | Effect |
|------|--------|
| `--flavor=X` | commonmark, gfm, extended (default: extended) |
| `--strip-html` | Remove all HTML (default: convert known tags) |
| `--footnotes=inline` | Convert links to inline parenthetical URLs |
| `--footnotes=endnote` | Collect links as endnotes |
| `--wrap=N` | Wrap output lines at N characters |
| `--no-front-matter` | Ignore YAML front matter |

---

## Example

### Input (Markdown)

```markdown
# The Quadratic Formula

For any quadratic equation $ax^2 + bx + c = 0$ where $a \neq 0$, the solutions are:

$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

## Properties

- Two solutions (real or complex)
- Symmetric about $x = -\frac{b}{2a}$

See [Wikipedia](https://en.wikipedia.org/wiki/Quadratic_formula) for more.
```

### Output (Compose)

```
.set-reference SectionLevel 1
.begin-text-title
The Quadratic Formula
.end-text-title

For any quadratic equation %math: 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 % where %math: 𝑎 ≠ 0 %, the solutions are:

.begin-math
𝑥 = (−𝑏 ± √(𝑏² − 4𝑎𝑐)) / (2𝑎)
.end-math

.set-reference SectionLevel 2
.begin-text-title
Properties
.end-text-title

.indent-left 0.25i
.undent-left 0.25i
• Two solutions (real or complex)
.undent-left 0.25i
• Symmetric about %math: 𝑥 = −𝑏/(2𝑎) %
.indent-left -0.25i

See Wikipedia (https://en.wikipedia.org/wiki/Quadratic_formula) for more.
```

---

## Implementation Notes

### Parser

Use established Markdown parser:
- **CommonMark**: `cmark`, `markdown-it`
- **GFM**: `github-markdown`, `remark-gfm`
- **Extended**: `pandoc`, `markdown-it` with plugins

### Math Handling

Delegate math content to the same conversion logic as latex2compose. Most Markdown math is LaTeX syntax.

### Link Handling

Markdown links are semantic. Compose is print-oriented. Options:
1. Inline: `text (url)`
2. Footnote: `text¹` with URL in footnote
3. Strip: `text` only

Default is inline for short URLs, footnote for long.

---

## License

Apache 2.0

---

*md2compose: From Markdown to readable.*
