# Workbook copy, cell updates, and verification

Read the installed Spreadsheets skill first and follow its current API documentation. For standalone workbook edits, use its supported artifact authoring tool and dependencies; do not use openpyxl or another authoring library when the approved artifact tool is available.

## Preserve the original

- Inspect workbook sheets and the intended identifier column before editing. Confirm header row, data range, formulas, hidden rows, and existing fill styles in the target cells.
- Work from a copy of the input workbook. Keep all sheets, formulas, values, merged ranges, and unrelated formatting intact.
- If editing a legacy .xls is not supported without loss, do not silently replace it or convert it. Use a verified format-preserving route when available; otherwise ask whether an .xlsx output copy is acceptable.
- If a matched identifier cell contains a formula, do not replace the formula with a value. Mark it only if the row match is confirmed, and report that the alias text could not be corrected in that formula cell.

## Mark found identifiers

- Apply a standard yellow fill (for example, #FFFF00) to the matched identifier cell only. Do not fill the entire row.
- If the user wants yellow to mean "file found," remove the old yellow marker only from unmatched identifier cells in the designated column. Preserve non-yellow fills and all other formatting.
- Correct a confirmed alias to the matched file's exact filename stem only when requested. Store the value as text so leading zeros and hyphens remain exact. Do not change unrelated columns.
- When a cell is both a confirmed alias and a match, correct its value and fill in the same output workbook copy.

## Add records for files absent from the workbook

- Add one new record for each distinct project-number/mold-number pair found in the source folders but absent from the workbook. When both DWG and PDF files represent the same pair, copy both requested files and keep one row unless the workbook's existing structure clearly uses one row per format.
- Map fields by their existing headers: 项目号, 模具编号, 材质 (or the existing 材料 header), 截面图, 总宽, 总高, 型材类型, and 图纸类别. Respect units in header labels such as 总宽（MM） and 总高（MM）. If a requested header is absent, add only that requested column to the output copy without replacing existing columns. Do not add other columns merely because they appear in a screenshot. Use an adjacent data row's style for new rows and preserve the workbook's table/formula structure.
- Read project number, mold number, material, and an explicitly stated profile type from the actual title block, drawing notes, or reliable drawing text. Do not infer material from the profile appearance or project name, and do not assign a profile type from shape alone. If a field is unreadable, contradictory, or absent, leave it blank and flag that record for review.
- Fill total width and total height from explicit overall dimension annotations on the profile drawing. Do not estimate from pixels, viewport size, or geometric extents. Store numeric millimeter values where the workbook uses numeric dimensions. If the drawing does not clearly identify overall width or height, leave that value blank and flag it for review.
- Render or export the source drawing and crop only its actual profile cross-section view for 截面图. Preserve aspect ratio, adequate resolution, and dimensions that belong to the section view; exclude unrelated views and the full-sheet title block. Do not redraw the section or fabricate missing geometry.
- If a drawing contains several distinct section views for the same record, include them together only if a clear, readable crop can be placed in the cell. If it is unclear which section belongs to the record, leave the image blank and flag it rather than guessing.
- Embed the image using the supported spreadsheet authoring API, position it in the 截面图 cell area, and size the row/column so the whole profile remains visible without distorting unrelated table layout.
- Determine 图纸类别 from the verified files associated with the new project/mold pair, not from a stale workbook value or filename text. Use these values, in this order when combining: DWG only = cad; ordinary PDF only = pdf; DWG plus ordinary PDF = cad+pdf; signed-and-sealed PDF only = 会签版; DWG plus such a signed-and-sealed PDF = cad+会签; ordinary PDF plus signed-and-sealed PDF = pdf+会签; all three = cad+pdf+会签. Treat a PDF with only one of signature or seal as ordinary pdf. Inspect the PDF contents, and if either mark is unclear, flag the category for review rather than assuming 会签版.
- Apply 图纸类别 only to newly added rows unless the user asks to update existing rows too. For a signed-and-sealed PDF copied to the destination, append 会签 to the destination filename stem before .pdf (for example, drawing会签.pdf), preserving the source filename. Avoid adding the suffix twice.
- Populate these descriptive and dimension fields only for newly added file records. Do not fill blanks or replace values in existing workbook records unless the user asks.

## Artifact contract

Immediately before the first workbook authoring command, follow the installed Spreadsheets skill's artifact-operation requirement. For one edited workbook output, the expected form is:

```bash
node container_tools/mark_artifact_operation_started.mjs --operation-kind edit --expected-output-count 1 --output-format xlsx
```

Run it successfully exactly once for that workbook edit operation. Use the current skill's instructions if the supported output format or count differs. Do not bypass a rejected operation marker.

## Verify and report

- Reopen or inspect the saved workbook with the supported spreadsheet tool. Verify representative exact matches, alias corrections, unmatched rows, yellow fills, and preserved formulas and styles.
- Reconcile the rows marked yellow against the manifest: only exact or user-confirmed alias matches are yellow. If requested, confirm old yellow on unmatched identifier cells was cleared.
- Confirm the source workbook is unchanged and that the new workbook copy exists at the reported path.
- Report output format changes, cells that could not be corrected, and any validation that could not be performed.
