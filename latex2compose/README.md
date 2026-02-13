# latex2compose

A converter from LaTeX to ｢‍｣ Lingenic Compose.

---

## Purpose

Provide an adoption path for existing LaTeX documents. Convert once, maintain in Compose.

---

## Usage

```
latex2compose input.tex -o output.compose
latex2compose input.tex                     # outputs to stdout
latex2compose --check input.tex             # report unconvertible constructs
```

---

## Conversion Rules

### Document Structure

| LaTeX | Compose |
|-------|---------|
| `\documentclass{...}` | (ignored) |
| `\usepackage{...}` | (ignored) |
| `\begin{document}` | (removed) |
| `\end{document}` | (removed) |
| `\title{...}` | `.set-reference DocumentTitle "..."` |
| `\author{...}` | `.set-reference DocumentAuthor "..."` |
| `\date{...}` | `.set-reference DocumentDate "..."` |
| `\maketitle` | (template expansion) |

### Sectioning

| LaTeX | Compose |
|-------|---------|
| `\part{Title}` | `.begin-text-title` ... `.end-text-title` |
| `\chapter{Title}` | `.begin-text-title` ... `.end-text-title` |
| `\section{Title}` | `.begin-text-title` ... `.end-text-title` |
| `\subsection{Title}` | `.begin-text-title` ... `.end-text-title` |
| `\subsubsection{Title}` | `.begin-text-title` ... `.end-text-title` |
| `\paragraph{Title}` | `.begin-text-title` ... `.end-text-title` |

Sectioning level preserved via `.set-reference SectionLevel N` before each title.

### Text Formatting

| LaTeX | Compose |
|-------|---------|
| `\textbf{text}` | `.font bold` text `.font -reset` |
| `\textit{text}` | `.font italic` text `.font -reset` |
| `\texttt{text}` | `.font monospace` text `.font -reset` |
| `\emph{text}` | `.font italic` text `.font -reset` |
| `\underline{text}` | `.underscore-on` text `.underscore-off` |
| `{\bf text}` | `.font bold` text `.font -reset` |
| `{\it text}` | `.font italic` text `.font -reset` |
| `{\tt text}` | `.font monospace` text `.font -reset` |

### Alignment

| LaTeX | Compose |
|-------|---------|
| `\begin{center}` | `.align-center` |
| `\end{center}` | `.align-left` |
| `\begin{flushleft}` | `.align-left` |
| `\end{flushleft}` | (none) |
| `\begin{flushright}` | `.align-right` |
| `\end{flushright}` | `.align-left` |
| `\centering` | `.align-center` |
| `\raggedright` | `.align-left` |
| `\raggedleft` | `.align-right` |

### Spacing

| LaTeX | Compose |
|-------|---------|
| `\newpage` | `.break-page` |
| `\pagebreak` | `.break-page` |
| `\newline` | `.break-format` |
| `\\` | `.break-format` |
| `\vspace{1cm}` | `.space 1c` |
| `\hspace{1cm}` | (inline space) |
| `\smallskip` | `.space 3p` |
| `\medskip` | `.space 6p` |
| `\bigskip` | `.space 12p` |

### Lists

| LaTeX | Compose |
|-------|---------|
| `\begin{itemize}` | `.indent-left 0.25i` |
| `\item` | `.undent-left 0.25i` • `.break-format` |
| `\end{itemize}` | `.indent-left -0.25i` |
| `\begin{enumerate}` | `.indent-left 0.25i` + counter |
| `\end{enumerate}` | `.indent-left -0.25i` |
| `\begin{description}` | `.indent-left 0.5i` |
| `\end{description}` | `.indent-left -0.5i` |

### Footnotes

| LaTeX | Compose |
|-------|---------|
| `\footnote{text}` | `.block-begin-footnote` text `.block-end-footnote` |
| `\footnotemark` | `.footnote-reference` |
| `\footnotetext{text}` | `.block-begin-footnote u` text `.block-end-footnote` |

### Cross-References

| LaTeX | Compose |
|-------|---------|
| `\label{name}` | `.set-reference name %SectionNo%` |
| `\ref{name}` | `%name%` |
| `\pageref{name}` | `%name-page%` |

---

## Mathematics

### Environments

| LaTeX | Compose |
|-------|---------|
| `$...$` | `%math: ... %` |
| `\(...\)` | `%math: ... %` |
| `$$...$$` | `.begin-math` ... `.end-math` |
| `\[...\]` | `.begin-math` ... `.end-math` |
| `\begin{equation}` | `.begin-math` |
| `\end{equation}` | `.end-math` |
| `\begin{equation*}` | `.begin-math number=false` |
| `\begin{align}` | `.begin-math-align` |
| `\end{align}` | `.end-math-align` |
| `\begin{align*}` | `.begin-math-align number=false` |

### Greek Letters

| LaTeX | Unicode |
|-------|---------|
| `\alpha` | α |
| `\beta` | β |
| `\gamma` | γ |
| `\delta` | δ |
| `\epsilon` | ε |
| `\varepsilon` | ε |
| `\zeta` | ζ |
| `\eta` | η |
| `\theta` | θ |
| `\vartheta` | ϑ |
| `\iota` | ι |
| `\kappa` | κ |
| `\lambda` | λ |
| `\mu` | μ |
| `\nu` | ν |
| `\xi` | ξ |
| `\pi` | π |
| `\varpi` | ϖ |
| `\rho` | ρ |
| `\varrho` | ϱ |
| `\sigma` | σ |
| `\varsigma` | ς |
| `\tau` | τ |
| `\upsilon` | υ |
| `\phi` | φ |
| `\varphi` | φ |
| `\chi` | χ |
| `\psi` | ψ |
| `\omega` | ω |
| `\Gamma` | Γ |
| `\Delta` | Δ |
| `\Theta` | Θ |
| `\Lambda` | Λ |
| `\Xi` | Ξ |
| `\Pi` | Π |
| `\Sigma` | Σ |
| `\Upsilon` | Υ |
| `\Phi` | Φ |
| `\Psi` | Ψ |
| `\Omega` | Ω |

### Operators

| LaTeX | Unicode |
|-------|---------|
| `\times` | × |
| `\div` | ÷ |
| `\cdot` | · |
| `\pm` | ± |
| `\mp` | ∓ |
| `\ast` | ∗ |
| `\star` | ⋆ |
| `\circ` | ∘ |
| `\bullet` | • |

### Relations

| LaTeX | Unicode |
|-------|---------|
| `\leq` or `\le` | ≤ |
| `\geq` or `\ge` | ≥ |
| `\neq` or `\ne` | ≠ |
| `\approx` | ≈ |
| `\equiv` | ≡ |
| `\sim` | ∼ |
| `\simeq` | ≃ |
| `\cong` | ≅ |
| `\propto` | ∝ |
| `\ll` | ≪ |
| `\gg` | ≫ |
| `\subset` | ⊂ |
| `\supset` | ⊃ |
| `\subseteq` | ⊆ |
| `\supseteq` | ⊇ |
| `\in` | ∈ |
| `\notin` | ∉ |
| `\ni` | ∋ |

### Arrows

| LaTeX | Unicode |
|-------|---------|
| `\rightarrow` or `\to` | → |
| `\leftarrow` | ← |
| `\leftrightarrow` | ↔ |
| `\Rightarrow` | ⇒ |
| `\Leftarrow` | ⇐ |
| `\Leftrightarrow` | ⇔ |
| `\mapsto` | ↦ |
| `\uparrow` | ↑ |
| `\downarrow` | ↓ |
| `\nearrow` | ↗ |
| `\searrow` | ↘ |
| `\nwarrow` | ↖ |
| `\swarrow` | ↙ |

### Large Operators

| LaTeX | Unicode |
|-------|---------|
| `\sum` | ∑ |
| `\prod` | ∏ |
| `\coprod` | ∐ |
| `\int` | ∫ |
| `\iint` | ∬ |
| `\iiint` | ∭ |
| `\oint` | ∮ |
| `\bigcup` | ⋃ |
| `\bigcap` | ⋂ |
| `\bigoplus` | ⨁ |
| `\bigotimes` | ⨂ |

### Set Theory

| LaTeX | Unicode |
|-------|---------|
| `\emptyset` | ∅ |
| `\cup` | ∪ |
| `\cap` | ∩ |
| `\setminus` | ∖ |
| `\mathbb{N}` | ℕ |
| `\mathbb{Z}` | ℤ |
| `\mathbb{Q}` | ℚ |
| `\mathbb{R}` | ℝ |
| `\mathbb{C}` | ℂ |

### Other Symbols

| LaTeX | Unicode |
|-------|---------|
| `\infty` | ∞ |
| `\partial` | ∂ |
| `\nabla` | ∇ |
| `\forall` | ∀ |
| `\exists` | ∃ |
| `\neg` or `\lnot` | ¬ |
| `\land` or `\wedge` | ∧ |
| `\lor` or `\vee` | ∨ |
| `\therefore` | ∴ |
| `\because` | ∵ |
| `\sqrt` | √ |
| `\angle` | ∠ |
| `\perp` | ⊥ |
| `\parallel` | ∥ |
| `\triangle` | △ |
| `\square` | □ |
| `\diamond` | ◇ |
| `\aleph` | ℵ |
| `\hbar` | ℏ |
| `\ell` | ℓ |
| `\wp` | ℘ |
| `\Re` | ℜ |
| `\Im` | ℑ |
| `\prime` | ′ |
| `\ldots` | … |
| `\cdots` | ⋯ |
| `\vdots` | ⋮ |
| `\ddots` | ⋱ |

### Fractions and Roots

| LaTeX | Compose |
|-------|---------|
| `\frac{a}{b}` | `(𝑎)/(𝑏)` |
| `\dfrac{a}{b}` | `.fraction` block |
| `\tfrac{a}{b}` | `(𝑎)/(𝑏)` |
| `\sqrt{x}` | `√(𝑥)` |
| `\sqrt[n]{x}` | `.root(𝑛, 𝑥)` |

### Subscripts and Superscripts

| LaTeX | Compose |
|-------|---------|
| `x^2` | `𝑥²` |
| `x^{n+1}` | `𝑥^(𝑛+1)` |
| `x_i` | `𝑥ᵢ` |
| `x_{ij}` | `𝑥_(𝑖𝑗)` |
| `x_i^2` | `𝑥ᵢ²` |

Simple numeric superscripts/subscripts convert to Unicode. Complex expressions use `^()` and `_()` syntax.

### Delimiters

| LaTeX | Compose |
|-------|---------|
| `\left( ... \right)` | `( ... )` |
| `\left[ ... \right]` | `[ ... ]` |
| `\left\{ ... \right\}` | `{ ... }` |
| `\langle ... \rangle` | `⟨ ... ⟩` |
| `\lfloor ... \rfloor` | `⌊ ... ⌋` |
| `\lceil ... \rceil` | `⌈ ... ⌉` |
| `\| ... \|` | `‖ ... ‖` |

### Matrices

| LaTeX | Compose |
|-------|---------|
| `\begin{matrix}` | `.matrix delimiters=none` |
| `\begin{pmatrix}` | `.matrix delimiters=parentheses` |
| `\begin{bmatrix}` | `.matrix delimiters=brackets` |
| `\begin{vmatrix}` | `.matrix delimiters=pipes` |
| `\begin{Vmatrix}` | `.matrix delimiters=double-pipes` |
| `a & b \\ c & d` | `𝑎, 𝑏` newline `𝑐, 𝑑` |
| `\end{...matrix}` | `.end-matrix` |

### Functions

| LaTeX | Compose |
|-------|---------|
| `\sin` | sin |
| `\cos` | cos |
| `\tan` | tan |
| `\log` | log |
| `\ln` | ln |
| `\exp` | exp |
| `\lim` | lim |
| `\max` | max |
| `\min` | min |
| `\sup` | sup |
| `\inf` | inf |
| `\det` | det |
| `\gcd` | gcd |
| `\arg` | arg |

Functions remain as text (rendered upright per ISO 80000-2).

### Accents

| LaTeX | Compose |
|-------|---------|
| `\hat{x}` | x̂ |
| `\bar{x}` | x̄ |
| `\dot{x}` | ẋ |
| `\ddot{x}` | ẍ |
| `\tilde{x}` | x̃ |
| `\vec{x}` | x⃗ |
| `\overline{x}` | x̄ |

---

## Figures and Tables

### Figures

| LaTeX | Compose |
|-------|---------|
| `\begin{figure}` | `.begin-block` |
| `\end{figure}` | `.end-block` |
| `\includegraphics{file}` | `.insert-graphic file` |
| `\caption{text}` | `.begin-text-caption` text `.end-text-caption` |

### Tables

| LaTeX | Compose |
|-------|---------|
| `\begin{table}` | `.begin-block` |
| `\end{table}` | `.end-block` |
| `\begin{tabular}{cols}` | `.table-define` ... `.table-on` |
| `\end{tabular}` | `.table-off` |
| `&` | `.table-column` |
| `\\` | `.break-format` |
| `\hline` | `.horizontal-rule` |

---

## Unsupported Constructs

The converter reports but does not translate:

| Construct | Reason |
|-----------|--------|
| TikZ/PGF | Complex graphics DSL |
| Custom macros (`\newcommand`) | Requires semantic analysis |
| BibTeX/bibliography | Separate toolchain |
| Index generation | Separate toolchain |
| Beamer slides | Presentation format |
| Complex table spans | Layout engine specific |

Unsupported constructs are preserved as comments:

```
.* UNCONVERTED: \begin{tikzpicture}...
```

---

## Output Options

| Flag | Effect |
|------|--------|
| `--check` | Report unconvertible constructs, do not output |
| `--strict` | Fail on any unconvertible construct |
| `--comments` | Preserve LaTeX comments as Compose comments |
| `--no-unicode-vars` | Keep ASCII variable names (x not 𝑥) |
| `--wrap=N` | Wrap output lines at N characters |

---

## Implementation Notes

### Parser

Use existing LaTeX parser (e.g., `tex2ast`, `latexcodec`, or custom PEG grammar). Do not attempt regex-based conversion—LaTeX grammar is context-sensitive.

### Two-Pass Conversion

1. **Parse**: Build AST from LaTeX source
2. **Transform**: Walk AST, emit Compose

### Variable Italicization

Single-letter variables in math mode convert to mathematical italic Unicode:

- `a` → `𝑎` (U+1D44E)
- `x` → `𝑥` (U+1D465)

Multi-letter sequences remain ASCII (likely function names).

### Brace Handling

LaTeX uses braces for grouping. Compose uses explicit begin/end. The converter must track nesting depth and emit appropriate commands.

---

## Example

### Input (LaTeX)

```latex
\documentclass{article}
\begin{document}

\section{The Quadratic Formula}

For any quadratic equation $ax^2 + bx + c = 0$ where $a \neq 0$, 
the solutions are:

\begin{equation}
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
\end{equation}

\end{document}
```

### Output (Compose)

```
.set-reference SectionLevel 1
.begin-text-title
The Quadratic Formula
.end-text-title

For any quadratic equation %math: 𝑎𝑥² + 𝑏𝑥 + 𝑐 = 0 % where %math: 𝑎 ≠ 0 %, 
the solutions are:

.begin-math
𝑥 = (−𝑏 ± √(𝑏² − 4𝑎𝑐)) / (2𝑎)
.end-math
```

---

## License

Apache 2.0

---

*latex2compose: The on-ramp to readable documents.*
