# Military-Grade Vulnerability Scanner

## Overview

6-layer security assessment pipeline aligned with the NIST NVD April 15, 2026 overhaul.
Built using nmap, Nikto, WhatWeb, OpenVAS, CISA KEV, and FIRST.org EPSS.

## Usage

```bash
# Scan a domain
python3 military_scanner.py target.com your@email.com "Your Name" "Your Org"

# Via API
curl -X POST https://sentinelitnextgen.co.za/api/siem/domain-scan \
  -H "Content-Type: application/json" \
  -d '{
    "target": "target.com",
    "name": "James Smith",
    "email": "james@company.com",
    "org": "Acme Corp"
  }'
```

## Sample Output

```json
{
  "status": "complete",
  "target": "target.com",
  "risk": "HIGH",
  "sit_score": 640,
  "total_findings": 47,
  "report_url": "https://sentinelitnextgen.co.za/reports/SIT-SCAN-202604191808.html"
}
```

## Layer Detail

### Layer 1 — Reconnaissance
- DNS A, MX, TXT, NS, AAAA records
- SPF and DMARC validation
- Zone transfer attempt
- WHOIS and ASN lookup

### Layer 2 — Port & Service Discovery
- nmap service version detection (-sV)
- NSE vulnerability scripts (--script=vuln)
- 35 high-risk port coverage
- OS fingerprinting

### Layer 3 — Web Application
- Nikto 2.1.5 — 6,700+ vulnerability checks
- WhatWeb technology fingerprinting
- HTTP security header analysis (HSTS, CSP, X-Frame-Options, etc.)
- SSL/TLS audit (cipher strength, version, expiry)

### Layer 4 — CVE Intelligence
- CISA KEV cross-reference (1,569 actively exploited CVEs)
- EPSS score lookup via FIRST.org API
- Service version → CVE pattern matching
- nmap NSE CVE extraction

### Layer 5 — NIST Risk-Based Prioritisation
```
P0 KEV match      → risk_score: 100 (fix today)
P1 EPSS ≥ 70%     → risk_score: 90  (fix this week)
P2 CVSS9+EPSS30%  → risk_score: 80
P3 CVSS7+EPSS10%  → risk_score: 65
P4 CVSS7+ only    → risk_score: 40
P5 Low signal     → risk_score: 20
P6 Info           → risk_score: 5
```

### Layer 6 — Reporting
- HTML report saved to `/reports/`
- Email delivered with full findings
- HIGH/CRITICAL findings logged to SIEM
- Available at password-protected URL

## Why CVSS Alone Is No Longer Enough

> "CVE submissions increased 263% between 2020 and 2025. We don't expect this trend
> to let up anytime soon." — NIST, April 15, 2026

Only **1% of CVEs** are ever exploited in the wild.
A CVSS 9.8 with 0.1% EPSS is less dangerous than a CVSS 6.5 with 94% EPSS.

SentinelIT uses three signals:
- **KEV** — is it being exploited right now?
- **EPSS** — what is the probability of exploitation in the next 30 days?
- **CVSS** — what is the theoretical maximum impact?

This is the model NIST adopted on April 15, 2026. SentinelIT was built for it.
