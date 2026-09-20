---
name: scidownload-authorized-literature
description: Use when a user needs authorized scholarly PDFs or supplementary files from DOI or title lists, title-to-DOI resolution, SCIDownload resume or retry help, or diagnosis of institutional browser access across supported publishers.
---

# SCIDownload authorized literature

## Overview

Use the bundled Python tools only for material the user is already entitled to access, such as open-access content or content available through an authorized institution/login. Do not turn an access failure into instructions for bypassing a paywall, verification system, rate limit, or subscription requirement.

## Workflow

1. If the input contains titles but no DOI, run `python scripts/FindDOI.py <input>`. Only rows classified as automatically accepted receive a DOI; leave review rows for human confirmation.
2. Network diagnosis is optional. Run `python scripts/check_exit_ip.py` only when access routing is in question; its result is advisory, not proof of entitlement.
3. Trial at most three records first: `python scripts/SCIDownload.py <dois> --out <output> --limit 3 --si`.
4. Ask the user to verify that saved PDFs open and that the access route is authorized before a larger batch.
5. Resume with the same command. Trust a PDF only after on-disk validation; the manifest alone is not sufficient.

If this runtime cannot launch a local Chromium browser or execute local Python, do not claim the download was run. Give the exact command and interpret the user's returned output instead.

## Stop conditions

Stop and report the observed status when access is denied, login needs user interaction, a DOI is invalid, verification persists, the publisher is unsupported, or the requested volume conflicts with publisher/institution rules. Do not respond by adding concurrency, exposing the CDP port, accepting optional tracking consent on the user's behalf, or weakening validation.

## Quick reference

| Need | Action |
|---|---|
| Titles → DOI | `python scripts/FindDOI.py <input>` |
| Network path check | `python scripts/check_exit_ip.py` |
| Small download trial | `python scripts/SCIDownload.py <input> --out <output> --limit 3 --si` |
| Cache preview | `python scripts/ClearCache.py --dry-run` |
| Normal cache cleanup | `python scripts/ClearCache.py` |

## Sensitive data

Treat the dedicated browser profile, cookies, signed URLs, proxy credentials, logs, manifests, DOI lists, and screenshots as potentially sensitive. Never ask the user to upload a browser profile or cookies. Public IPs are masked by default in the bundled diagnostic; do not request `--show-full-ip` unless the user explicitly needs it and understands the exposure.

## Routed references

- Read [references/troubleshooting.md](references/troubleshooting.md) for login, cookie-consent, network, entitlement, verification, cache, and port failures.
- Read [references/manifest-and-si.md](references/manifest-and-si.md) for resume state, JSONL records, PDF integrity, and SI completeness.
- Read [references/multi-publisher.md](references/multi-publisher.md) for publisher routing, generic fallback, MDPI limitations, and `unsupported_publisher`.
- Read [references/implementation-notes.md](references/implementation-notes.md) only when changing the Python/CDP implementation.
- Read [references/release-maintenance.md](references/release-maintenance.md) only when updating or repackaging this skill.

Use `python scripts/<name>.py --help` for current CLI flags.
