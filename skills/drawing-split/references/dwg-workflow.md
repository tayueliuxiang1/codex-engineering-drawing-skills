# DWG splitting workflow

Use this reference for files that contain several sheets inside one DWG, whether or not the sheets have drawn borders. Read the installed AutoCAD automation skill before using AutoCAD, its .NET API, a command-line core console, AutoLISP, or a conversion route. Check the installed AutoCAD version and available runtime; do not assume an AutoCAD executable path or plug-in compatibility from an earlier job.

## Inspect before splitting

1. Inventory the source files and record their paths and SHA-256 hashes. Open or copy each source to a disposable working area; never save changes to the source.
2. Inspect model space, paper space, and layouts. Identify explicit sheet frames and title blocks first. A DWG's overall extents may cover many separate sheets, and a large outside border may enclose multiple sheets.
3. Use title-block placement and visible page composition to distinguish sheet borders from detail rectangles, title-block cells, and enclosing borders. Do not treat every closed rectangle as a sheet.
4. Read the printed drawing number for each sheet from its title block. Check a visual preview at readable scale. Material identifiers, referenced part numbers, standards, and page sequence are not sheet identifiers.
5. If a sheet has no explicit frame, use the unframed-sheet procedure below to infer a virtual boundary. A drawing is not excluded just because its border is missing.
6. Confirm that each explicit or virtual boundary includes all associated views, dimensions, notes, and the title block without capturing content from its neighbor. If ownership remains uncertain, stop that sheet for review rather than clipping it silently.

## Unframed drawings

When there is no usable drawn frame, assign entities to a sheet using several independent cues rather than one proximity test:

1. **Find anchors.** Prefer a unique title block, printed drawing number, or drawing name. If no title block exists, look for a clear drawing-name or identification note within the sheet cluster.
2. **Group related content.** Associate views, section profiles, dimensions, leaders, notes, and detail labels with the nearest plausible anchor, considering alignment, scale, annotation links, block groups, and the page's visual composition. Treat a drawing as a group: its views and notes may be disconnected CAD entities.
3. **Infer the boundary.** Use the grouped entities' native extents plus a small margin, and compare against clear whitespace gutters between neighboring groups. Preserve dimensions and text that extend beyond the profile geometry. Do not use overall DWG extents, centroid distance alone, or a single wide object to define every sheet.
4. **Check the candidate visually.** Render the whole area and proposed regions at readable scale. Confirm that the preview contains one coherent drawing, all its dimensions/notes/title information, and no part of a neighboring drawing. A contact sheet with temporary boundary overlays can make this check easier; keep overlays out of the delivered DWG unless requested.
5. **Act by confidence.** Split automatically when the anchor is unique, group membership is coherent, and visible gaps separate neighboring sheets. If one or more objects could belong to either sheet, or several title/name anchors conflict, show the proposed regions and ask the user. Do not silently discard or force-assign uncertain entities.

If no drawing number is legible, use the actual drawing name when clear. If neither the sheet grouping nor its name can be identified reliably, leave it in the review list and ask one concise question at a time.

The inferred boundary is virtual; it does not require adding geometry to the source or working copy. If the user separately asks for an actual independent border, make it only on the working copy, on a dedicated layer, and size it to include the complete sheet without overlap. Record any added border. Never change original geometry, scale, text, dimensions, layers, or block definitions as part of boundary inference.

## Produce one DWG per sheet

- Use AutoCAD-native database operations where possible so that dimensions, text, blocks, layers, and drawing coordinates stay editable. Avoid rasterizing or exploding entities as a shortcut.
- For each sheet, select the complete contents assigned to its verified explicit or virtual boundary. Exclude other sheets and unrelated entities. Preserve needed layer, text style, dimension style, block, and linetype definitions.
- Save each result directly into the requested flat output directory. Name it with the title-block drawing number, or the actual drawing name when the number is absent. When the same title-block number appears more than once, use a collision suffix and list both entries for review.
- Handle model-space frames and layouts according to how the source drawing is actually organized. Do not silently discard paper-space content or treat viewports as the drawing itself.

## Verify

- Reopen each result in a compatible AutoCAD runtime. Confirm it contains the intended sheet, required title block, notes, dimensions, and detail geometry, and no neighboring sheet.
- Check entity extents and frame coverage natively. For dimensions and blocks, verify they remain present and editable; do not rely only on entity counts.
- Render previews and visually inspect every ambiguous or exceptional split plus representative normal outputs. Font substitution can make a rendered label extend beyond its DWG bounding box; compare the preview and expand a frame only when the source sheet content confirms that the text belongs to it.
- Recompute source hashes and confirm originals are unchanged. Compare the manifest counts with the output files on disk. Record any drawing that failed to open or could not be split safely.
