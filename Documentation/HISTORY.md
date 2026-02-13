# ｢‍｣ Lingenic Compose: History

---

## Multics

Multics (Multiplexed Information and Computing Service) was a time-sharing operating system developed beginning in 1965 as a joint project between MIT, General Electric, and Bell Labs. After Bell Labs withdrew in 1969—leading to the creation of Unix—MIT and Honeywell continued development.

Multics pioneered concepts now fundamental to computing: hierarchical file systems, dynamic linking, ring-based security, shared memory, and hot-swapping. It was designed for continuous operation and served as the primary computing environment at MIT and dozens of other installations through the 1970s and 1980s.

The last Multics system, operated by the Canadian Department of National Defence in Halifax, was shut down on October 30, 2000.

---

## Compose

Compose was the text formatting system for Multics, developed between 1972 and 1985. It processed source files containing text and control commands (dot commands) to produce formatted output for terminals, printers, and phototypesetters.

### Design Context

The computing environment imposed severe constraints:

- **Terminal speed**: Teletypes operated at 110 baud (~10 characters/second). Video terminals ran at 1200–9600 baud. Every keystroke had perceptible cost.
- **Storage**: Disk space was expensive. Shorter source files meant lower costs.
- **User base**: Both expert operators who formatted documents daily and occasional users who needed readable syntax.

### The Dual-Form Solution

The Multics team developed a naming system with two equivalent forms for every command:

| Terse | Verbose |
|-------|---------|
| `.bph` | `.begin-page-header` |
| `.alc` | `.align-center` |
| `.vmt` | `.vertical-margin-top` |
| `.brn` | `.break-need` |
| `.fif` | `.fill-off` |

The parser accepted either. Expert users typed terse forms for speed. Documentation and learners used verbose forms for clarity. The same source file could mix both.

### Naming Conventions

The verbose forms followed systematic patterns:

**Verb-object construction:**
```
.align-left
.break-page
.fill-on
.indent-right
```

**Begin/end pairs:**
```
.begin-page-header / .end-page-header
.begin-column-footer / .end-column-footer
.begin-artwork / .end-artwork
```

**Hierarchical modifiers:**
```
.vertical-margin-top
.vertical-margin-bottom
.vertical-margin-header
.vertical-margin-footer
```

This structure made commands discoverable. Knowing `.vertical-margin-top` existed suggested `.vertical-margin-bottom`. Seeing `.begin-page-header` implied `.end-page-header`. The naming system itself served as documentation.

### Terse Form Construction

Terse forms followed predictable abbreviation rules—typically first letters of hyphenated components:

```
begin-page-header     → bph
end-column-footer     → ecf
vertical-margin-top   → vmt
change-bars-deletion  → cbd
```

This predictability meant users could often guess the terse form from the verbose, or vice versa.

---

## What Was Lost

Multics Compose never achieved the adoption of its contemporaries. While troff became the standard for Unix documentation and Scribe's concepts influenced LaTeX, Compose remained bound to Multics.

When Multics was decommissioned, Compose went with it. The software survives in archives and the documentation remains available, but the system has had no active users for over two decades.

More significantly, the design philosophy found no successor:

**Self-documenting names**: Later systems chose brevity. LaTeX uses `\frac`, `\textbf`, `\hspace`. Typst uses `#set`, `#show`, `#let`. Neither adopted verbose vocabulary.

**Systematic naming**: LaTeX accumulated commands from dozens of packages with inconsistent conventions. Typst is principled but chose function-call syntax over natural language.

**Dual-form accommodation**: No subsequent system offered equivalent terse and verbose forms for all commands.

The Multics team, constrained by 1970s hardware, built a naming system that prioritized human readability. Four decades of subsequent development optimized for other goals.

---

## Recovery

This project recovers the Compose design philosophy for modern use.

**Preserved:**
- The complete verbose command vocabulary
- The naming conventions (verb-object, begin/end, hierarchical modifiers)
- The principle that commands should explain themselves

**Removed:**
- Terse forms—the constraints that justified them no longer exist

**Extended:**
- Block templates for reusable content structures
- Style classes for separated presentation
- Unicode mathematics following ISO 80000-2

The result maintains continuity with a 50-year-old design while addressing requirements—reusable components, styling abstraction, native Unicode—that the original system predates.

---

## Sources

- Multics Compose documentation, MIT/Honeywell
- Multics repository archives (git.io/multics)
- "Multics: History and the End of an Era," Tom Van Vleck
- ISO 80000-2:2019, Quantities and units — Part 2: Mathematics

---

*The Multics team built for constraints we no longer have. What remains valuable is not their compromises but their principles.*
