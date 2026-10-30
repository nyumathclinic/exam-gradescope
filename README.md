# exam-gradescope

Gradescope outlines, rubrics and answer keys for documents made with the
LaTeX [exam](https://ctan.org/pkg/exam) class.

## Introduction

Grading a paper exam on [Gradescope](https://www.gradescope.com) starts with
an outline drawn by hand on the blank exam, a rubric for every question, and
an answer key for any bubble sheet. Everything these need is already in the
LaTeX source. `exam-gradescope` records it while the document is typeset
with LuaLaTeX and writes, next to the PDF:

| File | Contents |
|---|---|
| `<jobname>.outline.json` | the region of each question, and of the student's name and ID, as percentages of the page |
| `<jobname>.rubric.json` | a rubric for each question: made from the correct choices for multiple-choice and check-all-that-apply questions, written in the source for free-response ones |
| `<jobname>.key.csv` | the answer key of the questions answered on a bubble sheet |
| `<jobname>.json` | a manifest of the questions (number, a name that is the same on every version, points, type, answer) for item analysis |

The answer key and rubrics come from the blank exam's own run, after any
shuffling of choices by
[exam-randomizechoices](https://ctan.org/pkg/exam-randomizechoices).

The outline and rubric files can be uploaded with
`edubag gradescope client setup-exam` from
[python-edubag](https://github.com/nyumathclinic/python-edubag), and rubric
files with the
[gradescope-tampermonkey](https://github.com/nyumathclinic/gradescope-tampermonkey)
importer.

## Requirements

- LuaLaTeX
- the exam class (or a class built on it)
- a LaTeX kernel from 2023 or later

## Building

This package uses [l3build](https://ctan.org/pkg/l3build).

Unpack the sources:

```shell
l3build unpack
```

Typeset the documentation:

```shell
l3build doc
```

Run the tests:

```shell
l3build check
```

Install:

```shell
l3build install --full
```

## Usage

```latex
\documentclass{exam}
\usepackage{exam-gradescope}            % a quiz graded on Gradescope
% \usepackage[mc=bubble]{exam-gradescope}  % an exam with a bubble sheet

\DeclareRubric{basic}{
  \item[1]   Correct
  \item[5/6] Mostly correct, but small mistakes
  \item[2/3] Major mistakes
  \item[1/3] Nice try
  \item[0]   No response, or no creditable work shown}

\begin{document}
\GradescopeRegion{name}{\fbox{\parbox{3in}{Name:\vspace{2em}}}}\hfill
\GradescopeRegion{sid}{\fbox{\parbox{2in}{ID:\vspace{2em}}}}

\begin{questions}
\question[2]\label{binomial} Which is equal to \(\binom{7}{3}\)?
\begin{oneparchoices}
  \choice 15 \choice 21 \CorrectChoice 35 \choice 56
\end{oneparchoices}

\question[6]\label{comb-proof} Give a combinatorial proof that ...
\begin{rubric}
  \begin{rubricgroup}[select one]{What is counted}
    \item[3] Clearly identifies the collection being counted
    \item[0] No collection identified
  \end{rubricgroup}
  \item[0] No response, or no creditable work shown
\end{rubric}

\question[3]\label{pizza} How many pizzas ...?
\userubric{basic}[\reword{Correct}{Correct, and explained}]
\end{questions}
\end{document}
```

Multiple-choice questions need nothing more: their rubric (or answer-key
line) is made from `\CorrectChoice`. See the documentation
(`exam-gradescope.pdf`) for the options, `\gradescopequestion`, and the
formats of the files.

## License

LaTeX Project Public License, version 1.3c or later.
