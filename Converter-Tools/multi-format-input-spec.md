# ｢‍｣ Lingenic Compose: Multi-Format Input

---

## Concept

Compose reads multiple input formats natively. One tool, many sources.

```
compose input.md -o output.pdf
compose input.tex -o output.pdf
compose input.wiki -o output.pdf
compose input.rtf -o output.pdf
compose input.1 -o output.pdf
compose input.compose -o output.pdf
```

Same output pipeline. Format detected by extension or `--from` flag.

---

## Supported Input Formats

| Extension | Format | Flag |
|-----------|--------|------|
| `.compose` | ｢‍｣ Lingenic Compose | `--from=compose` |
| `.md` | Markdown | `--from=markdown` |
| `.tex` | LaTeX | `--from=latex` |
| `.rtf` | Rich Text Format | `--from=rtf` |
| `.wiki` | MediaWiki | `--from=mediawiki` |
| `.1` `.2` `.3` ... | Unix man pages | `--from=man` |
| `.mdoc` | BSD mdoc | `--from=mdoc` |
| `.adoc` | AsciiDoc | `--from=asciidoc` |
| `.rst` | reStructuredText | `--from=rst` |
| `.org` | Org-mode | `--from=org` |
| `.textile` | Textile | `--from=textile` |
| `.creole` | WikiCreole | `--from=creole` |
| `.dokuwiki` | DokuWiki | `--from=dokuwiki` |
| `.confluence` | Confluence | `--from=confluence` |
| `.html` | HTML | `--from=html` |
| `.docx` | Word (future) | `--from=docx` |
| `.odt` | OpenDocument (future) | `--from=odt` |
| `.epub` | EPUB (future) | `--from=epub` |

---

## Architecture

```
┌─────────────┐
│ Input File  │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Reader    │  ← Format-specific parser
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Compose    │  ← Internal representation
│    AST      │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   Writer    │  ← Output renderer
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Output File │
└─────────────┘
```

All input formats parse to the same internal AST. All output formats render from that AST.

---

## Usage

### Basic

```
compose document.md                     # Markdown → PDF (default output)
compose document.tex -o out.pdf         # LaTeX → PDF
compose document.wiki -o out.compose    # MediaWiki → Compose source
```

### Explicit Format

```
compose --from=markdown input.txt -o output.pdf
compose --from=latex paper.txt -o paper.pdf
```

### Format Conversion

```
compose input.tex -o output.compose     # LaTeX → Compose source
compose input.md -o output.compose      # Markdown → Compose source
compose input.wiki -o output.md         # MediaWiki → Markdown
```

### Stdin

```
cat document.md | compose --from=markdown -o output.pdf
curl https://example.com/page.wiki | compose --from=mediawiki -
```

---

## Output Formats

| Extension | Format |
|-----------|--------|
| `.pdf` | PDF (default) |
| `.html` | HTML |
| `.compose` | Compose source |
| `.md` | Markdown |
| `.tex` | LaTeX |
| `.rtf` | RTF |
| `.txt` | Plain text |
| `.json` | AST as JSON |

---

## Format Options

### Markdown

```
compose input.md --md-flavor=gfm        # GitHub-Flavored
compose input.md --md-flavor=commonmark # Strict CommonMark
compose input.md --md-flavor=extended   # With footnotes, math
```

### LaTeX

```
compose input.tex --tex-macros=file.sty  # Load macro definitions
compose input.tex --tex-strict           # Fail on unknown commands
```

### Wiki

```
compose input.wiki --wiki-flavor=mediawiki
compose input.wiki --wiki-flavor=confluence
compose input.wiki --wiki-templates=templates.json
```

### HTML

```
compose input.html --html-strip-scripts
compose input.html --html-strip-styles
compose input.html --html-extract=article  # Extract <article> only
compose input.html --html-extract=main     # Extract <main> only
```

---

## Pipeline

Multiple inputs, combined output:

```
compose --from=markdown intro.md \
        --from=latex body.tex \
        --from=markdown appendix.md \
        -o complete.pdf
```

Or via file list:

```
compose --manifest book.txt -o book.pdf
```

Where `book.txt`:

```
intro.md
chapters/*.tex
appendix.md
```

---

## Include Directive

From within Compose source, include other formats:

```
.include-file "chapter1.md" format=markdown
.include-file "math-heavy.tex" format=latex
.include-file "legacy.rtf" format=rtf
```

Auto-detected if omitted:

```
.include-file "chapter1.md"
```

---

## Intermediate Output

Export the Compose AST for debugging or tooling:

```
compose input.tex -o output.json --to=ast
compose input.md -o output.compose       # Readable Compose source
```

The `.compose` output is always valid Compose source—round-trips cleanly.

---

## Format Detection

### By Extension

| Extension | Format |
|-----------|--------|
| `.md`, `.markdown` | Markdown |
| `.tex`, `.latex` | LaTeX |
| `.rtf` | RTF |
| `.wiki`, `.mediawiki` | MediaWiki |
| `.1`–`.9`, `.mdoc` | Man pages |
| `.adoc`, `.asciidoc` | AsciiDoc |
| `.rst` | reStructuredText |
| `.org` | Org-mode |
| `.html`, `.htm` | HTML |
| `.compose` | Compose |

### By Content (Fallback)

If extension is ambiguous (`.txt`), detect by content patterns:

| Pattern | Format |
|---------|--------|
| `\documentclass` | LaTeX |
| `{\rtf1` | RTF |
| `= Title =` or `[[link]]` | MediaWiki |
| `.Dd` or `.TH` | Man page |
| `# Heading` or `**bold**` | Markdown |
| `<!DOCTYPE` or `<html` | HTML |
| `.begin-` or `.align-` | Compose |

---

## Error Handling

### Unknown Format

```
$ compose mystery.xyz
Error: Cannot detect format for 'mystery.xyz'
Use --from=FORMAT to specify explicitly
```

### Parse Errors

```
$ compose broken.tex
Warning: Line 42: Unknown command \badmacro (skipped)
Warning: Line 87: Unmatched brace (recovered)
Output written to output.pdf with 2 warnings
```

### Strict Mode

```
$ compose broken.tex --strict
Error: Line 42: Unknown command \badmacro
```

---

## Implementation

### Reader Interface

Each format implements:

```
trait Reader {
    fn detect(content: &[u8]) -> bool;
    fn parse(content: &str, options: &Options) -> Result<AST, Error>;
}
```

### AST

Common representation:

```
Document
├── Metadata (title, author, date)
├── Block[]
│   ├── Heading { level, content }
│   ├── Paragraph { content }
│   ├── List { ordered, items[] }
│   ├── Table { headers[], rows[][] }
│   ├── CodeBlock { language, content }
│   ├── Math { display, content }
│   ├── Image { path, caption }
│   └── ...
└── Footnotes[]
```

### Writer Interface

Each output format implements:

```
trait Writer {
    fn render(ast: &AST, options: &Options) -> Result<Vec<u8>, Error>;
}
```

---

## Benefits

### For Users

- One tool to learn
- Any input, any output
- Mix formats in single document

### For Adoption

- No migration required
- Gradual transition possible
- "Just use your existing files"

### For Compose

- Proves the internal model is complete
- If it can represent LaTeX, Markdown, Wiki... it can represent anything
- Every converted document is a Compose document

---

## Command Summary

```
compose [OPTIONS] INPUT... -o OUTPUT

Options:
  --from=FORMAT       Input format (auto-detected if omitted)
  --to=FORMAT         Output format (from extension if omitted)
  --strict            Fail on any parse warning
  --manifest=FILE     Read input list from file
  
Format-specific:
  --md-flavor=X       Markdown flavor (gfm, commonmark, extended)
  --tex-macros=FILE   LaTeX macro definitions
  --wiki-flavor=X     Wiki flavor (mediawiki, confluence, etc.)
  --wiki-templates=F  Wiki template definitions
  --html-extract=SEL  Extract HTML element by selector
```

---

## Tagline

*Compose: Reads everything. Writes everything. One format to rule them all.*
