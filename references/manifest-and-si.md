# Manifest, resume, and supplementary files

## Latest record wins

`manifest.jsonl` is append-only. Parse valid JSON lines in order, normalize DOI to lowercase, and let every later record replace the earlier record for that DOI, including a later failure. Ignore malformed lines without discarding the rest of the file.

Resume decisions must verify the PDF on disk:

- valid PDF and requested SI already complete: `skip_all`;
- valid PDF but SI was not requested or is incomplete: `si_only`;
- missing, too small, or invalid PDF: `full`.

Do not rewrite history just to migrate legacy records.

## Relevant fields

- `status`: overall/article result kept for compatibility.
- `pdf_status`: `downloaded` only after file validation.
- `si_requested`: whether that attempt requested SI.
- `si_status`: `not_requested`, `downloaded`, `none_found`, `partial`, or `failed`.
- `pdf` and `si`: relative output filenames; never trust them as unrestricted filesystem paths.

For MDPI or any attachment-style route, a PDF that exists only in the browser's default Downloads folder does not satisfy resume state. The configured output must contain a valid PDF.

## Publisher placeholder packages

Elsevier can return a valid but contentless supplementary package for an article with no independent supplement, such as a tiny ZIP containing only `Data Profile.xml`. Treat this as a terminal `none_found` state, not a fetch failure. Do not waste retry rounds on it.

## SI integrity

For Elsevier, independent supplementary assets use `-mmc<N>.` links; do not confuse article figures or high-resolution images with SI. Preserve the `mmc` number in filenames. Reject files below the program's minimum size, HTML error pages, invalid PDF headers, and invalid ZIP/Office signatures. Downloads are written through temporary files and replaced only after validation.

SI support varies by publisher. Absence of an exposed independent SI link is not proof that supplementary material never existed; report the program's observed `none_found` state precisely.
