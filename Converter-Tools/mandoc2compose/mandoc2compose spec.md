# mandoc2compose

A converter from mandoc/mdoc to ｢‍｣ Lingenic Compose.

---

## Purpose

Convert Unix manual pages to Compose. mandoc is a semantic format—it describes *what* things are (arguments, flags, functions), not just how they look. This maps well to Compose.

---

## Usage

```
mandoc2compose input.1 -o output.compose
mandoc2compose input.3 -o output.compose    # section 3 (library functions)
cat manpage | mandoc2compose -              # stdin
mandoc2compose --section-headers input.1    # include section numbers
```

---

## Conversion Rules

### Document Structure

| mdoc | Compose |
|------|---------|
| `.Dd date` | `.set-reference DocumentDate "date"` |
| `.Dt NAME section` | `.set-reference ManSection "section"` `.set-reference ManTitle "NAME"` |
| `.Os` | `.set-reference OS "..."` |

### Sections

| mdoc | Compose |
|------|---------|
| `.Sh NAME` | `.begin-text-title` NAME `.end-text-title` |
| `.Sh SYNOPSIS` | `.begin-text-title` SYNOPSIS `.end-text-title` |
| `.Sh DESCRIPTION` | `.begin-text-title` DESCRIPTION `.end-text-title` |
| `.Sh OPTIONS` | `.begin-text-title` OPTIONS `.end-text-title` |
| `.Sh EXAMPLES` | `.begin-text-title` EXAMPLES `.end-text-title` |
| `.Sh SEE ALSO` | `.begin-text-title` SEE ALSO `.end-text-title` |
| `.Ss subsection` | `.begin-text-title` subsection `.end-text-title` (level 2) |

### Semantic Markup

#### Names and Emphasis

| mdoc | Compose |
|------|---------|
| `.Nm name` | `.font bold` name `.font -reset` |
| `.Nd description` | — description |
| `.Em text` | `.font italic` text `.font -reset` |
| `.Sy text` | `.font bold` text `.font -reset` |
| `.Li text` | `.font monospace` text `.font -reset` |
| `.No text` | text (normal) |

#### Arguments and Flags

| mdoc | Compose |
|------|---------|
| `.Fl flag` | `.font bold` -flag `.font -reset` |
| `.Cm command` | `.font bold` command `.font -reset` |
| `.Ar argument` | `.font italic` argument `.font -reset` |
| `.Op ...` | [ ... ] (optional) |
| `.Oo ... .Oc` | [ ... ] (optional, multi-line) |
| `.Ic command` | `.font bold` command `.font -reset` |

#### Paths and Files

| mdoc | Compose |
|------|---------|
| `.Pa /path/to/file` | `.font monospace` /path/to/file `.font -reset` |
| `.Ev ENVVAR` | `.font monospace` ENVVAR `.font -reset` |

#### Functions and Code

| mdoc | Compose |
|------|---------|
| `.Fn function args` | `.font bold` function `.font -reset` ( `.font italic` args `.font -reset` ) |
| `.Ft type` | `.font italic` type `.font -reset` |
| `.Fa argument` | `.font italic` argument `.font -reset` |
| `.Vt type` | `.font italic` type `.font -reset` |
| `.Dv CONSTANT` | `.font monospace` CONSTANT `.font -reset` |
| `.Er ERRNO` | `.font monospace` ERRNO `.font -reset` |
| `.Rv` | Standard return value text |

#### Cross References

| mdoc | Compose |
|------|---------|
| `.Xr name section` | `.font bold` name `.font -reset` (section) |

#### Quotations

| mdoc | Compose |
|------|---------|
| `.Dq text` | "text" |
| `.Sq text` | 'text' |
| `.Pq text` | (text) |
| `.Bq text` | [text] |
| `.Brq text` | {text} |
| `.Aq text` | ⟨text⟩ |
| `.Ql text` | `.font monospace` 'text' `.font -reset` |

### Lists

#### Bullet List

```mdoc
.Bl -bullet
.It
item one
.It
item two
.El
```

Converts to:

```
.indent-left 0.25i
.undent-left 0.25i
• item one
.undent-left 0.25i
• item two
.indent-left -0.25i
```

#### Numbered List

```mdoc
.Bl -enum
.It
first
.It
second
.El
```

Converts to:

```
.indent-left 0.25i
.undent-left 0.25i
1. first
.undent-left 0.25i
2. second
.indent-left -0.25i
```

#### Tag List (Definition List)

```mdoc
.Bl -tag -width "XXXX"
.It Fl v
Verbose mode.
.It Fl q
Quiet mode.
.El
```

Converts to:

```
.indent-left 0.5i
.undent-left 0.5i
.font bold
-v
.font -reset
.break-format
Verbose mode.
.undent-left 0.5i
.font bold
-q
.font -reset
.break-format
Quiet mode.
.indent-left -0.5i
```

#### Column List

```mdoc
.Bl -column "Header1" "Header2"
.It Header1 Ta Header2
.It value1 Ta value2
.El
```

Converts to table:

```
.table-define 2
.table-on
Header1 .table-column Header2
.break-format
value1 .table-column value2
.table-off
```

#### Other List Types

| mdoc | Compose |
|------|---------|
| `.Bl -dash` | Dash list (—) |
| `.Bl -hyphen` | Hyphen list (-) |
| `.Bl -item` | No markers |
| `.Bl -diag` | Diagnostic list |
| `.Bl -hang` | Hanging indent |
| `.Bl -ohang` | Overhanging |
| `.Bl -inset` | Inset labels |

### Display Blocks

| mdoc | Compose |
|------|---------|
| `.Bd -literal` | `.fill-off` |
| `.Ed` | `.fill-on` |
| `.Bd -filled` | (normal fill) |
| `.Bd -ragged` | `.align-left` |
| `.Bd -centered` | `.align-center` |
| `.Bd -offset indent` | `.indent-left 0.5i` |

### Code Examples

```mdoc
.Bd -literal -offset indent
code here
.Ed
```

Converts to:

```
.indent-left 0.5i
.fill-off
.font monospace
code here
.font -reset
.fill-on
.indent-left -0.5i
```

### Tables (tbl)

mandoc supports tbl preprocessor:

```mdoc
.Bl -column "Col1" "Col2" "Col3"
.It Col1 Ta Col2 Ta Col3
.It a Ta b Ta c
.El
```

Converts to Compose tables.

### Special Characters

| mdoc | Unicode |
|------|---------|
| `\(em` | — (em dash) |
| `\(en` | – (en dash) |
| `\(lq` | " |
| `\(rq` | " |
| `\(oq` | ' |
| `\(cq` | ' |
| `\(la` | ⟨ |
| `\(ra` | ⟩ |
| `\(bu` | • |
| `\(co` | © |
| `\(rg` | ® |
| `\(tm` | ™ |
| `\(Na` | NaN |
| `\(+-` | ± |
| `\(if` | ∞ |
| `\(!=` | ≠ |
| `\(<=` | ≤ |
| `\(>=` | ≥ |

### Escape Sequences

| mdoc | Result |
|------|--------|
| `\ ` | Non-breaking space |
| `\&` | Zero-width space |
| `\e` | Backslash |
| `\~` | Non-breaking space |
| `\%` | Optional hyphenation |
| `\:` | Zero-width break |
| `\fB` | Bold |
| `\fI` | Italic |
| `\fR` | Roman |
| `\fP` | Previous font |

### man (Legacy) Support

Also handle traditional man macros:

| man | Compose |
|-----|---------|
| `.TH name section date` | Document header |
| `.SH SECTION` | `.begin-text-title` SECTION `.end-text-title` |
| `.SS subsection` | Level 2 title |
| `.B text` | `.font bold` text `.font -reset` |
| `.I text` | `.font italic` text `.font -reset` |
| `.BI bold italic` | Alternating bold/italic |
| `.BR bold roman` | Alternating bold/roman |
| `.RB roman bold` | Alternating roman/bold |
| `.IR italic roman` | Alternating italic/roman |
| `.RI roman italic` | Alternating roman/italic |
| `.SM text` | Small text |
| `.SB text` | Small bold |
| `.TP` | Tagged paragraph |
| `.IP marker` | Indented paragraph |
| `.PP` or `.LP` | Paragraph |
| `.RS` | Relative indent start |
| `.RE` | Relative indent end |
| `.nf` | No fill |
| `.fi` | Fill |
| `.br` | Line break |
| `.sp` | Vertical space |

---

## Output Options

| Flag | Effect |
|------|--------|
| `--man` | Force legacy man macro parsing |
| `--mdoc` | Force mdoc parsing (default: auto-detect) |
| `--section-headers` | Include "Section N" in titles |
| `--no-see-also` | Omit SEE ALSO section |
| `--wrap=N` | Wrap output lines at N characters |

---

## Example

### Input (mdoc)

```mdoc
.Dd January 1, 2025
.Dt EXAMPLE 1
.Os
.Sh NAME
.Nm example
.Nd an example program
.Sh SYNOPSIS
.Nm
.Op Fl v
.Op Fl o Ar output
.Ar file ...
.Sh DESCRIPTION
The
.Nm
utility processes
.Ar file
arguments.
.Pp
Options:
.Bl -tag -width Ds
.It Fl v
Verbose output.
.It Fl o Ar output
Write to
.Ar output
instead of stdout.
.El
```

### Output (Compose)

```
.set-reference DocumentDate "January 1, 2025"
.set-reference ManSection "1"
.set-reference ManTitle "EXAMPLE"

.begin-text-title
NAME
.end-text-title

.font bold
example
.font -reset
— an example program

.begin-text-title
SYNOPSIS
.end-text-title

.font bold
example
.font -reset
[.font bold
-v
.font -reset
] [.font bold
-o
.font -reset
 .font italic
output
.font -reset
] .font italic
file ...
.font -reset

.begin-text-title
DESCRIPTION
.end-text-title

The .font bold
example
.font -reset
 utility processes .font italic
file
.font -reset
 arguments.

Options:

.indent-left 0.5i
.undent-left 0.5i
.font bold
-v
.font -reset
.break-format
Verbose output.
.undent-left 0.5i
.font bold
-o
.font -reset
 .font italic
output
.font -reset
.break-format
Write to .font italic
output
.font -reset
 instead of stdout.
.indent-left -0.5i
```

---

## Implementation Notes

### Parser

Options:
- Use `mandoc -T tree` to get AST, then convert
- Parse mdoc/man directly (grammar is documented)
- Use libmandoc if available

### Auto-Detection

- `.Dd` / `.Dt` / `.Os` → mdoc format
- `.TH` → legacy man format

### Semantic Preservation

mdoc is semantic (`.Fl` means "flag", `.Ar` means "argument"). Compose output can preserve this via templates:

```
.define-inline flag
  .font bold
  -%content%
  .font -reset
.end-define
```

This allows styling all flags consistently.

---

## License

Apache 2.0

---

*mandoc2compose: Unix manuals, readable format.*
