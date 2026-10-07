# beamer-slides

Claude Code skill: house style for Beamer slides (advisor meetings, seminars, talks). See `SKILL.md`.

- 16:9, Singapore theme (its colors and clickable section-navigation headline), bold left-aligned frame titles, page
  numbers bottom right.
- Every frame title states the takeaway as a sentence; every content frame ends with a `\takeaway{}` box.
- Simplified booktabs tables beside short bullets; figures are the paper's journal-style PDFs (see the companion skill
  `journal-figures-tables`).

`assets/slides_template.tex` compiles as is (pdflatex, twice) and shows every layout pattern: bullets with grey inline
citations, equation with two contrast boxes, literature table, identification, table + bullets, full-width figure,
figure + bullets, problem / solution, figure + small table, data status, plan, questions for discussion.

Install: clone into `~/.claude/skills/beamer-slides`.
