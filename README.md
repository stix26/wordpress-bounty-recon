# WordPress Bug Bounty Reconnaissance

Security research repository targeting the [WordPress HackerOne program](https://hackerone.com/wordpress).  
This repo documents automated reconnaissance runs, methodology, and raw data for future reference.

## Repository Structure

```
├── {YYYY-MM-DD}/           # Date-stamped session directory
│   ├── session-log.md      # Narrative of tools, findings, and blockers
│   ├── targets.txt         # In-scope asset list for the session
│   ├── subdomains.txt      # Subdomain enumeration output
│   ├── scan-results.txt    # Probe responses and scanner results
├── README.md               # (this file)
```

Each session is fully self-contained. Raw data is preserved so that a vulnerability discovered later can be traced back to the reconnaissance that enabled it.

## Scope

WordPress Core, Gutenberg, BuddyPress, bbPress, WP-CLI, GlotPress, WordCamp.org, and associated infrastructure under `*.wordpress.org`, `*.buddypress.org`, `*.wordcamp.org`, and related source code repositories.  
WordPress.com is out of scope (see [Automattic's program](https://hackerone.com/automattic)).

## Sessions

| Date | Focus | Outcome | Notes |
|------|-------|---------|-------|
| 2026-06-29 | WordPress.org live infrastructure | No vulnerabilities identified; 23,295 subdomains enumerated; rate-limited on most paths | WordPress 7.0 behind Cloudflare/WP.com; 0 nuclei matches; no recent CVEs |

## Tools

- [subfinder](https://github.com/projectdiscovery/subfinder) — subdomain enumeration
- [httpx](https://github.com/projectdiscovery/httpx) — HTTP probing
- [nuclei](https://github.com/projectdiscovery/nuclei) — template-based vulnerability scanning
- [HackerOne API](https://api.hackerone.com/) — program scope and report submission

## Disclaimer

This repository contains output from automated security reconnaissance tools.  
No vulnerabilities were verified or exploited. All work was performed within the scope of the WordPress HackerOne program and in compliance with its policies.