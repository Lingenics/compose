# Multics Compose Control Commands Reference

Complete reference for all 187 control commands (dot commands) in the Multics Compose text formatting system.

## Command Syntax

Control commands begin with a period (`.`) at the start of a line:

```
.command [parameters]
```

Comments use `.*` or `.~`:
```
.* This is a comment
.~ This is also a comment
```

Short form for file insertion:
```
..filename    (equivalent to .ifi filename)
```

## Units and Measurements

Numeric parameters accept various units:

| Suffix | Unit | Scale |
|--------|------|-------|
| (none) | Points | 7200 per inch |
| `i` | Inches | 1 inch |
| `c` | Centimeters | 2.54 per inch |
| `p` | Picas | 6 per inch |
| `l` | Lines | 12 points |
| `m` | Millipoints | 1/1000 point |

Values can be absolute or relative (`+` or `-` prefix).

---

## Text Alignment

Control how text is aligned within the measure.

### `.alb` / `.align-both`
Justify text on both margins (full justification).

### `.alc` / `.align-center`
Center text between margins.

### `.ali` / `.align-inside`
Align text to inside margin (binding edge).

### `.all` / `.align-left`
Align text flush left (ragged right).

### `.alo` / `.align-outside`
Align text to outside margin.

### `.alr` / `.align-right`
Align text flush right (ragged left).

---

## Block Controls

Blocks are logical units of text with associated formatting.

### `.bblk` / `.begin-block`
Begin a new text block. Ends any current block and resets block modes (art, keep, title, literal).

### `.bart` / `.begin-artwork` [count]
Begin artwork mode. Lines are treated as fixed-position text.
- `count` — number of artwork lines (-1 = until `.eart`)

### `.eart` / `.end-artwork`
End artwork mode.

---

## Break Controls

Breaks force output and control page/column flow.

### `.br` / `.break`
Synonym for `.brb` (break-block).

### `.brb` / `.break-block`
Force a block break. Outputs accumulated text and ends the current block.

### `.brc` / `.break-column` [column]
Break to a new column.
- `column` — target column number (default: next column)
- In single-column mode, equivalent to page break

### `.brf` / `.break-format`
Force a format break. Outputs the current line without ending the block.

### `.brn` / `.break-need` [space]
Conditional break. Forces a page/column break if insufficient space remains.
- `space` — space needed (default: 1 line)

### `.brp` / `.break-page` [options]
Force a page break.
- Can specify new page number and format

### `.brs` / `.break-skip`
Break and skip (not implemented in this version).

### `.brw` / `.break-word` [char]
Set word-break character.
- `char` — character that can break words (default: space)

---

## Change Bars

Mark text changes with marginal bars.

### `.cba` / `.change-bars-addition` [level]
Mark following text as added.
- `level` — change level (single character, optional)

### `.cbd` / `.change-bars-deletion` [level]
Mark a deletion point.

### `.cbf` / `.change-bars-off` [level]
Turn off change bars.

### `.cbm` / `.change-bars-modification` [level]
Mark following text as modified.

### `.cbn` / `.change-bars-on` [level]
Turn on change bars (synonym for `.cbm`).

---

## Conditional Execution

Control flow based on conditions.

### `.if` expression
Begin conditional block. Evaluates expression as logical.

### `.then` [command]
Execute if condition is true.
- Can include inline command or begin block

### `.else` [command]
Execute if condition is false.

### `.elseif` expression
Test another condition within an if block.

### `.endif`
End conditional block.

### `.ts` / `.test` expression
Single-line test. Skips next line if expression is false.

**Example:**
```
.if %PageNo% > 10
.then Header text for later pages
.else Header text for early pages
.endif
```

---

## Delimiters

### `.csd` / `.change-symbol-delimiter` [char]
Change the symbol reference delimiter.
- Default: `%`

### `.ctd` / `.change-title-delimiter` [char]
Change the title field delimiter.
- Default: `|`

---

## Fill Mode

Control text filling (word wrapping).

### `.fi` / `.fill-default`
Reset to default fill mode (from command line option).

### `.fif` / `.fill-off` [count]
Turn off filling. Text is output as entered.
- `count` — number of unfilled lines (-1 = until `.fin`)

### `.fin` / `.fill-on`
Turn on filling. Text is wrapped to fit measure.

---

## Fonts

### `.fnt` / `.font` name | `-reset`
Select a named font or reset to default.

**Example:**
```
.fnt bold
.fnt -reset
```

---

## Footnotes

### `.bbf` / `.block-begin-footnote` [options]
Begin a footnote block.
- `s` or `u` — suppress reference mark
- `c` — column footnote (default)
- `p` — page footnote

### `.bef` / `.block-end-footnote`
End footnote block.

### `.frf` / `.footnote-reference` [n]
Insert a reference to footnote n (default: most recent).

### `.fth` / `.footnotes-held`
Hold footnotes until explicitly inserted with `.ift`.

### `.ftp` / `.footnotes-paged`
Output footnotes at bottom of current page.

### `.ftr` / `.footnotes-running`
Output footnotes at bottom of each page as encountered.

### `.ftu` / `.footnotes-unreferenced`
Create footnotes without automatic reference marks.

### `.ift` / `.insert-footnotes`
Insert all held footnotes.

---

## Headers and Footers

Page headers and footers are blocks that repeat on each page.

### Page Headers

#### `.bph` / `.begin-page-header` [indent] [type]
Begin page header definition block.
- `indent` — left indent
- `type` — `a` (all pages), `e` (even), `o` (odd)

#### `.eph` / `.end-page-header`
End page header block.

#### `.phl` / `.page-header-line`
Define a single-line page header.

### Page Footers

#### `.bpf` / `.begin-page-footer` [indent] [type]
Begin page footer definition block.

#### `.epf` / `.end-page-footer`
End page footer block.

#### `.pfl` / `.page-footer-line`
Define a single-line page footer.

### Column Headers

#### `.bch` / `.begin-column-header`
Begin column header definition.

#### `.ech` / `.end-column-header`
End column header block.

#### `.chl` / `.column-header-line`
Define single-line column header.

### Column Footers

#### `.bcf` / `.begin-column-footer`
Begin column footer definition.

#### `.ecf` / `.end-column-footer`
End column footer block.

#### `.cfl` / `.column-footer-line`
Define single-line column footer.

### Header/Footer Lines

#### `.hl` / `.header-line` title-text
Define header line using title format (`|left|center|right|`).

#### `.hla` / `.header-line-all`
Header for all pages.

#### `.hle` / `.header-line-even`
Header for even pages only.

#### `.hlo` / `.header-line-odd`
Header for odd pages only.

#### `.hlf` / `.header-line-footnote`
Header for footnote section.

#### `.fl` / `.footer-line` title-text
Define footer line using title format.

#### `.fla` / `.footer-line-all`
Footer for all pages.

#### `.fle` / `.footer-line-even`
Footer for even pages only.

#### `.flo` / `.footer-line-odd`
Footer for odd pages only.

---

## Header/Footer Blocks

### `.hb` / `.header-block`
Define a header block.

### `.hbb` / `.header-block-begin`
Begin header block.

### `.hbe` / `.header-block-end`
End header block.

### `.fb` / `.footer-block`
Define a footer block.

### `.fbb` / `.footer-block-begin`
Begin footer block.

### `.fbe` / `.footer-block-end`
End footer block.

---

## Horizontal Tabs

### `.htd` / `.horizontal-tabs-define` name stops...
Define a horizontal tab pattern.
- `name` — pattern name
- `stops` — comma-separated column positions with optional fill strings

**Example:**
```
.htd mytabs 1i, 3i ".", 5i
```

### `.htf` / `.horizontal-tabs-off` [chars]
Disable horizontal tabs.
- `chars` — specific tab characters to disable (default: all)

### `.htn` / `.horizontal-tabs-on` char name|pattern
Enable horizontal tabs.
- `char` — character that triggers tab
- `name` — pattern name or inline pattern definition

---

## Hyphenation

### `.hy` / `.hyphenate-default`
Reset to default hyphenation mode.

### `.hyf` / `.hyphenate-off`
Disable hyphenation.

### `.hyn` / `.hyphenate-on` [size]
Enable hyphenation.
- `size` — minimum word size for hyphenation

### `.hyw` / `.hyphenate-word` word...
Define explicit hyphenation points for words.

---

## Indentation

### `.in` / `.indent` [value]
Set left indent.
- `value` — indent from left margin (default: 0)

### `.inb` / `.indent-both` [value]
Set both left and right indent equally.

### `.inl` / `.indent-left` [value]
Set left indent (synonym for `.in`).

### `.inr` / `.indent-right` [value]
Set right indent.

### `.indctl` / `.indent-controls` on|off
Allow indented control lines.
- When on, control lines can be preceded by spaces

---

## Insertion Controls

### `.ifi` / `.insert-file` path
Insert contents of another file.
- Short form: `..path`

### `.igr` / `.insert-graphic` path
Insert a graphic element.

### `.ibl` / `.insert-block`
Insert a named block (not implemented).

### `.indx` / `.insert-index`
Insert index entries.

---

## Keep Blocks

Keep text together on same page/column.

### `.bbk` / `.block-begin-keep`
Begin keep block. Text will not be split across pages.

### `.bek` / `.block-end-keep`
End keep block.

---

## Labels and Goto

### `.la` / `.label` name
Define a label for `.go` target.

### `.go` / `.go-to` label
Jump to labeled location in file.

### `.rt` / `.return`
Return from inserted file.

---

## Line Spacing

### `.ls` / `.linespace` [value]
Set line spacing.
- `value` — space between baselines (default: 12 points = 1 line)

---

## Literal Mode

### `.bbl` / `.block-begin-literal` [count]
Begin literal block. Text is not processed for controls.
- `count` — number of literal lines (-1 = until `.bel`)

### `.bel` / `.block-end-literal`
End literal block.

---

## Page Definition

### `.pd` / `.page-define`
Synonym for `.pdl` (starts page length definition chain).

### `.pdc` / `.page-define-columns` specs...
Define page columns.
- `specs` — column widths and gutters

**Example:**
```
.pdc 3i, .5i, 3i    (two 3-inch columns with .5-inch gutter)
.pdc 0, .5i, 2.5i, .5i, 2.5i    (three columns, first is margin)
```

### `.pdl` / `.page-define-length` [value]
Set page length.

### `.pdw` / `.page-define-width` [value]
Set page width (measure).

### `.pml` / `.page-margin-left` [odd] [,even]
Set left page margin.

### `.ps` / `.page-space`
Reserved (not implemented).

---

## Picture Blocks

### `.bbp` / `.block-begin-picture`
Begin picture block.

### `.bep` / `.block-end-picture`
End picture block.

---

## References and Variables

### `.sr` / `.set-reference` name [value]
Set a reference variable.

**Example:**
```
.sr DocumentTitle "User Manual"
.sr Version 2.1
```

### `.src` / `.set-reference-counter` name [value]
Set a counter variable.

### `.srv` / `.set-reference-variable` name [value]
Synonym for `.sr`.

### `.srm` / `.set-reference-mode` mode names...
Set display mode for variables.
- Modes: numeric, roman, alpha, etc.

### `.ur` / `.use-reference`
Process symbol references in following text.
- Symbol delimiter (default `%`) surrounds variable names: `%PageNo%`

### `.eqc` / `.equation-count` [value]
Set/increment equation counter.

---

## Rules

### `.hrul` / `.horizontal-rule`
Insert horizontal rule (not implemented).

### `.vrul` / `.vertical-rule`
Insert vertical rule (not implemented).

---

## Spacing

### `.sp` / `.space` [amount]
Add vertical space.
- `amount` — space to add (default: 1 line)

### `.spb` / `.space-break` [amount]
Space with block break.

### `.spd` / `.space-to-depth` [depth]
Space to absolute page depth.

### `.spf` / `.space-format` [amount]
Space with format break (within block).

### `.spt` / `.space-total` [amount]
Space to ensure total accumulated space.

---

## Tables

### `.tab` / `.table-define` specs
Define table format.

### `.tac` / `.table-column` [column]
Switch to table column.

### `.tan` / `.table-on`
Enter table mode.

### `.taf` / `.table-off`
Exit table mode.

---

## Text Titles and Captions

### `.btt` / `.begin-text-title`
Begin text title block.

### `.ett` / `.end-text-title`
End text title block.

### `.btc` / `.begin-text-caption`
Begin text caption block.

### `.etc` / `.end-text-caption`
End text caption block.

### `.thl` / `.text-header-line`
Single-line text header.

### `.tcl` / `.text-caption-line`
Single-line text caption.

### `.tlh` / `.title-line-header`
Title line for headers.

### `.tlc` / `.title-line-caption`
Title line for captions.

### `.ttl` / `.text-title-line`
Synonym for `.thl`.

---

## Title Blocks

### `.tb` / `.title-block`
Begin/define title block.

### `.tbb` / `.title-block-begin`
Begin title block.

### `.tbe` / `.title-block-end`
End title block.

### `.stl` / `.split-title-line`
Split title line format.

---

## Translation

### `.trn` / `.translate` pairs
Define character translation pairs.

**Example:**
```
.trn ae    (translate 'a' to 'e')
```

### `.trf` / `.translate-formatted`
Translate in formatted output.

### `.tre` / `.translate-exceptions`
Define translation exceptions.

---

## Underscoring

### `.unson` / `.underscore-on`
Begin underscoring mode.

### `.unsoff` / `.underscore-off`
End underscoring mode.

---

## Undenting

Undents temporarily reduce margins for a single line.

### `.un` / `.undent` [value]
Undent from left margin.

### `.unb` / `.undent-both` [value]
Undent from both margins.

### `.unh` / `.undent-hanging`
Hanging undent (first line outdented).

### `.unl` / `.undent-left` [value]
Undent from left.

### `.unn` / `.undent-nobreak` [value]
Undent without break (obsolete, use `.unh`).

### `.unr` / `.undent-right` [value]
Undent from right.

---

## Vertical Alignment

### `.vab` / `.vertical-align-bottom`
Align to bottom (not implemented).

### `.vac` / `.vertical-align-center`
Center vertically (not implemented).

### `.vaj` / `.vertical-align-justified`
Justify vertically (not implemented).

### `.vat` / `.vertical-align-top`
Align to top (not implemented).

---

## Vertical Margins

### `.vm` / `.vertical-margin-all`
Set all vertical margins (chains to `.vmt`).

### `.vmb` / `.vertical-margin-bottom`
Set bottom margin.

### `.vmf` / `.vertical-margin-footer`
Set footer margin (space between text and footer).

### `.vmh` / `.vertical-margin-header`
Set header margin (space between header and text).

### `.vmt` / `.vertical-margin-top`
Set top margin.

---

## Widow Control

### `.wi` / `.widow`
Synonym for `.wit`.

### `.wif` / `.widow-footnote` [lines]
Set minimum footnote widow lines.

### `.wit` / `.widow-text` [lines]
Set minimum text widow lines.
- `lines` — minimum lines at page bottom/top (default: 2)

---

## Writing Output

### `.wrt` / `.write-text` path text
Write text to auxiliary file.

### `.wrf` / `.write-formatted` path text
Write formatted text to auxiliary file (not implemented).

### `.wro` / `.write-order`
Write order file (not implemented).

---

## Miscellaneous

### `.dfu` / `.defer-until` condition
Defer text until condition (e.g., `block_split`).

### `.dmp` / `.dump`
Debugging dump (not implemented).

### `.do` / `.do`
Begin do loop (not implemented).

### `.enddo`
End do loop (not implemented).

### `.dvc` / `.device-control`
Send device-specific control (not implemented).

### `.err` / `.error` message
Generate error message.

### `.exc` / `.execute` command
Execute Multics command.

### `.galley` / `.gl` / `.galley-mode`
Enter galley mode (continuous output, no page breaks).
Must be first control in file.

### `.hit` / `.hit`
Create index hit entry.

### `.hif` / `.hit-file`
Define hit file.

### `.rd` / `.read`
Read input from terminal.

### `.ty` / `.type` text
Write text to error output (console).

### `.wt` / `.wait`
Wait signal for synchronization.

---

## Built-in Variables

Reference with `%name%` (or custom delimiter):

| Variable | Description |
|----------|-------------|
| `%PageNo%` | Current page number |
| `%Date%` | Current date |
| `%Time%` | Current time |
| `%InputFileName%` | Source file name |
| `%LineNo%` | Current line number |

User variables set with `.sr` are referenced the same way.

---

## Obsolete Commands

These commands are recognized but deprecated:

- `.bb` → use `.bblk`
- `.bba` → use `.bart`
- `.bbe` → use title blocks
- `.bbf` → footnote blocks
- `.bbi` → inline blocks
- `.bbk` → keep blocks
- `.bbl` → literal blocks
- `.bbn` → named blocks
- `.bbp` → picture blocks
- `.bbt` → title blocks
- `.bbc` / `.bec` → use `.tac`
- `.be`, `.bea`, `.bee`, `.bek`, `.ben`, `.bet` → corresponding end blocks
- `.fbb` / `.fbe` → use `.fb`
- `.hbb` / `.hbe` → use `.hb`
- `.rac`, `.ral`, `.rar` → runaround (not documented/implemented)
