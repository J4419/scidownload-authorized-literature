# Implementation notes

Read this only when changing the bundled Python implementation.

## Current bundled baselines

- `SCIDownload.py`: version 1.6.1.
- `FindDOI.py`: version 1.3, cache schema 2.
- FindDOI defaults: `--threshold 0.97`, `--review-threshold 0.80`.
- `check_exit_ip.py`: HTTPS-only public IP lookups, masked display by default, explicit `--show-full-ip` opt-in.

Do not lower the title-matching thresholds or restore an older cache schema without a fresh validation set that includes near-duplicate scholarly titles.

## Browser and CDP invariants

- Use a dedicated persistent non-default Chromium profile so remote debugging and authorized institutional login can persist.
- Bind `--remote-debugging-address=127.0.0.1`; never expose CDP to the network.
- Test that the requested loopback port is free before launch. Do not attach to an unrelated browser instance.
- Bypass environment proxies only for local CDP calls; public FindDOI metadata lookup follows system proxy settings unless `--direct` is explicit.
- Reuse one tab and do not add parallel workers.

## File invariants

Sanitize DOI/remote filenames into one safe filename component, enforce output containment, validate bytes/expected length, and replace atomically. A failed attempt must not overwrite an existing valid target.

PDF success requires a valid `%PDF-` header and the configured output file. Browser-default downloads, page HTML, viewer shells, and tiny/error payloads do not count.

## Publisher routing

Keep ScienceDirect logic separate from the non-ScienceDirect dispatcher. Dedicated publisher candidates are merged with generic candidates from `citation_pdf_url` and explicit PDF-like links; every candidate still passes validation.

MDPI is a special caution: version 1.6.1 contains attachment-download capture logic, but the maintainer's real Windows/Chrome test still observed cases where the browser downloaded outside the configured output and SCIDownload recorded failure. Preserve that limitation in user-facing claims until a future implementation is verified on real MDPI runs.

## State distinctions

Keep incomplete render, manual login, invalid DOI, no entitlement, unsupported publisher, persistent verification, and local exception separate. Retry only states that can plausibly change. Do not convert a terminal access or routing state into aggressive retries.
