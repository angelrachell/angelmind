# [ACTIVE] GoodRx — Bug Bounty Research Draft

**Program:** GoodRx
**Platform:** HackerOne
**URL:** hackerone.com/goodrx
**Researcher:** angelmind
**Date:** 2026-09-25
**Status:** ACTIVE RECON — Findings confirmed

## Title

API Auth Info Disclosure + Expired Cookie

## Vulnerability Type

Information Disclosure (API auth mechanism) + Cookie Misconfiguration

## Executive Summary

**FINDING-G1:** API auth info disclosure — `api.goodrx.com/v1/health` returns `{"error":{"type":"authentication_error","detail":"missing api key","code":"unauthorized"}}` — reveals API key auth mechanism.

**FINDING-G2:** Expired cookie — `fastly_unique_id` cookie has expired date (Aug 2019).

## Affected Asset

- Program: GoodRx Bug Bounty Program
- Asset/hostname: `api.goodrx.com`
- Endpoint/function: `/v1/health` and `/v1/prices`
- Environment: Production (Varnish cache)
- Root cause: API error message reveals auth mechanism

## Severity

- FINDING-G1: P3 — API auth info disclosure
- FINDING-G2: informational — expired cookie
- Impact severity: Low
- CVSS: N/A (info disclosure)
- Confidence: High — verified via curl

## Preconditions

- Authentication required: No (public API endpoint)
- Privileges required: None
- User interaction: None (passive)
- Testing header: none required

## Proof of Concept

### FINDING-G1: API Auth Info Disclosure

**Step 1 — Baseline:**
```bash
curl -sI https://api.goodrx.com/v1/health
```

**Step 2 — Request:**
```bash
curl -s https://api.goodrx.com/v1/health
```

**Step 3 — Result:**
```json
{"error": {"type": "authentication_error", "detail": "missing api key", "code": "unauthorized"}}
```

**CONFIRMED:** API reveals auth mechanism (API key). Error message informative.

### FINDING-G2: Expired Cookie

**Step 1 — Check cookies:**
```bash
curl -sI https://goodrx.com
```

**Step 2 — Result:**
```
Set-Cookie: fastly_unique_id=[REDACTED]; expires=Mon, 19-Aug-2019[REDACTED]
```

**CONFIRMED:** Cookie with expired date (2019).

## Confirmed Impact

- FINDING-G1: Information disclosure — API auth mechanism revealed
- FINDING-G2: Minor — expired cookie (2019) indicates old/misconfigured code
- Confidentiality: Low — no user data exposed
- Integrity: N/A
- Availability: N/A

## Remediation

- FINDING-G1: Return generic 401 without revealing auth mechanism details
- FINDING-G2: Remove expired cookie or update expiration date

## Evidence

- Timestamp/timezone: 2026-09-25 UTC
- Tool and command: `curl -s https://api.goodrx.com/v1/health`
- Raw evidence: `/workspaces/angelmind/angelmind_findings.md`
- Screenshot/video: N/A (terminal test)
- Redaction notes: None needed

## Limitations

- No destructive testing
- No third-party data accessed
- Out-of-scope items not tested (Auth0 rate-limiting, third-party software)
- DoS testing not performed
- API key format not brute-forced (rate limit)

## Disclosure Notes

- Follow HackerOne GoodRx program policy
- No public disclosure before authorization
- 90-day disclosure timeline (standard)

## Duplicate Risk

- Program checked: Yes (216 reports resolved)
- Sources checked: [TO VERIFY]
- Candidate duplicates: [TO CHECK]
- Exact duplicate/variant/systemic/independent: [TO CLASSIFY]
- Shared-platform impact: N/A
- Duplicate risk: low/medium/high/unknown
- Manual verification required: Yes

---

*This is a template. Fill in after safe reproduction and evidence collection.*
*Do not submit until human final-review gate is completed.*