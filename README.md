# ｢‍｣ Lingenic Compose

A document formatting language with self-documenting syntax and native Unicode mathematics. 

Compose, developed between 1972 and 1985, processed source files containing text and control commands (dot commands) to produce formatted output for terminals, printers, and phototypesetters. 

This project recovers Compose, and extends the system with block templates, style classes, and Unicode mathematics. Unlike most such formats (LaTeX comes to mind) a Compose file is readable without rendering. Mathematical typesetting requires only ten layout commands plus Unicode.

---

## Overview

｢‍｣ Lingenic Compose descends from Multics Compose (1972–1985), a text formatting system developed at MIT and Honeywell for the Multics operating system—a pioneering time-sharing system that introduced hierarchical file systems, dynamic linking, and ring-based security. The Multics team created systematic naming conventions that made commands self-documenting. When Multics was decommissioned in 2000, Compose went with it. This project recovers Compose, and extends the system with block templates, style classes, and Unicode mathematics. See `HISTORY.md` for the full account.

Commands use verbose, descriptive names. Mathematics uses Unicode symbols directly—no escape sequences. The source file is readable without rendering.

```
.begin-page-header
.align-center
Document Title
.end-page-header

.vertical-margin-top 1i

The quadratic formula:

.begin-math
𝑥 = (−𝑏 ± √(𝑏² − 4𝑎𝑐)) / (2𝑎)
.end-math
```

---

## Design

**Commands are sentences:**
```
.begin-page-header      "Begin a page header"
.break-page             "Break to a new page"
.vertical-margin-top    "Set vertical margin at top"
```

**Math is math:**

| Symbol | Meaning |
|--------|---------|
| ∑ | Summation |
| ∫ | Integral |
| α | Greek alpha |
| ℝ | Real numbers |
| → | Arrow |

Ten layout commands plus Unicode. No escape sequences.

**Structure separates from style:**
- Block templates define reusable content patterns
- Style classes define reusable formatting

---

## Documentation

- `SPECIFICATION.md` — Complete language reference
- `HISTORY.md` — Heritage and design decisions

---

*｢‍｣ Lingenic Compose: Because `.begin-page-header` needs no explanation.*
