# Multi-publisher routing

## Current status

The bundled SCIDownload script is version 1.6.1. ScienceDirect remains the primary path. Dedicated DOI routes also exist for Wiley-family prefixes, Canadian Science Publishing, CSIRO, Copernicus, PeerJ, Nature Portfolio / Scientific Reports, and MDPI. Unknown non-ScienceDirect pages may also be tried through standard `citation_pdf_url` metadata or explicit PDF-like links; candidate bytes still have to pass PDF validation.

| Status | Publisher / prefix | Agent guidance |
|---|---|---|
| Stable in prior real runs | Elsevier / ScienceDirect (`10.1016/*`) | Use the ScienceDirect path. |
| Stable in prior real runs | Wiley family (`10.1002`, `10.1111`, `10.2134`, `10.2136`) | Preserve the actual Wiley subdomain and prefer `pdfdirect`. |
| Stable in prior real runs | Canadian Science (`10.1139`, `10.4141`) | Dedicated PDF route. |
| Stable in prior real runs | CSIRO (`10.1071`) | Use page metadata and follow the signed redirect. |
| Stable in prior real runs | Copernicus (`10.5194`) | Page metadata normally exposes the PDF directly. |
| Verified in the public-release testing described by the maintainer | PeerJ (`10.7717`) | Dedicated route for standard `peerj.<id>` DOIs plus generic fallback. |
| Verified in the public-release testing described by the maintainer | Nature Portfolio / Scientific Reports (`10.1038`) | Dedicated Nature article-PDF route plus generic fallback. |
| **Experimental / known limitation** | MDPI (`10.3390`) | A browser may download the file while SCIDownload still fails to capture it into its output or mark it downloaded. Never report success unless the configured `PDFs/` directory contains a valid PDF and the run record reflects success. A file seen only in the browser's default Downloads folder is not SCIDownload success. |

## Known unsupported patterns in the bundled script

The current script short-circuits these as unsupported rather than repeatedly retrying: Taylor & Francis (`10.1080`), Springer (`10.1007`), Cambridge (`10.1017`), Science Press/sciengine (`10.3724`), 生态学报 (`10.5846`), and SSRN (`10.2139`). Other unknown publishers may reach the generic fallback and still end as `unsupported_publisher` if no valid PDF can be obtained.

## Distinguish failure types

- `no_entitlement`: the current authorized session does not have full-text rights. Do not propose bypasses.
- `needs_manual_login`: the user must complete an authorized institutional login in the dedicated browser.
- `bad_doi`: correct the input; retries do not help.
- `challenge_not_resolved`: stop, wait, reduce volume, and avoid concurrent instances.
- `unsupported_publisher`: current routing did not obtain a valid PDF; do not relabel this as a subscription problem.

Do not infer success from a visible browser download animation, a browser history entry, or an unvalidated file. The success criterion is the program's validated output.
