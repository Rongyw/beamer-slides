---
name: beamer-slides
description: House style for Beamer presentation slides (advisor meetings, seminars, conference and job talks, pitch decks) — 16:9 Singapore theme with clickable section navigation, frame titles that state the takeaway as a sentence, a takeaway box on every frame, simplified booktabs tables beside short bullets, and journal-style figures. Use whenever creating, updating or restyling slides or a deck (.tex Beamer, "slides", "deck", "talk", "presentation") in any project.
---

# Beamer slides (takeaway style)

Every deck follows this style unless the user explicitly asks for another one.
Compilable template with every layout pattern: `assets/slides_template.tex` (copy it, keep the preamble, replace the frames).
Figures and tables reused on slides come from the `journal-figures-tables` skill (greyscale PDFs, numbers from the same CSVs).

## Preamble (keep exactly)
```latex
\documentclass[aspectratio=169,10pt]{beamer}
\usetheme{Singapore}                       % colors + clickable section-navigation headline (miniframes)
\usepackage{booktabs,array,graphicx}
\graphicspath{{figures/}}
\setbeamertemplate{frametitle}[default][left]
\setbeamerfont{frametitle}{series=\bfseries,size=\large}
\setbeamertemplate{navigation symbols}{}
\setbeamertemplate{itemize items}[circle]
\setbeamertemplate{enumerate items}[default]
\setbeamertemplate{footline}{\hbox to \paperwidth{\hfill\scriptsize\color{gray}%
  \insertframenumber\,/\,\inserttotalframenumber\hspace*{1em}}\vspace{0.6em}}
\setbeamercolor{block body}{bg=structure.fg!8}
\newcommand{\takeaway}[1]{\vfill\begin{beamercolorbox}[sep=5pt]{block body}\small\textbf{Takeaway:} #1\end{beamercolorbox}}
\newcommand{\citeg}[1]{\textcolor{gray}{(#1)}}
```
Use the Singapore theme's own colors; do not add a color theme or custom palette. Do not change fonts.

## Structure
- **Title page**: `\title{}` (for an advisor meeting: "Meeting Agenda"), `\subtitle{<paper title>}`, `\author{A \and B}` in
  the paper's agreed author order, `\institute{}`, `\date{<occasion>, <Month Year>}`.
- **Sections** drive the navigation headline: `\section{}` before each block of 2--5 frames. Default order for a research
  update: Motivation, Design, Results, Mechanism, Extensions, Next steps. Rename to fit, but keep 4--7 sections.
- **Last frame**: "Questions for discussion" (advisor meeting) or "Summary" (talk): a numbered list, each item starting
  with a bold label (`\item \textbf{Framing:} ...`). It has no takeaway box.
- Length: about 15--20 frames for a 30--45 minute meeting; 1 frame per 1.5--2 minutes for a talk.

## Every frame
- **Title = the takeaway, as a full sentence** with a verb and, where possible, the direction or size of the result
  ("Fuel-clause prices rise with regional wind output; co-op prices do not"), never a topic ("Results", "Regression
  table"). At most about 75 characters, so that it fits on one line at 16:9; split a longer claim across two frames
  or move the qualifier into the takeaway.
- **End with `\takeaway{...}`**: one sentence starting lower-case, which says what the evidence implies, not a repeat of
  the title. Exceptions: title page and the last frame.
- **Bullets**: at most 5 per frame (4 beside a table or figure), at most 2 lines each. Write each as a finding with its
  number, unit and $p$-value or standard error. One level of sub-bullets at most.
- **Citations** inline in grey: `\citeg{Author \& Author, 2021}`. No reference frame unless asked.
- **Abbreviations**: spell out at first use on the slides (`investor-owned utilities (IOUs)`), even if common in the field.
  No internal labels or code names on slides (file names, variable names, "JMP", etc.).
- **Math**: at most one display equation per frame; label its parts with `\underbrace{}_{\text{...}}`.

## Layout patterns (all in the template)
1. **Table + bullets** (results): `columns[T]`, left `0.56\textwidth` with `\footnotesize` and
   `\resizebox{\linewidth}{!}{tabular}`, right `0.42\textwidth` with `\small` bullets.
2. **Full-width figure**: `\includegraphics[width=0.92\textwidth]{fig.pdf}`, then a `\scriptsize` note (estimator, fixed
   effects, CI level, clustering, sample), then the takeaway.
3. **Figure + bullets**: left `0.58\textwidth` figure with a `\scriptsize` note under it, right `0.40\textwidth` bullets.
4. **Figure + small table below**: figure at most `0.68\linewidth`, then two `0.48\textwidth` columns (`\scriptsize`
   table | `\scriptsize` bullets).
5. **Contrast of two mechanisms**: two `0.48\textwidth` columns, each a `\begin{beamercolorbox}[sep=5pt]{block body}`
   with a bold heading and `\small` text.
6. **Literature / contribution table**: three `p{}` columns "Literature | What it does | What we add", `\citeg{}` in the
   first column.
7. **Data status / plan tables**: "Source | Status | Use" and "Weeks | Tasks | Deliverable", `\footnotesize` or `\small`.
8. **Problem / solution**: two columns with bold `\textbf{The problem}` and `\textbf{Why <method> fits}` headings.

## Tables on slides
- Simplified booktabs (`\toprule`, `\midrule`, `\bottomrule`), `@{}l...@{}` columns, no vertical rules.
- Columns: `Est. & SE & $p$` (or `Boot.\ $p$`, `$F$`, `BAs`/`N`). Report $p$-values instead of stars. 2--3 decimals;
  negative numbers in math mode (`$-0.043$`).
- Italic sub-panel rows: `\multicolumn{4}{@{}l}{\textit{Robustness}}\\`; `\addlinespace` between blocks; ranges in
  one cell: `\multicolumn{3}{c}{0.037--0.074}`.
- Never paste the full paper table; pick the 4--8 rows that support the title.

## Figures on slides
- Reuse the paper PDFs (greyscale, journal style) from `figures/`; never screenshots or PNGs when a PDF exists.
- Wide two-panel figures go full width (pattern 2) or left of short bullets (pattern 3).
- The note under the figure carries what the paper caption would: estimator, controls, CI level, clustering, sample, source.

## Workflow
1. Copy `assets/slides_template.tex` into the project's slides or meeting folder (Overleaf-ready: `.tex` plus
   `figures/*.pdf`). Name it `slides_<YYYY-MM-DD>.tex`.
2. Take every number from the rendered paper tables or their CSVs; never retype from memory. When results change, update
   the slides in the same pass as the paper or proposal.
3. Compile twice with `pdflatex -interaction=nonstopmode` (frame totals need the second pass).
4. Check the log: 0 errors (`grep -c '^!'`), and **no `Overfull \vbox`** (a frame that is too tall: shrink the figure,
   cut a bullet or shorten the takeaway). Small `Overfull \hbox` from tables under 5pt are acceptable.
5. Render pages to PNG (`pdftoppm -r 70 -png`) and look at every changed frame: titles on one line, no text running into
   the takeaway box, legends readable.
6. Delete the auxiliary files (`.aux .log .nav .out .snm .toc`), keep the `.tex` and the `.pdf`.

## Checklist before reporting
- [ ] Preamble unchanged; Singapore navigation headline shows the sections.
- [ ] Every frame title is a sentence that states the takeaway.
- [ ] Every content frame ends with a `\takeaway{}`; the last frame is "Questions for discussion" or "Summary".
- [ ] At most 5 bullets per frame; every result bullet has a number with units and $p$ or SE.
- [ ] Tables are simplified booktabs with $p$-values; figures are the paper PDFs with a note.
- [ ] Abbreviations spelled out at first use; no internal labels.
- [ ] Compiles with 0 errors and no `Overfull \vbox`; changed frames looked at as images.
