# rtf2compose

A converter from Rich Text Format to ｢‍｣ Lingenic Compose.

---

## Purpose

Convert RTF documents to Compose. RTF is a legacy interchange format still found in archives, legal documents, and older workflows.

---

## Usage

```
rtf2compose input.rtf -o output.compose
rtf2compose input.rtf                      # outputs to stdout
rtf2compose --strip-images input.rtf       # ignore embedded images
```

---

## Conversion Rules

### Document Properties

| RTF | Compose |
|-----|---------|
| `\title` | `.set-reference DocumentTitle "..."` |
| `\author` | `.set-reference DocumentAuthor "..."` |
| `\creatim` | `.set-reference DocumentDate "..."` |

### Page Setup

| RTF | Compose |
|-----|---------|
| `\paperw` | `.page-define-width` |
| `\paperh` | `.page-define-length` |
| `\margl` | `.page-margin-left` |
| `\margr` | (right margin) |
| `\margt` | `.vertical-margin-top` |
| `\margb` | `.vertical-margin-bottom` |

### Character Formatting

| RTF | Compose |
|-----|---------|
| `\b` | `.font bold` |
| `\b0` | `.font -reset` |
| `\i` | `.font italic` |
| `\i0` | `.font -reset` |
| `\ul` | `.underscore-on` |
| `\ul0` | `.underscore-off` |
| `\strike` | `.text-decoration strikethrough` |
| `\strike0` | `.text-decoration none` |
| `\f0`, `\f1`, etc. | `.font "fontname"` (from font table) |
| `\fs24` | `.font-size 12p` (RTF half-points → points) |
| `\cf1` | `.color` (from color table) |
| `\super` | (Unicode superscript) |
| `\sub` | (Unicode subscript) |
| `\scaps` | `.text-transform uppercase` |
| `\caps` | `.text-transform uppercase` |
| `\plain` | `.font -reset` |

### Paragraph Formatting

| RTF | Compose |
|-----|---------|
| `\par` | (paragraph break) |
| `\line` | `.break-format` |
| `\ql` | `.align-left` |
| `\qr` | `.align-right` |
| `\qc` | `.align-center` |
| `\qj` | `.align-both` |
| `\fi720` | `.indent 0.5i` (first line, twips → inches) |
| `\li720` | `.indent-left 0.5i` |
| `\ri720` | `.indent-right 0.5i` |
| `\sl240` | `.linespace 12p` |
| `\sb120` | `.space 6p` (space before) |
| `\sa120` | `.space 6p` (space after) |
| `\pard` | (reset paragraph formatting) |

### Sections

| RTF | Compose |
|-----|---------|
| `\sect` | `.break-page` |
| `\sectd` | (reset section formatting) |
| `\cols2` | `.page-define-columns` |
| `\colsx720` | (column gap) |

### Page Breaks

| RTF | Compose |
|-----|---------|
| `\page` | `.break-page` |
| `\column` | `.break-column` |

### Headers and Footers

| RTF | Compose |
|-----|---------|
| `\header` | `.begin-page-header` ... `.end-page-header` |
| `\footer` | `.begin-page-footer` ... `.end-page-footer` |
| `\headerl` | `.begin-page-header e` (even/left) |
| `\headerr` | `.begin-page-header o` (odd/right) |
| `\footerl` | `.begin-page-footer e` |
| `\footerr` | `.begin-page-footer o` |
| `\chpgn` | `%PageNo%` |
| `\chdate` | `%Date%` |
| `\chtime` | `%Time%` |

### Lists

| RTF | Compose |
|-----|---------|
| `\pntext` | List item marker |
| `\pn` | List properties |
| `\pnlvlblt` | Bullet list |
| `\pnlvlbody` | Numbered list |

Converts to:

```
.indent-left 0.25i
.undent-left 0.25i
• item
```

### Tables

| RTF | Compose |
|-----|---------|
| `\trowd` | Begin table row |
| `\cellx` | Cell boundary position |
| `\cell` | End cell |
| `\row` | End row |
| `\intbl` | Content is in table |
| `\lastrow` | Last row marker |

Converts to:

```
.table-define ...
.table-on
content .table-column content
.break-format
content .table-column content
.table-off
```

### Footnotes

| RTF | Compose |
|-----|---------|
| `\footnote` | `.block-begin-footnote` ... `.block-end-footnote` |
| `\chftn` | Footnote reference mark |

### Images

| RTF | Compose |
|-----|---------|
| `\pict` | `.insert-graphic` (extract to file) |
| `\pngblip` | PNG image data |
| `\jpegblip` | JPEG image data |
| `\wmetafile` | Windows metafile |
| `\emfblip` | EMF image data |
| `\picw`, `\pich` | Image dimensions |

Embedded images are extracted to separate files and referenced.

### Bookmarks

| RTF | Compose |
|-----|---------|
| `\bkmkstart name` | `.label name` |
| `\bkmkend name` | (ignored) |

### Hyperlinks

| RTF | Compose |
|-----|---------|
| `\field{\fldinst HYPERLINK "url"}{\fldrslt text}` | text (url) |

### Special Characters

| RTF | Unicode |
|-----|---------|
| `\'e9` | é (hex escape) |
| `\u233?` | é (Unicode escape) |
| `\emdash` | — |
| `\endash` | – |
| `\lquote` | ' |
| `\rquote` | ' |
| `\ldblquote` | " |
| `\rdblquote` | " |
| `\bullet` | • |
| `\tab` | (tab character) |
| `\~` | (non-breaking space) |
| `\_` | (non-breaking hyphen) |
| `\-` | (optional hyphen) |

### Math (OMML in RTF)

RTF can contain Office Math Markup Language (OMML). Convert using same rules as latex2compose:

| Structure | Compose |
|-----------|---------|
| Fraction | `(𝑎)/(𝑏)` |
| Radical | `√(𝑥)` |
| Subscript/superscript | Unicode or `^()`/`_()` |
| Greek | Unicode symbols |

---

## RTF Internals

### Control Words

RTF uses backslash-prefixed control words:
- `\b` — toggle bold
- `\fs24` — font size 24 half-points
- `\par` — paragraph break

### Groups

Braces define scope:
```rtf
{\b bold text}
```

Formatting reverts after closing brace.

### Units

| RTF Unit | Conversion |
|----------|------------|
| Twips | 1/1440 inch (20 twips = 1 point) |
| Half-points | 1/2 point |

### Font Table

```rtf
{\fonttbl
{\f0 Times New Roman;}
{\f1 Arial;}
}
```

Maps `\f0`, `\f1` to font names.

### Color Table

```rtf
{\colortbl
;\red255\green0\blue0;
\red0\green0\blue255;
}
```

Maps `\cf1`, `\cf2` to colors.

---

## Unsupported Constructs

| Construct | Reason |
|-----------|--------|
| OLE objects | Binary embedding |
| Drawing shapes | Complex vector graphics |
| Form fields | Interactive elements |
| Tracked changes | Revision history |
| Comments | Annotations |

Unsupported constructs are noted in comments:

```
.* UNCONVERTED: OLE object
```

---

## Output Options

| Flag | Effect |
|------|--------|
| `--strip-images` | Ignore embedded images |
| `--extract-images=DIR` | Extract images to directory |
| `--no-styles` | Flatten all formatting |
| `--wrap=N` | Wrap output lines at N characters |

---

## Example

### Input (RTF)

```rtf
{\rtf1\ansi
{\fonttbl{\f0 Times New Roman;}}
\f0\fs24
{\b The Quadratic Formula}\par
\par
For any quadratic equation {\i ax}{\super 2} + {\i bx} + {\i c} = 0 
where {\i a} \u8800? 0, the solutions are:\par
}
```

### Output (Compose)

```
.font "Times New Roman"
.font-size 12p
.font bold
The Quadratic Formula
.font -reset

For any quadratic equation %math: 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 % where %math: 𝑎 ≠ 0 %, the solutions are:
```

---

## Implementation Notes

### Parser

RTF is a well-documented format. Use existing parser:
- **Python**: `pyrtf-ng`, `striprtf`
- **JavaScript**: `rtf-parser`
- **C**: `unrtf`

Or implement custom parser—RTF grammar is straightforward.

### State Machine

RTF formatting is stateful. Track:
- Current font (from font table index)
- Current size
- Bold/italic/underline flags
- Indentation levels
- Table context

Emit Compose commands on state transitions.

### Encoding

RTF uses:
- 7-bit ASCII with escapes
- `\'XX` for 8-bit characters
- `\uNNNN?` for Unicode (with fallback character)

Decode all to UTF-8 for Compose output.

---

## License

Apache 2.0

---

*rtf2compose: Legacy documents, modern format.*
