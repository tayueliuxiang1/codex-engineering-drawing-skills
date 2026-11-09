# Codex Drawing and Excel Skills

This repository contains two reusable workflow skills:

- **drawing-split**: split framed or clearly grouped frameless DWG/PDF sheets into individual files, named from the drawing number or title.
- **excel-file-match**: reconcile Excel identifiers with drawing folders, copy matches, mark found rows, and add missing drawing records with section images and drawing metadata.

## Install

Copy either folder under `skills/` into the active Codex skills directory for your setup, then start a new Codex session. Invoke them explicitly with `$drawing-split` or `$excel-file-match` when you want to ensure the relevant workflow is used.

## Notes

- The skills contain workflow instructions and references; they do not include drawing files, spreadsheets, credentials, or a copy of AutoCAD/Excel/PDF software.
- The Excel matching skill ships without project-specific identifier aliases. Confirm prefix, zero-padding, or suffix exceptions for each project before using them.
- Frameless drawing boundaries are inferred only when title-block anchors, nearby annotations/views, and whitespace make the grouping clear; ambiguous items are left for review.
