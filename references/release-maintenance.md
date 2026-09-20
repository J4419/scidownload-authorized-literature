# Release maintenance

## Source of truth

The public GitHub-ready SCIDownload release is the source of truth for script behavior. When updating this skill, synchronize the tested public scripts into `scripts/` using canonical skill filenames:

| Public release file | Skill file |
|---|---|
| `1.check_exit_ip.py` | `scripts/check_exit_ip.py` |
| `2.FindDOI.py` | `scripts/FindDOI.py` |
| `3.SCIDownload.py` | `scripts/SCIDownload.py` |
| `4.ClearCache.py` | `scripts/ClearCache.py` |
| `SCIDownload_cdp.py` | `scripts/SCIDownload_cdp.py` |

The rename is packaging-only. The skill copy may replace only self-referential CLI example strings such as `python 3.SCIDownload.py` with the canonical skill filename `python SCIDownload.py`; do not change runtime behavior. If the skill needs different behavior, change and test the public source first, then resynchronize.

## Update checklist

1. Confirm the public scripts are the intended release and record their versions.
2. Copy the five files above into `scripts/`.
3. Update references only where behavior actually changed: publisher support, statuses, FindDOI thresholds, privacy behavior, or CLI examples.
4. Compile every Python file and run `--help` for the user-facing CLIs.
5. Run the skill-package validation checks: required structure, frontmatter, current version markers, privacy-hardened IP diagnostic, current FindDOI thresholds, and no stale publisher claims.
6. Make one real authorized small-batch check before claiming a publisher route works. Unit/static checks cannot prove publisher access.
7. Rebuild the skill ZIP and inspect its entry names before publishing.

## Redaction gate

Never ship browser profiles, cookies, login databases, manifests, run logs, downloaded papers, DOI lists, proxy credentials, tokens, personal email addresses, institution-specific network ranges, local absolute paths, or usernames. Example values must be clearly synthetic.

Review user-facing claims separately from secret scanning. A package can contain no secrets and still be misleading if it advertises publisher behavior that was not actually verified.
