---
name: excel-file-match
description: Reconcile Excel identifiers with drawing files, copy matches, mark found cells, and add missing records with profile images and drawing categories. Use for spreadsheet-to-file-library updates, not live Excel session control.
---

# Excel File Match

Reconcile identifiers in a workbook with filenames in one or more folders. Preserve the source workbook and files, copy matched files into a flat destination directory, and mark the matching identifier cells in an output workbook copy.

## Workflow

1. Identify the workbook, sheet, identifier column, file-search roots, destination folder, and included file types. Use column C when the task says that column contains the identifiers. Search subfolders only when the user means the full folder tree.
2. Read the installed Spreadsheets skill before editing a workbook. Follow its supported authoring tool and required artifact-operation contract. Preserve formulas, sheets, values, layout, and unrelated formatting. If an old workbook format cannot be safely edited by the available tool, do not silently convert or overwrite it; use a verified format-preserving route or explain the limitation.
3. Treat identifiers as text. Build a match manifest before copying or editing, with workbook, sheet, row, original cell value, candidate filename stem, applied equivalence rule, associated DWG/PDF files, signature-and-seal review for PDFs, file category, and match status.
4. Match exact identifiers first. Apply only the confirmed equivalence rules in the matching reference. Do not make broad fuzzy, typo, transposition, or suffix guesses. Keep uncertain candidates in a review list and ask one concise question at a time if the choice affects results.
5. Reconcile in both directions. For each Excel identifier, record whether a file exists. For each in-scope file, check whether a workbook record exists. If a file exists but its record is absent, add a row in the workbook copy with project number, mold number, material, a cropped profile cross-section image, explicit overall width and height, profile type, and drawing category. Fill these drawing-derived fields only on newly added rows; do not backfill blank fields in existing rows unless the user asks. Read [references/workbook-editing.md](references/workbook-editing.md) for extraction, classification, and image handling.
6. Copy each confirmed matching file directly into the requested destination root, including files whose records are newly added. Do not create per-source subfolders or move/delete source files. For a PDF that visibly contains both a signature and a seal, append 会签 to the destination copy's filename before the extension, without changing the source. Do not append it twice. If an output name already exists, do not overwrite it; compare content and report conflicts. Keep exact duplicate content once when that behavior is requested.
7. In a copy of the workbook, mark only the matched mold-number cells with yellow fill, including rows added for files found in the source folders. Remove an old yellow match marker from unmatched mold-number cells only when the user wants yellow to mean "found". Preserve all other cell formatting and do not color the entire row.
8. For a confirmed alias, replace the cell's identifier with the exact filename stem, without the extension, only when the user asked to align spreadsheet identifiers to filenames. Keep it as text so leading zeros remain intact. Do not change formulas or other fields to perform the correction.
9. Reconcile the manifest against the destination files and edited workbook. Report matched existing rows, new records added, Excel identifiers with no file, alias corrections, filename collisions, skipped or ambiguous files, and the paths of both output artifacts.

## Confirmed defaults

- The workbook is copied and the original remains unchanged.
- Matched files are copied, not moved, into one flat folder with no child directories.
- Only the matched identifier cell in column C is yellow; a whole row is not highlighted.
- Files that exist in the source folders but not in the workbook are added as new workbook records, and are also copied into the destination library.
- New rows include 项目号, 模具编号, 材质, 截面图, 总宽（mm）, 总高（mm）, 型材类型, and 图纸类别, mapped to the workbook's existing headers. The image is a crop of the profile cross-section view, not a full-sheet preview. Read values from the drawing and category from the matching folder files; leave uncertain fields blank and flag them for review. Do not backfill existing rows unless asked.
- Figure category uses the files present for that project/mold record: DWG only = cad; ordinary PDF only = pdf; both = cad+pdf; a signed-and-sealed PDF only = 会签版; DWG plus that signed-and-sealed PDF = cad+会签. See the workbook reference for combinations.
- Prefix and zero-padding aliases are project-specific. Apply them only when the user provides or confirms the mapping for that task. See [references/matching.md](references/matching.md) for boundaries.

## References

- Read [references/matching.md](references/matching.md) for normalization, aliases, candidate review, and duplicate handling.
- Read [references/workbook-editing.md](references/workbook-editing.md) for spreadsheet preservation, marking, and verification.
