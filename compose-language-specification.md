# ｢‍｣ Lingenic Compose Language Specification

*Version 1.0*

---

## Overview

｢‍｣ Lingenic Compose is a document formatting language using verbose, self-documenting command names. Commands begin with a period (`.`) at the start of a line.

---

## Syntax

### Commands

```
.command-name [parameters]
```

### Comments

```
.* comment text
.~ comment text
```

### File Insertion (Short Form)

```
..filename
```

### Variable References

```
%variable-name%
```

### Inline Math

```
%math: expression %
```

### Inline Styles

```
.style[style-name]{content}
```

---

## Units

| Suffix | Unit | Scale |
|--------|------|-------|
| (none) | Points | 7200/inch |
| `i` | Inches | 1 |
| `c` | Centimeters | 2.54/inch |
| `p` | Picas | 6/inch |
| `l` | Lines | 12 points |
| `m` | Millipoints | 1/1000 point |

Values may be prefixed with `+` or `-` for relative adjustment.

---

## Core Commands

### Text Alignment

| Command | Description |
|---------|-------------|
| `.align-both` | Justify on both margins |
| `.align-center` | Center between margins |
| `.align-inside` | Align to inside margin |
| `.align-left` | Flush left |
| `.align-outside` | Align to outside margin |
| `.align-right` | Flush right |

### Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-block` | | Begin text block |
| `.begin-artwork` | [count] | Begin artwork mode |
| `.end-artwork` | | End artwork mode |

### Breaks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.break` | | Force block break |
| `.break-block` | | Force block break |
| `.break-column` | [column] | Break to column |
| `.break-format` | | Output current line |
| `.break-need` | [space] | Conditional break |
| `.break-page` | [options] | Force page break |
| `.break-skip` | | Break and skip |
| `.break-word` | [char] | Set word-break character |

### Change Bars

| Command | Parameters | Description |
|---------|------------|-------------|
| `.change-bars-addition` | [level] | Mark text as added |
| `.change-bars-deletion` | [level] | Mark deletion point |
| `.change-bars-modification` | [level] | Mark text as modified |
| `.change-bars-on` | [level] | Turn on change bars |
| `.change-bars-off` | [level] | Turn off change bars |

### Conditionals

| Command | Parameters | Description |
|---------|------------|-------------|
| `.if` | expression | Begin conditional |
| `.then` | [command] | Execute if true |
| `.else` | [command] | Execute if false |
| `.elseif` | expression | Test another condition |
| `.endif` | | End conditional |
| `.test` | expression | Single-line test |

### Delimiters

| Command | Parameters | Description |
|---------|------------|-------------|
| `.change-symbol-delimiter` | [char] | Change variable delimiter (default: %) |
| `.change-title-delimiter` | [char] | Change title field delimiter (default: \|) |

### Fill Mode

| Command | Parameters | Description |
|---------|------------|-------------|
| `.fill-default` | | Reset to default fill mode |
| `.fill-off` | [count] | Disable text wrapping |
| `.fill-on` | | Enable text wrapping |

### Fonts

| Command | Parameters | Description |
|---------|------------|-------------|
| `.font` | name \| -reset | Select font or reset |

### Footnotes

| Command | Parameters | Description |
|---------|------------|-------------|
| `.block-begin-footnote` | [options] | Begin footnote block |
| `.block-end-footnote` | | End footnote block |
| `.footnote-reference` | [n] | Insert footnote reference |
| `.footnotes-held` | | Hold footnotes |
| `.footnotes-paged` | | Output at page bottom |
| `.footnotes-running` | | Output as encountered |
| `.footnotes-unreferenced` | | Suppress reference marks |
| `.insert-footnotes` | | Insert held footnotes |

### Page Headers

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-page-header` | [indent] [type] | Begin page header |
| `.end-page-header` | | End page header |
| `.page-header-line` | text | Single-line page header |

### Page Footers

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-page-footer` | [indent] [type] | Begin page footer |
| `.end-page-footer` | | End page footer |
| `.page-footer-line` | text | Single-line page footer |

### Column Headers

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-column-header` | | Begin column header |
| `.end-column-header` | | End column header |
| `.column-header-line` | text | Single-line column header |

### Column Footers

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-column-footer` | | Begin column footer |
| `.end-column-footer` | | End column footer |
| `.column-footer-line` | text | Single-line column footer |

### Header/Footer Lines

| Command | Parameters | Description |
|---------|------------|-------------|
| `.header-line` | title-text | Header using title format |
| `.header-line-all` | title-text | Header for all pages |
| `.header-line-even` | title-text | Header for even pages |
| `.header-line-odd` | title-text | Header for odd pages |
| `.header-line-footnote` | title-text | Header for footnote section |
| `.footer-line` | title-text | Footer using title format |
| `.footer-line-all` | title-text | Footer for all pages |
| `.footer-line-even` | title-text | Footer for even pages |
| `.footer-line-odd` | title-text | Footer for odd pages |

### Header/Footer Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.header-block` | | Define header block |
| `.header-block-begin` | | Begin header block |
| `.header-block-end` | | End header block |
| `.footer-block` | | Define footer block |
| `.footer-block-begin` | | Begin footer block |
| `.footer-block-end` | | End footer block |

### Horizontal Tabs

| Command | Parameters | Description |
|---------|------------|-------------|
| `.horizontal-tabs-define` | name stops | Define tab pattern |
| `.horizontal-tabs-on` | char name | Enable tabs |
| `.horizontal-tabs-off` | [chars] | Disable tabs |

### Hyphenation

| Command | Parameters | Description |
|---------|------------|-------------|
| `.hyphenate-default` | | Reset hyphenation |
| `.hyphenate-off` | | Disable hyphenation |
| `.hyphenate-on` | [size] | Enable hyphenation |
| `.hyphenate-word` | word | Define hyphenation points |

### Indentation

| Command | Parameters | Description |
|---------|------------|-------------|
| `.indent` | [value] | Set left indent |
| `.indent-both` | [value] | Set both indents |
| `.indent-left` | [value] | Set left indent |
| `.indent-right` | [value] | Set right indent |
| `.indent-controls` | on\|off | Allow indented controls |

### Insertion

| Command | Parameters | Description |
|---------|------------|-------------|
| `.insert-file` | path | Insert file contents |
| `.insert-graphic` | path | Insert graphic |
| `.insert-block` | name | Insert named block |
| `.insert-index` | | Insert index entries |

### Keep Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.block-begin-keep` | | Begin keep block |
| `.block-end-keep` | | End keep block |

### Labels and Goto

| Command | Parameters | Description |
|---------|------------|-------------|
| `.label` | name | Define label |
| `.go-to` | label | Jump to label |
| `.return` | | Return from inserted file |

### Line Spacing

| Command | Parameters | Description |
|---------|------------|-------------|
| `.linespace` | [value] | Set line spacing |

### Literal Mode

| Command | Parameters | Description |
|---------|------------|-------------|
| `.block-begin-literal` | [count] | Begin literal block |
| `.block-end-literal` | | End literal block |

### Page Definition

| Command | Parameters | Description |
|---------|------------|-------------|
| `.page-define` | | Begin page definition |
| `.page-define-columns` | specs | Define columns |
| `.page-define-length` | [value] | Set page length |
| `.page-define-width` | [value] | Set page width |
| `.page-margin-left` | [odd] [even] | Set left margin |
| `.page-space` | | Reserved |

### Picture Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.block-begin-picture` | | Begin picture block |
| `.block-end-picture` | | End picture block |

### References and Variables

| Command | Parameters | Description |
|---------|------------|-------------|
| `.set-reference` | name [value] | Set variable |
| `.set-reference-counter` | name [value] | Set counter |
| `.set-reference-variable` | name [value] | Set variable |
| `.set-reference-mode` | mode names | Set display mode |
| `.use-reference` | | Process variable references |
| `.equation-count` | [value] | Set equation counter |

### Rules

| Command | Parameters | Description |
|---------|------------|-------------|
| `.horizontal-rule` | | Insert horizontal rule |
| `.vertical-rule` | | Insert vertical rule |

### Spacing

| Command | Parameters | Description |
|---------|------------|-------------|
| `.space` | [amount] | Add vertical space |
| `.space-break` | [amount] | Space with block break |
| `.space-to-depth` | [depth] | Space to absolute depth |
| `.space-format` | [amount] | Space with format break |
| `.space-total` | [amount] | Ensure total space |

### Tables

| Command | Parameters | Description |
|---------|------------|-------------|
| `.table-define` | specs | Define table format |
| `.table-column` | [column] | Switch to column |
| `.table-on` | | Enter table mode |
| `.table-off` | | Exit table mode |

### Text Titles and Captions

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-text-title` | | Begin text title |
| `.end-text-title` | | End text title |
| `.begin-text-caption` | | Begin text caption |
| `.end-text-caption` | | End text caption |
| `.text-header-line` | text | Single-line text header |
| `.text-caption-line` | text | Single-line caption |
| `.title-line-header` | text | Title line for headers |
| `.title-line-caption` | text | Title line for captions |
| `.text-title-line` | text | Single-line text title |

### Title Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.title-block` | | Begin title block |
| `.title-block-begin` | | Begin title block |
| `.title-block-end` | | End title block |
| `.split-title-line` | | Split title line |

### Translation

| Command | Parameters | Description |
|---------|------------|-------------|
| `.translate` | pairs | Define translation pairs |
| `.translate-formatted` | | Translate in output |
| `.translate-exceptions` | | Define exceptions |

### Underscoring

| Command | Parameters | Description |
|---------|------------|-------------|
| `.underscore-on` | | Begin underscoring |
| `.underscore-off` | | End underscoring |

### Undenting

| Command | Parameters | Description |
|---------|------------|-------------|
| `.undent` | [value] | Undent from left |
| `.undent-both` | [value] | Undent from both |
| `.undent-hanging` | | Hanging undent |
| `.undent-left` | [value] | Undent from left |
| `.undent-nobreak` | [value] | Undent without break |
| `.undent-right` | [value] | Undent from right |

### Vertical Alignment

| Command | Parameters | Description |
|---------|------------|-------------|
| `.vertical-align-bottom` | | Align to bottom |
| `.vertical-align-center` | | Center vertically |
| `.vertical-align-justified` | | Justify vertically |
| `.vertical-align-top` | | Align to top |

### Vertical Margins

| Command | Parameters | Description |
|---------|------------|-------------|
| `.vertical-margin-all` | [value] | Set all margins |
| `.vertical-margin-bottom` | [value] | Set bottom margin |
| `.vertical-margin-footer` | [value] | Set footer margin |
| `.vertical-margin-header` | [value] | Set header margin |
| `.vertical-margin-top` | [value] | Set top margin |

### Widow Control

| Command | Parameters | Description |
|---------|------------|-------------|
| `.widow` | | Set widow control |
| `.widow-footnote` | [lines] | Set footnote widow lines |
| `.widow-text` | [lines] | Set text widow lines |

### Writing Output

| Command | Parameters | Description |
|---------|------------|-------------|
| `.write-text` | path text | Write to file |
| `.write-formatted` | path text | Write formatted to file |
| `.write-order` | | Write order file |

### Miscellaneous

| Command | Parameters | Description |
|---------|------------|-------------|
| `.defer-until` | condition | Defer text |
| `.dump` | | Debugging dump |
| `.do` | | Begin do loop |
| `.enddo` | | End do loop |
| `.device-control` | | Device-specific control |
| `.error` | message | Generate error |
| `.execute` | command | Execute system command |
| `.galley-mode` | | Enter galley mode |
| `.hit` | | Create index entry |
| `.hit-file` | | Define hit file |
| `.read` | | Read from terminal |
| `.type` | text | Write to console |
| `.wait` | | Wait for signal |

---

## Built-in Variables

| Variable | Description |
|----------|-------------|
| `%PageNo%` | Current page number |
| `%Date%` | Current date |
| `%Time%` | Current time |
| `%InputFileName%` | Source file name |
| `%LineNo%` | Current line number |

---

## Block Templates

### Definition

| Command | Parameters | Description |
|---------|------------|-------------|
| `.define-block` | name [params] | Define block template |
| `.end-define` | | End definition |
| `.define-inline` | name [params] | Define inline template |
| `.redefine-block` | name | Replace template |
| `.extend-block` | name | Extend template |
| `.include-templates` | "file" [prefix=] | Load template library |

### Parameters

Declared in brackets with optional defaults:

| Syntax | Description |
|--------|-------------|
| `[name="default"]` | String with default |
| `[name=1i]` | Dimension with default |
| `[name=value]` | Keyword with default |
| `[name=true]` | Boolean with default |
| `[name]` | Required parameter |

### Special Variables

| Variable | Description |
|----------|-------------|
| `%content%` | Block content placeholder |
| `%param%` | Parameter reference |
| `defined(%param%)` | Parameter presence test |

---

## Style Classes

### Definition

| Command | Parameters | Description |
|---------|------------|-------------|
| `.define-style` | name [extends parent] | Define style |
| `.end-define` | | End definition |
| `.redefine-style` | name | Replace style |
| `.extend-style` | name | Add to style |
| `.default-style` | element | Set element default |
| `.include-styles` | "file" [prefix=] | Load style library |
| `.define-style-variable` | name value | Define variable |

### Application

| Command | Parameters | Description |
|---------|------------|-------------|
| `.apply-style` | name | Apply style to block |
| `.end-style` | | End styled block |
| `.style[name]{content}` | | Inline style |
| `style=name` | | Style attribute |

### Font Properties

| Property | Values |
|----------|--------|
| `.font` | name, bold, italic, monospace |
| `.font-family` | font name |
| `.font-size` | dimension or relative |
| `.font-weight` | normal, bold, 100-900 |
| `.font-style` | normal, italic, oblique |

### Color Properties

| Property | Values |
|----------|--------|
| `.color` | name or hex |
| `.background-color` | name or hex |

### Spacing Properties

| Property | Values |
|----------|--------|
| `.padding` | dimension |
| `.padding-left` | dimension |
| `.padding-right` | dimension |
| `.padding-top` | dimension |
| `.padding-bottom` | dimension |
| `.margin` | dimension |
| `.margin-left` | dimension |
| `.margin-right` | dimension |
| `.margin-top` | dimension |
| `.margin-bottom` | dimension |

### Border Properties

| Property | Values |
|----------|--------|
| `.border` | width style color |
| `.border-left` | width style color |
| `.border-right` | width style color |
| `.border-top` | width style color |
| `.border-bottom` | width style color |
| `.border-radius` | dimension |

### Text Properties

| Property | Values |
|----------|--------|
| `.text-align` | left, right, center, justify |
| `.text-decoration` | none, underline, strikethrough |
| `.text-transform` | none, uppercase, lowercase, capitalize |
| `.line-height` | dimension or multiplier |
| `.letter-spacing` | dimension |

### Conditionals

| Command | Parameters | Description |
|---------|------------|-------------|
| `.when-media` | type | Media conditional |
| `.when-page` | type | Page conditional |
| `.when-last` | | Last element conditional |
| `.when-hover` | | Hover conditional |
| `.end-when` | | End conditional |

---

## Mathematics

### Blocks

| Command | Parameters | Description |
|---------|------------|-------------|
| `.begin-math` | [label=] [number=] | Begin display equation |
| `.end-math` | | End display equation |
| `.begin-math-align` | | Begin aligned equations |
| `.end-math-align` | | End aligned equations |

### Inline

| Syntax | Description |
|--------|-------------|
| `%math: expression %` | Inline math |

### Structure

| Command | Parameters | Description |
|---------|------------|-------------|
| `.fraction` | | Begin fraction |
| `.numerator` | | Fraction numerator |
| `.denominator` | | Fraction denominator |
| `.end-fraction` | | End fraction |
| `.matrix` | [delimiters=] | Begin matrix |
| `.end-matrix` | | End matrix |
| `.cases` | | Begin cases |
| `.end-cases` | | End cases |
| `.root` | n | Begin nth root |
| `.end-root` | | End root |
| `.overbrace` | | Begin overbrace |
| `.underbrace` | | Begin underbrace |
| `.end-overbrace` | | End overbrace |
| `.end-underbrace` | | End underbrace |
| `.label` | text | Brace label |

### Matrix Delimiters

| Value | Description |
|-------|-------------|
| `parentheses` | ( ) |
| `brackets` | [ ] |
| `braces` | { } |
| `pipes` | \| \| |
| `double-pipes` | ‖ ‖ |
| `none` | No delimiters |

### Subscripts and Superscripts

| Syntax | Description |
|--------|-------------|
| `^(expression)` | Superscript |
| `_(expression)` | Subscript |
| Unicode superscripts | ⁰¹²³⁴⁵⁶⁷⁸⁹⁺⁻⁼⁽⁾ⁿⁱ |
| Unicode subscripts | ₀₁₂₃₄₅₆₇₈₉₊₋₌₍₎ₐₑₕᵢⱼₖₗₘₙₒₚᵣₛₜᵤᵥₓ |

### Typography (ISO 80000-2)

| Element | Style |
|---------|-------|
| Variables | Italic (𝑥, 𝑦, 𝑛) |
| Functions | Upright (sin, cos, log) |
| Constants | Upright (e, i, π) |

### Standard Functions

```
sin, cos, tan, cot, sec, csc
arcsin, arccos, arctan
sinh, cosh, tanh
log, ln, lg, exp
lim, sup, inf, max, min
det, tr, rank
gcd, lcm, mod
Re, Im, arg
```

### Operators

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| × | U+00D7 | Multiplication |
| ÷ | U+00F7 | Division |
| − | U+2212 | Minus |
| ± | U+00B1 | Plus-minus |
| ∓ | U+2213 | Minus-plus |
| · | U+00B7 | Dot product |
| ∘ | U+2218 | Composition |

### Relations

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| = | U+003D | Equals |
| ≠ | U+2260 | Not equal |
| < | U+003C | Less than |
| > | U+003E | Greater than |
| ≤ | U+2264 | Less or equal |
| ≥ | U+2265 | Greater or equal |
| ≪ | U+226A | Much less |
| ≫ | U+226B | Much greater |
| ≈ | U+2248 | Approximately |
| ∝ | U+221D | Proportional |
| ≡ | U+2261 | Equivalent |
| ≔ | U+2254 | Defined as |

### Set Theory

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| ∈ | U+2208 | Element of |
| ∉ | U+2209 | Not element of |
| ∋ | U+220B | Contains |
| ⊂ | U+2282 | Subset |
| ⊃ | U+2283 | Superset |
| ⊆ | U+2286 | Subset or equal |
| ∪ | U+222A | Union |
| ∩ | U+2229 | Intersection |
| ∅ | U+2205 | Empty set |

### Number Sets

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| ℕ | U+2115 | Natural numbers |
| ℤ | U+2124 | Integers |
| ℚ | U+211A | Rationals |
| ℝ | U+211D | Reals |
| ℂ | U+2102 | Complex |

### Arrows

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| → | U+2192 | Right arrow |
| ← | U+2190 | Left arrow |
| ↦ | U+21A6 | Maps to |
| ↔ | U+2194 | Bidirectional |
| ⇒ | U+21D2 | Implies |
| ⇔ | U+21D4 | If and only if |

### Large Operators

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| ∑ | U+2211 | Summation |
| ∏ | U+220F | Product |
| ∐ | U+2210 | Coproduct |
| ∫ | U+222B | Integral |
| ∬ | U+222C | Double integral |
| ∭ | U+222D | Triple integral |
| ∮ | U+222E | Contour integral |

### Other Symbols

| Symbol | Codepoint | Description |
|--------|-----------|-------------|
| √ | U+221A | Square root |
| ∛ | U+221B | Cube root |
| ∜ | U+221C | Fourth root |
| ∞ | U+221E | Infinity |
| ∂ | U+2202 | Partial |
| ∇ | U+2207 | Nabla |
| ∀ | U+2200 | For all |
| ∃ | U+2203 | Exists |
| ∴ | U+2234 | Therefore |
| ∵ | U+2235 | Because |

### Greek Letters

| Letter | Codepoint |
|--------|-----------|
| α | U+03B1 |
| β | U+03B2 |
| γ | U+03B3 |
| δ | U+03B4 |
| ε | U+03B5 |
| θ | U+03B8 |
| λ | U+03BB |
| μ | U+03BC |
| π | U+03C0 |
| σ | U+03C3 |
| φ | U+03C6 |
| ω | U+03C9 |
| Δ | U+0394 |
| Σ | U+03A3 |
| Π | U+03A0 |
| Ω | U+03A9 |

### Mathematical Italic

| Letter | Codepoint |
|--------|-----------|
| 𝑎 | U+1D44E |
| 𝑏 | U+1D44F |
| 𝑛 | U+1D45B |
| 𝑥 | U+1D465 |
| 𝑦 | U+1D466 |
| 𝑧 | U+1D467 |
| 𝑓 | U+1D453 |

### Delimiters

| Left | Right | Description |
|------|-------|-------------|
| ( | ) | Parentheses |
| [ | ] | Brackets |
| { | } | Braces |
| ⟨ | ⟩ | Angles |
| \| | \| | Pipes |
| ‖ | ‖ | Double pipes |
| ⌊ | ⌋ | Floor |
| ⌈ | ⌉ | Ceiling |

### Decorations

| Decoration | Method |
|------------|--------|
| Hat | x̂ (U+0302 combining) |
| Bar | x̄ (U+0304 combining) |
| Dot | ẋ (U+0307 combining) |
| Double dot | ẍ (U+0308 combining) |
| Tilde | x̃ (U+0303 combining) |
| Vector | x⃗ (U+20D7 combining) |

---

*｢‍｣ Lingenic Compose Language Specification v1.0*
