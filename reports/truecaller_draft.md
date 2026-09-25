# [ACTIVE] Truecaller — Bug Bounty Research Draft

**Program:** Truecaller
**Platform:** HackerOne
**URL:** hackerone.com/truecaller
**Researcher:** angelmind
**Date:** 2026-09-25
**Status:** ACTIVE RECON — Limited findings

## Title

No Critical Findings — API Not Publicly Accessible

## Vulnerability Type

N/A — API endpoints not reachable from internet

## Executive Summary

**No critical findings from active testing.** API endpoints not publicly accessible (no response from api.truecaller.com). Known SDK ContentProvider issues documented for reference.

## Affected Asset

- Program: Truecaller Bug Bounty Program
- Asset/hostname: api.truecaller.com (no response)
- Endpoint/function: N/A (API not reachable)
- Environment: Unknown (behind Cloudflare)
- Root cause: N/A

## Severity

- No active findings from testing
- Known SDK ContentProvider issues (documented for reference)
- Impact severity: N/A
- CVSS: N/A
- Confidence: N/A

## Preconditions

- Authentication required: Unknown
- Privileges required: Unknown
- User interaction: Unknown
- Special setup: API not reachable from internet

## Proof of Concept

### API Reachability Test

**Step 1 — Check API:**
```bash
curl -sI --max-time 10 https://api.truecaller.com
```

**Step 2 — Result:**
```
(no response / timeout)
```

**Result:** API not publicly accessible. No active testing possible.

### Web Endpoint Test

**Step 1 — Check web:**
```bash
curl -sI --max-time 10 https://webapi.truecaller.com
```

**Step 2 — Result:**
```
(no response / timeout)
```

**Result:** Web API also not accessible from internet.

## Confirmed Impact

- No active findings from testing
- API not reachable from internet
- Known SDK ContentProvider issues (check for duplicates)
- Confidentiality/Integrity/Availability: N/A

## Remediation

- N/A — no active findings
- Check known SDK ContentProvider issues for duplicates

## Evidence

- Timestamp/timezone: 2026-09-25 UTC
- Tool and command: `curl -sI --max-time 10 https://api.truecaller.com`
- Raw evidence: `/workspaces/angelmind/angelmind_findings.md`
- Screenshot/video: N/A (terminal test)
- Redaction notes: None needed

## Limitations

- No destructive testing
- No third-party data accessed
- Out-of-scope items not tested
- DoS testing not performed
- API not reachable from internet — no active testing possible

## Disclosure Notes

- Follow HackerOne Truecaller program policy
- No public disclosure before authorization
- 90-day disclosure timeline (standard)

## Duplicate Risk

- Program checked: Yes (59 reports resolved)
- Sources checked: [TO VERIFY]
- Candidate duplicates: [TO CHECK] — known SDK ContentProvider issues
- Exact duplicate/variant/systemic/independent: [TO CLASSIFY]
- Shared-platform impact: N/A
- Duplicate risk: low/medium/high/unknown
- Manual verification required: Yes

---

*This is a template. Fill in after safe reproduction and evidence collection.*
*Do not submit until human final-review gate is completed.*