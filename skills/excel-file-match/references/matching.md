# Identifier matching rules

## Read identifiers

- Read cell values as text, including numeric-looking values such as 0007. Do not let spreadsheet numeric formatting silently remove meaningful zeros.
- Match against filename stems after removing only the final extension. Preserve the exact source filename for the copy and the exact stem for any authorized cell correction.
- Use case-insensitive comparison, trim outside whitespace, normalize full-width Latin letters/digits and common dash characters, and ignore accidental spaces around separators. Do not remove meaningful letters, prefixes, suffixes, or interior digits.

## Allowed equivalences

Apply the narrowest rule that the user has confirmed. This public package contains no preconfigured project alias map:

| Difference | Default handling |
| --- | --- |
| Configured prefix alias, with the remaining identifier identical | Treat as equivalent only when the current task or an approved alias map confirms that prefix pair. |
| Zero-padded serial difference, with all other identifier characters identical | Treat as equivalent only when the current task or an approved alias map confirms the affected numeric segment. Preserve the file's exact spelling when aligning the workbook cell. |
| Added or removed suffix | Treat as equivalent only for an explicitly confirmed, row-specific alias. Do not generalize one suffix exception to other identifiers. |
| Any other prefix/suffix difference, swapped digits, or changed code character | Do not auto-match. Show the candidate and ask whether it is an alias. |

Do not infer that two IDs are equivalent just because they look similar. A digit transposition such as AB-123 versus AB-132 is a different identifier unless the user explicitly corrects it. Do not use broad substring matching that would collapse codes with revision letters, product variants, or neighboring serial numbers.

## Reconcile both directions

- Build the inventory of in-scope files before deciding the final unmatched set. An Excel row with no file is an Excel-only record; a file with no workbook row is a file-only record that may need a new row, not a failed match.
- Use project number plus mold number as the record key when both are available. Do not collapse two projects just because their mold numbers match. If the project number is unknown, use the confirmed mold number and flag possible duplicates across projects for review.
- Group different requested file formats with the same confirmed project/mold key under one workbook record when the workbook layout supports one record per mold. Copy each requested file format once.
- Only mark a row as present after the source file has been verified and either matched to an existing row or added as a new record.

## Multiple candidates and collisions

- If several files share the same confirmed identifier but have different included extensions, copy all requested types and report them as one row matching multiple files.
- If several files of the same type match one identifier, list each candidate with its source path and distinguishable metadata. Do not choose one based on folder order when the versions differ.
- If two workbook rows share one identifier, mark every row whose value is confirmed as a match; copy the physical file only once unless the user wants separate copies.
- In a flat destination, never overwrite a same-name file. Compare SHA-256 hashes: if identical, keep one copy; if different, preserve the existing destination file and report the conflict for resolution. Never delete a source file as duplicate cleanup unless the user explicitly asked for deletion.

## Manifest states

Track each row as one of:

- Exact match
- Confirmed alias match, with the specific rule recorded
- Missing file
- Ambiguous candidate requiring review
- Excluded by file-type or search-scope settings

Do not combine "missing" with "ambiguous". Only exact and user-confirmed alias matches count as found for copy and highlighting.
