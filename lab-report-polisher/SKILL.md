---
name: lab-report-polisher
description: Use when editing Chinese university lab report DOCX templates, especially to insert programming-question code, outputs, figures, and analysis while preserving the original template, teacher review areas, fonts, spacing, and report structure.
---

# Lab Report Polisher

## Overview

Use this skill to complete only the programming-report portions of a Chinese lab report template. The priority order is: preserve the original framework, place content under the matching question, make output report-ready, then verify the rendered DOCX.

## Workflow

1. Identify the source template, programming questions, data files, and expected deliverable.
2. Keep the original `.docx` untouched. Save edits to a clearly named copy.
3. Insert each question's report content directly below its own prompt: `关键代码` -> `输出结果` -> `结果分析`.
4. Do not fill unrelated sections unless the user asks: concept questions, data analysis blanks, experiment conclusion, teacher comments, grade, signature, and remarks.
5. Use the Documents skill for DOCX work. Render the finished DOCX to PNGs and inspect the pages. If rendering fails because `soffice` is unavailable, say so.
6. Run structural checks before delivery: DOCX archive test, required text present, original template unchanged, teacher review framework present.

## Must Preserve

- Never delete `指导教师批阅意见`, `成绩评定`, `指导教师签字`, `备注`, or template notes unless the user explicitly asks.
- Never rewrite the entire report when only programming content is requested.
- Never move programming content to the end if the user asked for question-level placement.
- If a mostly blank page contains only standalone `深圳大学学生实验报告用纸`, remove only that standalone paragraph and its immediate empty spacer. Do not remove the final review table.
- Keep teacher-provided frameworks or libraries unmodified unless the user explicitly requests a framework change.

## Formatting Defaults

- Chinese body text: `宋体`, 五号 (`10.5 pt`).
- English/numbers in body: `Times New Roman`, `10.5 pt`.
- Body line spacing: single (`w:line=240`, `w:lineRule=auto`).
- Body paragraph spacing: usually `before=0 pt`, `after=4 pt`; adjust only when the page looks cramped or gappy.
- Body analysis paragraphs should usually use first-line indent of about 2 Chinese characters.
- Keep result analysis in academic prose, not scattered bullet points, unless the report format asks for bullets.
- Figure captions should be centered and smaller than body text.

## Code Blocks

Use a framed code block for report snippets:

- Font: `Consolas`, about `8-8.5 pt`.
- Background: light gray, e.g. `F7F7F7`.
- Border: thin gray, e.g. `BFBFBF`.
- Add modest paragraph spacing before/after so the code does not collide with surrounding text.
- Use key code only, not full files, unless the question requires full source.
- Add short comments in the code that explain algorithmically important changes.

## Output And Analysis

- Generate `outputs/` with text logs and clear figures used by the report.
- Include concrete metrics in output text: accuracy, macro-F1, MSE, MAE, confusion matrix, loss values, or task-appropriate values.
- Result analysis should explain what the numbers show, what limitations remain, and why the behavior occurs.
- When a model overfits, underfits, or depends on small data, say so plainly and connect it to the dataset/model design.

## Implementation Notes

For detailed DOCX insertion patterns, read `references/docx-patterns.md` when working on a report template.

