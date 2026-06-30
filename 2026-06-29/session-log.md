# Bug Bounty Session — June 29, 2026 (WordPress)

**Target:** WordPress (HackerOne program)
**HackerOne User:** stix26

**Token Management:** Stored in `$H1_TOKEN` env var (never logged to disk)

---

## Recon Pipeline

### 1. Program Verification
- WordPress program: active, public mode, offers bounties
- Scope: `*.wordpress.org`, source code repos, Gutenberg, BuddyPress, bbPress, WP-CLI, GlotPress, WordCamp.org
- WordPress.com explicitly out of scope (Automattic program)

### 2. Subdomain Enumeration
- `subfinder -d wordpress.org` → **23,295 subdomains** (~49s)
- Filtered to 1,127 official/service subdomains
- Identified primary targets: api, codex, profiles, make, developer, login, core, download, translate, trac, blog, news, events, docs, help, support, status, security, scan

### 3. Live Host Probing
**Accessible (200):** wordpress.org, make.wordpress.org, events.wordpress.org, status.wordpress.org
**Redirect (301/302):** www, api, profiles, core, download, trac, blog, news, jobs
**Rate-limited (429):** developer, login, translate, docs, help, support, scan, central

### 4. Nuclei Scanning
- Templates: WordPress CVE + misconfiguration
- Targets: wordpress.org, status.wordpress.org
- **Result: 0 matches**
- WordPress detected on status.wordpress.org (WordPress-detect template)

### 5. Version Detection
- api.wordpress.org reports latest stable: **WordPress 7.0**
- wordpress.org behind Cloudflare/WP.com infrastructure
- status.wordpress.org runs WordPress with wp.com CDN assets

### 6. CVE Research
- Checked CIRCL and NVD databases for May-June 2026
- No recent WordPress-specific CVEs found

### 7. GitHub Commit Review
- Checked `WordPress/WordPress` recent commits
- No security-related patches in the last 30 commits

### 8. Manual Checks
- `/readme.html`: exists on wordpress.org
- `/wp-json/`: rate-limited
- `/wp-admin/`: 429 (rate-limited)
- `/wp-content/uploads/`: 429 (rate-limited)
- CSP headers: present with `frame-ancestors 'self'`

---

## Summary

| Step | Result |
|------|--------|
| Program status | Active, bounties offered |
| Subdomains found | 23,295 |
| Live targets | 9 accessible |
| Nuclei matches | 0 |
| Recent CVEs | None found |
| GitHub sec commits | None recent |
| **Actual bugs found** | **0** |

## Files Saved
- `~/Desktop/wordpress-recon/` — full recon folder
- `~/Desktop/wordpress-recon/targets.txt` — key target list
- `~/Desktop/wordpress-recon/subdomains.txt` — all wordpress.org subdomains
- `~/Desktop/wordpress-recon/scan-results.txt` — probe + scan output
- This document

---

## What I Learned
1. WordPress.org rate-limits aggressively — hard to scan directly
2. API endpoint (`api.wordpress.org`) is open and useful for version data
3. The WordPress program is well-maintained; no easy finds
4. Source code review is more promising than live-site scanning for this target

## Aggressive Testing Results

All false positives verified. Summary of each:

### CORS
- `developers.cloudflare.com` — Access-Control-Allow-Origin: * (docs site, intentional)
- `blog.cloudflare.com` — ACAO: https://dash.cloudflare.com (intentional)
- `www.djangoproject.com` — ACAO: https://code.djangoproject.com (intentional, Trac integration)

### Open Redirects (ALL FALSE POSITIVES)
wordpress.org returned 302 for `?url=`, `?redirect=`, `?next=`, `?return=`, `?to=`, `?dest=`, `?target=`
→ Verified: redirects to same page with URL-encoded param value. Not an open redirect.

### Exposed Files (ALL FALSE POSITIVES)
status.djangoproject.com returned 200 for .env, wp-config.php, config.json, dump.sql, etc.
→ Verified: Freshping status page returns HTML for all paths (custom catch-all).
wordpress.org/wp-config.php returned 0 bytes (empty body).

### API Endpoints
status.djangoproject.com returned 200 for /api/, /graphql, /rest/, /wp-json/
→ Freshping catch-all (same HTML status page for all).
developers.cloudflare.com/api/ → Cloudflare API docs, intentional.

### .git Exposure
All targets: .git/HEAD returns HTML (not git content). Not exposed.

### Wayback URLs
Gau failed on all three targets (tool error, not a finding).

### GitHub Secrets
No secrets found in recent commits of WordPress, Django, or workerd.
