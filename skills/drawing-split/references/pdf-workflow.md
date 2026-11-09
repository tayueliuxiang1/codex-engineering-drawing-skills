# PDF splitting workflow

Use this reference for multipage engineering PDFs. Read and follow the installed PDF skill, including any environment-specific authoring marker or output-count rules, before the first write. Use a local PDF library or application that can extract original pages without turning them into screenshots.

## Map source pages to drawings

1. Inventory PDF files, page counts, page dimensions, rotations, encryption status, and any digital-signature indication. Keep source files unchanged.
2. Render pages or title-block crops at readable resolution. Inspect every page's printed title block and record an explicit mapping of source file and page number to drawing number. Scanned pages may need visual inspection; extracted text alone is not reliable.
3. Preserve the exact printed drawing number, including prefix, letters, separators, and leading zeros. Sheet numbering may be out of order, and one PDF may contain pages from more than one drawing-code series. Never infer a code from page index or source filename.
4. By default, a multipage PDF page is one sheet. If a page contains multiple distinct drawings, with or without drawn borders, find their title blocks or drawing-number/name anchors, group nearby views/notes/dimensions with each anchor, and use clear whitespace gutters to infer crop regions. Split into page crops only when each region contains one coherent drawing with no ambiguous shared content. If the grouping is unclear, show candidate crops and ask the user. Do not crop a page containing one drawing simply because it lacks an outer border.
5. Skip one-page PDFs by default because they already contain one sheet. Include or rename them only if the user asks to collect all single-page files too.

## Extract pages

- Copy each page as an original PDF page object, preserving vector content, text, page size, scan quality, stamps, and visible signatures. Avoid rendering a page to an image and wrapping that image in a new PDF.
- Write one page per output PDF, directly in the flat output directory. Use the title-block drawing number; use the actual drawing name if no number is printed.
- For a page with multiple clearly separate drawings, crop the original PDF page to one verified region per output while preserving vector and raster content. Include all associated dimensions, notes, and title information; exclude neighboring drawings. Use a PDF crop/page-boundary operation, not a screenshot, and visually verify every crop. If the available tool cannot preserve the source page safely, ask before rasterizing or using a lossy workaround.
- Do not add approval/signature labels to filenames unless requested. When output names collide, add a stable suffix and record the duplicate code in the manifest.
- If the source contains a cryptographic digital signature, page extraction creates a different document and may invalidate that signature. Report this before proceeding with signed files when the request requires preserving signature validity. Scanned or visible ink stamps and signatures should remain in the extracted page.

## Verify

- Reopen every output PDF and confirm it contains exactly one expected page and has the expected page dimensions and orientation.
- Render each output and compare it with the corresponding original source page. Check that title blocks, dimensions, notes, borders, visible seals, and handwritten marks are not clipped or omitted.
- Confirm output filenames against the visual title blocks, not the page sequence. Check page-to-output counts against the manifest and the files on disk.
- Report skipped one-page files, ambiguous pages with multiple frames, unreadable or encrypted PDFs, and signature-related limitations separately. Do not claim that a cryptographic signature remains valid after extraction.
