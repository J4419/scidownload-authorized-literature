# Troubleshooting

## Diagnose by observed state

| Observation/status | Meaning | Action |
|---|---|---|
| A validated PDF appears in the configured output | The actual route succeeded | Prefer this evidence over IP/ASN guesses. |
| `no_entitlement` and the article shows Get access / purchase | Current session lacks rights | Check authorized institutional access; stop if none. |
| `needs_manual_login` | Institutional authentication is waiting | Re-run with `--login-wait 300` and let the user complete the login. |
| `bad_doi` | DOI did not resolve correctly | Correct the input; repeated retries do not help. |
| `challenge_not_resolved` | Browser verification did not settle | Stop, wait, reduce volume, and avoid concurrent runs. |
| `unsupported_publisher` | No supported/generic route produced a valid PDF | Read `multi-publisher.md`; do not call it a subscription failure. |
| Debug port occupied | Another local process owns the port | Close the stale dedicated browser or choose another `--port`. |

## Network diagnostics

`check_exit_ip.py` is optional and advisory. It reports the Python process's route, which may differ from the browser route because of proxy rules, CARSI, EZproxy, institutional VPN, or other routing. It uses HTTPS endpoints and masks public IPs by default. Do not infer “no subscription” solely from country or ASN.

FindDOI is different: it queries public metadata services and follows system proxy settings unless `--direct` is explicit.

## Cookie / consent banners

A visual cookie banner does not necessarily block PDF discovery. If a site genuinely withholds article/PDF markup until a consent choice is made, ask the user to make their own choice in the dedicated browser. Do not automatically select “Accept all” or consent to optional tracking on the user's behalf.

## MDPI attachment symptom

Known symptom: the browser visibly downloads an MDPI PDF, but SCIDownload reports failure and the configured `PDFs/` directory is empty. Treat this as the known experimental MDPI capture limitation, not as success. Do not tell the user to change their everyday browser download directory as a normal requirement.

## Cache and login state

Run `python scripts/ClearCache.py --dry-run` before cleanup when the target is unfamiliar. Normal cleanup preserves login data and cookies. `--all` removes the dedicated profile and therefore requires a new login. Custom-profile deletion additionally requires `--confirm-custom-profile`; broad or shallow paths are rejected.

Never ask the user to upload their browser profile, cookies, login database, or signed download URLs for debugging.
