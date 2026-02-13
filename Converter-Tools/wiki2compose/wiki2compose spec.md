# wiki2compose

A converter from Wiki markup to ｢‍｣ Lingenic Compose.

---

## Purpose

Convert MediaWiki and other wiki formats to Compose. Wikis contain vast amounts of structured content—Wikipedia alone has 60+ million articles.

---

## Usage

```
wiki2compose input.wiki -o output.compose
wiki2compose --flavor=mediawiki input.txt
wiki2compose --flavor=creole input.txt
wiki2compose --flavor=dokuwiki input.txt
```

---

## Supported Flavors

| Flavor | Source |
|--------|--------|
| `mediawiki` | Wikipedia, MediaWiki (default) |
| `creole` | WikiCreole standard |
| `dokuwiki` | DokuWiki |
| `tiddlywiki` | TiddlyWiki |
| `confluence` | Atlassian Confluence |

---

## Conversion Rules (MediaWiki)

### Headings

| Wiki | Compose |
|------|---------|
| `= H1 =` | `.set-reference SectionLevel 1` `.begin-text-title` H1 `.end-text-title` |
| `== H2 ==` | `.set-reference SectionLevel 2` `.begin-text-title` H2 `.end-text-title` |
| `=== H3 ===` | `.set-reference SectionLevel 3` `.begin-text-title` H3 `.end-text-title` |
| `==== H4 ====` | `.set-reference SectionLevel 4` `.begin-text-title` H4 `.end-text-title` |
| `===== H5 =====` | `.set-reference SectionLevel 5` `.begin-text-title` H5 `.end-text-title` |
| `====== H6 ======` | `.set-reference SectionLevel 6` `.begin-text-title` H6 `.end-text-title` |

### Text Formatting

| Wiki | Compose |
|------|---------|
| `'''bold'''` | `.font bold` bold `.font -reset` |
| `''italic''` | `.font italic` italic `.font -reset` |
| `'''''bold italic'''''` | `.font bold` `.font italic` text `.font -reset` `.font -reset` |
| `<u>underline</u>` | `.underscore-on` underline `.underscore-off` |
| `<s>strike</s>` | `.text-decoration strikethrough` strike `.text-decoration none` |
| `<code>code</code>` | `.font monospace` code `.font -reset` |
| `<tt>teletype</tt>` | `.font monospace` teletype `.font -reset` |
| `<small>small</small>` | `.font-size 0.85em` small `.font-size -reset` |
| `<big>big</big>` | `.font-size 1.2em` big `.font-size -reset` |
| `<sup>super</sup>` | (Unicode superscript) |
| `<sub>sub</sub>` | (Unicode subscript) |

### Paragraphs and Breaks

| Wiki | Compose |
|------|---------|
| Blank line | Paragraph break |
| `<br>` or `<br/>` | `.break-format` |
| `----` | `.horizontal-rule` |

### Lists

#### Unordered

```wiki
* Item one
* Item two
** Nested item
* Item three
```

Converts to:

```
.indent-left 0.25i
.undent-left 0.25i
• Item one
.undent-left 0.25i
• Item two
.indent-left 0.25i
.undent-left 0.25i
• Nested item
.indent-left -0.25i
.undent-left 0.25i
• Item three
.indent-left -0.25i
```

#### Ordered

```wiki
# First
# Second
## Nested
# Third
```

Converts to numbered list with nesting.

#### Definition List

```wiki
; Term
: Definition
; Another term
: Another definition
```

Converts to:

```
.indent-left 0.5i
.undent-left 0.5i
.font bold
Term
.font -reset
.break-format
Definition
.undent-left 0.5i
.font bold
Another term
.font -reset
.break-format
Another definition
.indent-left -0.5i
```

#### Mixed Lists

```wiki
* Bullet
*# Numbered inside bullet
*# Another numbered
* Back to bullet
```

Nesting preserved via indent levels.

### Links

| Wiki | Compose |
|------|---------|
| `[[Page]]` | Page |
| `[[Page|display text]]` | display text |
| `[[Page#Section]]` | Page § Section |
| `[https://example.com]` | (https://example.com) |
| `[https://example.com text]` | text (https://example.com) |
| `[[File:Image.png]]` | `.insert-graphic Image.png` |
| `[[Category:Name]]` | `.set-reference Category "Name"` |

### Images

| Wiki | Compose |
|------|---------|
| `[[File:Name.jpg]]` | `.insert-graphic Name.jpg` |
| `[[File:Name.jpg|thumb]]` | `.insert-graphic Name.jpg` (thumbnail) |
| `[[File:Name.jpg|200px]]` | `.insert-graphic Name.jpg` (sized) |
| `[[File:Name.jpg|right]]` | `.insert-graphic Name.jpg` (aligned) |
| `[[File:Name.jpg|caption text]]` | `.insert-graphic Name.jpg` `.begin-text-caption` caption text `.end-text-caption` |

### Tables

```wiki
{| class="wikitable"
|+ Caption
|-
! Header 1 !! Header 2
|-
| Cell 1 || Cell 2
|-
| Cell 3 || Cell 4
|}
```

Converts to:

```
.begin-text-caption
Caption
.end-text-caption
.table-define 2
.table-on
.font bold
Header 1
.font -reset
.table-column
.font bold
Header 2
.font -reset
.break-format
Cell 1 .table-column Cell 2
.break-format
Cell 3 .table-column Cell 4
.table-off
```

### Preformatted Text

| Wiki | Compose |
|------|---------|
| ` text` (leading space) | `.fill-off` text `.fill-on` |
| `<pre>text</pre>` | `.fill-off` `.font monospace` text `.font -reset` `.fill-on` |
| `<nowiki>text</nowiki>` | text (literal, no parsing) |
| `<syntaxhighlight>` | `.fill-off` `.font monospace` ... |

### Blockquotes

```wiki
<blockquote>
Quoted text here.
</blockquote>
```

Converts to:

```
.indent-both 0.5i
Quoted text here.
.indent-both -0.5i
```

### Templates

| Wiki | Compose |
|------|---------|
| `{{TemplateName}}` | `.* TEMPLATE: TemplateName` |
| `{{TemplateName|arg1|arg2}}` | `.* TEMPLATE: TemplateName (arg1, arg2)` |
| `{{{parameter}}}` | `%parameter%` |

Templates are noted as comments—full expansion requires access to template definitions.

### Magic Words

| Wiki | Compose |
|------|---------|
| `__TOC__` | `.* TOC placeholder` |
| `__NOTOC__` | (ignored) |
| `__FORCETOC__` | (ignored) |
| `{{PAGENAME}}` | `%PageName%` |
| `{{CURRENTDATE}}` | `%Date%` |
| `{{CURRENTTIME}}` | `%Time%` |

### References/Footnotes

```wiki
Text with reference.<ref>Reference content.</ref>

== References ==
{{reflist}}
```

Converts to:

```
Text with reference.block-begin-footnote
Reference content.
.block-end-footnote

.begin-text-title
References
.end-text-title
.insert-footnotes
```

### Math

```wiki
<math>x = \frac{-b \pm \sqrt{b^2-4ac}}{2a}</math>
```

Converts using latex2compose rules:

```
.begin-math
𝑥 = (−𝑏 ± √(𝑏² − 4𝑎𝑐)) / (2𝑎)
.end-math
```

Inline math `<math>...</math>` converts to `%math: ... %`.

### Comments

| Wiki | Compose |
|------|---------|
| `<!-- comment -->` | `.* comment` |

### Redirects

| Wiki | Compose |
|------|---------|
| `#REDIRECT [[Target]]` | `.* REDIRECT: Target` |

### Categories and Metadata

| Wiki | Compose |
|------|---------|
| `[[Category:Name]]` | `.set-reference Category "Name"` |
| `{{DEFAULTSORT:Key}}` | `.set-reference SortKey "Key"` |

### Interwiki Links

| Wiki | Compose |
|------|---------|
| `[[wikipedia:Article]]` | Article (Wikipedia) |
| `[[wikt:Word]]` | Word (Wiktionary) |

---

## Other Flavors

### Creole

| Creole | MediaWiki Equivalent |
|--------|---------------------|
| `**bold**` | `'''bold'''` |
| `//italic//` | `''italic''` |
| `= H1` | `= H1 =` |
| `== H2` | `== H2 ==` |
| `[[link]]` | `[[link]]` |
| `{{image.png}}` | `[[File:image.png]]` |
| `{{{nowiki}}}` | `<nowiki>nowiki</nowiki>` |
| `\\` | Line break |

### DokuWiki

| DokuWiki | MediaWiki Equivalent |
|----------|---------------------|
| `**bold**` | `'''bold'''` |
| `//italic//` | `''italic''` |
| `__underline__` | `<u>underline</u>` |
| `''monospace''` | `<code>monospace</code>` |
| `====== H1 ======` | `= H1 =` |
| `===== H2 =====` | `== H2 ==` |
| `[[link|text]]` | `[[link|text]]` |
| `{{image.png}}` | `[[File:image.png]]` |
| `<code>` | `<syntaxhighlight>` |

### Confluence

| Confluence | MediaWiki Equivalent |
|------------|---------------------|
| `*bold*` | `'''bold'''` |
| `_italic_` | `''italic''` |
| `{{monospace}}` | `<code>monospace</code>` |
| `h1. Heading` | `= Heading =` |
| `[text|url]` | `[url text]` |
| `!image.png!` | `[[File:image.png]]` |
| `{code}...{code}` | `<pre>...</pre>` |
| `{quote}...{quote}` | `<blockquote>...</blockquote>` |

---

## Output Options

| Flag | Effect |
|------|--------|
| `--flavor=X` | mediawiki, creole, dokuwiki, confluence |
| `--expand-templates=FILE` | Load template definitions for expansion |
| `--strip-categories` | Remove category assignments |
| `--strip-interwiki` | Remove interwiki links |
| `--resolve-redirects=FILE` | Follow redirects using map file |
| `--wrap=N` | Wrap output lines at N characters |

---

## Example

### Input (MediaWiki)

```wiki
= The Quadratic Formula =

For any '''quadratic equation''' <math>ax^2 + bx + c = 0</math> where <math>a \neq 0</math>, the solutions are:

<math>x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}</math>

== Properties ==

* Two solutions (real or complex)
* Symmetric about <math>x = -\frac{b}{2a}</math>

== See also ==

* [[Cubic equation]]
* [[Quartic equation]]

== References ==
<ref>Standard algebra textbook</ref>
{{reflist}}

[[Category:Algebra]]
[[Category:Equations]]
```

### Output (Compose)

```
.set-reference SectionLevel 1
.begin-text-title
The Quadratic Formula
.end-text-title

For any .font bold
quadratic equation
.font -reset
 %math: 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 % where %math: 𝑎 ≠ 0 %, the solutions are:

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

.set-reference SectionLevel 2
.begin-text-title
See also
.end-text-title

.indent-left 0.25i
.undent-left 0.25i
• Cubic equation
.undent-left 0.25i
• Quartic equation
.indent-left -0.25i

.set-reference SectionLevel 2
.begin-text-title
References
.end-text-title
.block-begin-footnote
Standard algebra textbook
.block-end-footnote
.insert-footnotes

.set-reference Category "Algebra"
.set-reference Category "Equations"
```

---

## Implementation Notes

### Parser

Options:
- **mwparserfromhell** (Python) — MediaWiki-specific
- **Parsoid** — MediaWiki's official parser
- **Custom PEG grammar** — For multi-flavor support

### Template Handling

Templates are wiki-specific and often complex. Options:

1. **Stub** — Note as comment, don't expand
2. **Map file** — Simple template → Compose mappings
3. **Full expansion** — Requires template database

Default is stub. Full expansion requires `--expand-templates`.

### Nested Formatting

Wiki allows nesting: `'''bold ''and italic'' text'''`

Track state stack, emit Compose commands on transitions.

---

## License

Apache 2.0

---

*wiki2compose: From wiki to readable.*
