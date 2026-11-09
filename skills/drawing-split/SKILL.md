---
name: drawing-split
description: Split multi-sheet engineering drawings in DWG or PDF into individual files, using each sheet's drawing number or name and a flat output directory. When sheet borders are absent, infer regions from title blocks and spatial grouping if clear.
---

# Drawing Split

Split a DWG or PDF drawing set into separate files, named from the title block on each sheet. This skill covers DWG and PDF and should be used together with the available AutoCAD and PDF skills for format-specific operations.

## Shared rules

- Confirm the input scope and output location from the request and the available files. Search recursively only when the user means the whole folder tree.
- The default output is one flat directory: put each resulting drawing file directly in its root, with no per-source subfolders. Follow a different layout only when the user requests it.
- Read the actual drawing number from the sheet's title block. Preserve its prefix, letters, separators, and leading zeros. Do not derive it from source filename, page order, nearby detail callouts, material codes, standards, or model numbers.
- If a sheet has no drawing number, use its actual drawing name. If neither can be identified reliably, leave it for review and ask one concise question at a time rather than inventing a name.
- Use the title block's number only; do not add extra labels such as "会签" or "手写签名" unless the user requests them.
- Keep source files unchanged. Do all edits on working copies. Never overwrite an existing output; resolve name collisions with a stable suffix such as _2 and record the collision.
- Sanitize characters that Windows forbids in filenames without changing the drawing number's meaningful characters. Compare output names case-insensitively.
- If duplicate source filtering is requested, treat files as duplicates only when their contents have the same SHA-256 hash. Matching names alone are not enough.
- Before processing, inventory inputs and identify ambiguous, unreadable, encrypted, or unsupported files. Keep a source-to-output manifest with the source, sheet/page or frame, observed drawing number, chosen filename, and status.

## DWG mode

Read [references/dwg-workflow.md](references/dwg-workflow.md) when splitting DWG files. Use the installed AutoCAD automation skill for native drawing access and preservation. Use explicit frames when present. If a drawing has no frame, infer a virtual sheet region from its title block or drawing-number anchor, associated views/notes/dimensions, and clear whitespace between neighboring drawings. Do not skip a frameless drawing solely because it lacks a rectangle. If the region is ambiguous, ask before splitting. Adding an actual rectangle to a working copy is a separate choice and should be done only when the user asks.

## PDF mode

Read [references/pdf-workflow.md](references/pdf-workflow.md) when splitting PDFs. Follow the installed PDF skill's authoring and verification requirements. By default, split multipage PDFs when each page represents one independent drawing. If one page contains several distinct drawings without borders, use their title blocks, nearby views/annotations, and clear whitespace to infer crop regions; crop only when the groups are clear. A one-page PDF containing one drawing is already a single sheet and can be skipped unless the user asks to collect it into the output. Inspect pages visually to map each region to its printed drawing number; page order and source filename are not naming evidence.

## Delivery check

Before reporting completion, confirm the source files remain unchanged, every produced file opens, and each output filename agrees with the drawing number visible on that sheet. Visually inspect the split boundaries and representative or exceptional outputs. Report the number of source files examined, sheets successfully split, skipped or ambiguous items, and the flat output directory. Do not claim a check that was not performed.
