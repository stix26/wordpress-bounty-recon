# Bug Bounty Recon — WordPress (HackerOne)

Each bounty program gets its own repo.
Within each repo, attempts are organized by date:

```
{program}/
├── {YYYY-MM-DD}/
│   ├── session-log.md       # Full narrative of what was done
│   ├── targets.txt           # Scope targets enumerated
│   ├── subdomains.txt        # Subdomain list
│   ├── scan-results.txt      # Probe + scanner output
│   ├── ...
├── README.md                 # This file — index of all attempts
```

## Sessions

| Date | Target | Outcome | Notes |
|------|--------|---------|-------|
| 2026-06-29 | WordPress.org | No bugs found | Rate-limited, 0 nuclei matches, no recent CVEs |

> Someone may find a bug later using this recon as a starting point.
> The subdomain list, live hosts, and scan data are preserved here for reference.