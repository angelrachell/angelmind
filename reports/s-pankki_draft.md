# [ACTIVE] S-Pankki — Bug Bounty Research Draft

**Program:** S-Pankki (S-Bank Finland)
**Platform:** HackerOne
**URL:** hackerone.com/s-pankki
**Researcher:** angelmind
**Date:** 2026-09-25
**Status:** ACTIVE RECON — Findings confirmed, ready for validation

## Title

CORS Wildcard + SameSite=None Cookie on Netbank Domain

## Vulnerability Type

CORS Misconfiguration + Cookie Security (SameSite=None)

## Executive Summary

**PRIMARY FINDING:** CORS wildcard (`Access-Control-Allow-Origin: *`) on root domain `online.s-pankki.fi` only — /ebank/ API endpoints do NOT have CORS wildcard. Root domain returns HTML redirect page (no sensitive data). Browser does NOT send credentials with wildcard (`*`). Policy: "Unauthenticated CSRF" is out of scope.

**Secondary findings:** CSRF token anomaly (standard Struts2), mTLS banking API, Keycloak admin on public subdomain, weak CSP on public pages.

## Affected Asset

- Program: S-Pankki Bug Bounty Program
- Asset/hostname: `online.s-pankki.fi` (netbank domain)
- Endpoint/function: `/ebank/` API endpoints (CORS wildcard applies)
- Environment: Production
- Root cause: CORS header set to `*` on banking domain with cross-site cookie

## Severity

- FINDING-1 (CORS wildcard): **P3/Low** — root domain only, no sensitive data, no credentials sent
- FINDING-3 (mTLS): **P2/Medium** — banking API client cert required
- FINDING-4 (Keycloak admin): **P2/Medium** — IP-restricted but publicly visible
- FINDING-6 (subdomain exposure): **P3/Medium** — scope management issue
- FINDING-5 (API Gateway): **P3/Low-Medium** — custom auth review
- FINDING-2 (CSRF token): **Informational** — standard Struts2 behavior
- FINDING-7 (Weak CSP): **P4/Low** — public pages only

## Preconditions

- Authentication required: Yes (login required for /ebank/ endpoints)
- Privileges required: None (CORS applies to all origins)
- User interaction: Active (victim must visit malicious page while logged in)
- Special setup: Demo credentials available for S-Pankki testing
- Testing header: `@wearehackerone` email in User-Agent

## Proof of Concept

### FINDING-1: CORS Wildcard (Root Domain Only)

**Step 1 — Root domain (CORS wildcard):**
```bash
curl -sI -H "Origin: https://evil.com" https://online.s-pankki.fi
```
**Result:** `Access-Control-Allow-Origin: *` ✅ CONFIRMED

**Step 2 — /ebank/ API (NO CORS):**
```bash
curl -sI -H "Origin: https://evil.com" https://online.s-pankki.fi/ebank/auth/initLogin.do
```
**Result:** No CORS header ✅ CONFIRMED

**Step 3 — Login endpoint (OPTIONS rejected):**
```bash
curl -sI -X OPTIONS -H "Origin: https://evil.com" https://online.s-pankki.fi/ebank/auth/loginEbank.do
```
**Result:** 405 Method Not Allowed ✅ CONFIRMED

**CONFIRMED:** CORS wildcard only on root domain. Root returns redirect HTML. /ebank/ APIs not affected. No credentials sent with wildcard.

### FINDING-7: Weak CSP

**Step 1 — Check CSP:**
```bash
curl -sI https://www.s-pankki.fi 2>/dev/null | grep -i content-security
```
**Result:** `script-src 'self' 'unsafe-inline' 'unsafe-eval'` ✅ CONFIRMED

## Confirmed Impact

- FINDING-1: CORS wildcard on root domain only — LOW impact
- FINDING-2: CSRF token standard behavior — informational
- FINDING-3: mTLS banking API — MEDIUM potential risk
- FINDING-4: Keycloak admin exposed — MEDIUM
- FINDING-5: API Gateway custom auth — LOW-MEDIUM
- FINDING-6: Subdomain overexposure — MEDIUM scope issue
- FINDING-7: Weak CSP on public pages — LOW
- Confidentiality: Low-Medium depending on finding
- Integrity: Low-Medium
- Availability: N/A
- Attacker capability: Varies by finding

## Why This Priority

[To be filled]

## Limitations

- No destructive testing
- Demo credentials only for own testing
- Out-of-scope assets not tested
- DoS testing not performed
- CORS impact requires authenticated session
- CVSS estimated, not calculated from official calculator

## Remediation

1. Remove `Access-Control-Allow-Origin: *` from netbank domain responses
2. Set `Access-Control-Allow-Origin` to specific trusted origins only
3. Change `SameSite=None` to `SameSite=Strict` or `SameSite=Lax` on `keycross` cookie
4. Add `Access-Control-Allow-Credentials: true` only for trusted origins
5. Implement CORS preflight validation for all `/ebank/` endpoints

## Evidence

- Timestamp/timezone: 2026-09-25 UTC
- Tool and command: `curl -sI -H "Origin: https://evil.com" https://online.s-pankki.fi`
- Raw evidence: `/workspaces/angelmind/angelmind_findings.md`
- Screenshot/video: N/A (terminal test)
- Redaction notes: None needed

## Disclosure Notes

- Follow HackerOne S-Pankki program policy
- No public disclosure before authorization
- 90-day disclosure timeline (standard)

## Duplicate Risk

- Program checked: Yes (88 reports resolved)
- Sources checked: [TO VERIFY]
- Candidate duplicates: [TO CHECK]
- Exact duplicate/variant/systemic/independent: [TO CLASSIFY]
- Shared-platform impact: N/A
- Duplicate risk: low/medium/high/unknown
- Manual verification required: Yes

---

*This is a template. Fill in after safe reproduction and evidence collection.*
*Do not submit until human final-review gate is completed.*