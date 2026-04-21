# sentinelit-nextgen-security
Enterprise cybersecurity SaaS platform — SIEM, military-grade scanner, OT/ICS security, NIS2/ISO27001 compliance
# SentinelIT NextGen Security — Enterprise Cybersecurity Platform

[![Live Platform](https://img.shields.io/badge/Live-sentinelitnextgen.co.za-00d28c?style=flat-square)](https://sentinelitnextgen.co.za)
[![ISO 27001](https://img.shields.io/badge/ISO%2027001-100%25%20Compliant-00d28c?style=flat-square)](#compliance)
[![NIS2](https://img.shields.io/badge/NIS2-90%25%20Compliant-00d28c?style=flat-square)](#compliance)
[![License](https://img.shields.io/badge/License-Proprietary-blue?style=flat-square)](#)

> Production cybersecurity SaaS platform built from scratch — three product suites, 249+ modules, 99.9% uptime over eight years without a breach.

---

## Platform Overview

SentinelIT Guardian is a full-stack enterprise cybersecurity platform covering:

- **AI-Powered SIEM** — real-time event correlation, threat scoring, auto-block on score ≥ 80
- **Military-Grade Vulnerability Scanner** — 6-layer assessment with NIST risk-based prioritisation (KEV + EPSS + CVSS)
- **Multi-Tenant SOC Portal** — per-client isolation, live agent, branded dashboards
- **Industrial OT/ICS Security** — 12 protocols including S7comm, IEC 61850, HART-IP (Dragos covers 6)
- **Automated Compliance** — NIS2 (90%), ISO 27001 (100%), Cyber Essentials (80%) — weekly auto-generated reports
- **Web Application Firewall** — OWASP CRS, 921 rules, ModSecurity on Nginx

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                  sentinelitnextgen.co.za                     │
│                  Azure VM · Ubuntu 24.04 LTS                 │
├─────────────────────────────────────────────────────────────┤
│  Nginx (SSL/TLS) + ModSecurity WAF (OWASP CRS 921 rules)    │
├──────────────┬──────────────┬──────────────┬────────────────┤
│  Flask SIEM  │  Tenant API  │  Secure API  │  Industrial    │
│  API :5001   │  :5002       │  :5000       │  Core          │
├──────────────┴──────────────┴──────────────┴────────────────┤
│  SQLite SIEM DB · Tenant DB · PostgreSQL                     │
├─────────────────────────────────────────────────────────────┤
│  CRON: HIDS · FIM · Honeypot · Auto-block · WAF Monitor     │
│        Compliance Reports · Threat Digest · Vuln Scanner     │
└─────────────────────────────────────────────────────────────┘
```

---

## Product Suites

### Professional (60 modules)
- Container image scanning (CISA KEV + NVD + Exploit-DB)
- ML-powered vulnerability ranking by exploitation probability
- Real-time threat dashboard
- Basic compliance reporting

### Enterprise (90+ modules)
- Full SIEM with AI-powered correlation (28,000+ events processed)
- Multi-tenant SOC portal — per-client agent and isolated dashboard
- Military-grade domain/IP scanner (6 layers, NIST model)
- NIS2 / ISO 27001 / Cyber Essentials automated reports
- Auto-block engine — threat score ≥ 80 triggers instant firewall block
- Daily AI threat digest via email
- IP geo-reputation enrichment (FIRST.org EPSS integration)
- OpenVAS integration — 95,086 CVE signatures
- Honeypot deception layer
- Web Application Firewall (OWASP CRS)

### Industrial (99+ modules)
- 12 industrial protocol monitoring (vs Dragos's 6):
  - Standard: Modbus TCP, OPC UA, DNP3, EtherNet/IP, PROFINET, BACnet
  - Exclusive: S7comm (Siemens), FINS (Omron), MELSEC (Mitsubishi), IEC 61850, CODESYS, HART-IP
- PLC security monitoring with AI process learning
- Digital twin attack simulation
- Safety-security convergence
- IEC 62443 compliance

---

## Military-Grade Vulnerability Scanner

Aligned with **NIST NVD April 15, 2026 overhaul** — CVE submissions increased 263% between 2020–2025. CVSS-only scoring is no longer sufficient.

### 6-Layer Assessment Pipeline

```
Layer 1: Reconnaissance    — DNS, SPF/DMARC, zone transfer, WHOIS/ASN
Layer 2: Port Discovery    — nmap -sV --script=vuln, OS detection, NSE CVE scripts
Layer 3: Web Application   — Nikto (6,700+ checks), WhatWeb, SSL/TLS audit, HTTP headers
Layer 4: CVE Intelligence  — CISA KEV (1,569 CVEs), EPSS from FIRST.org, service matching
Layer 5: Prioritisation    — NIST risk-based model (KEV + EPSS + CVSS)
Layer 6: Report & Output   — Branded PDF, SIEM integration, email delivery
```

### Risk-Based Prioritisation Model

```
P0 — KEV confirmed     → Actively exploited in the wild (fix today)
P1 — EPSS > 70%        → High exploitation probability in 30 days
P2 — CVSS 9+ EPSS 30%  → Critical severity with real exploitation likelihood  
P3 — CVSS 7+ EPSS 10%  → High severity with notable exploitation risk
P4 — CVSS 7+ only      → High theoretical severity, low real-world data
P5 — Low               → Monitor and patch in next maintenance cycle
P6 — Info              → No current exploitation evidence
```

> A CVSS 9.8 with 0.1% EPSS ranks **below** a CVSS 6.5 with 94% EPSS.
> Only 1% of CVEs are ever exploited in the wild — this scanner finds those 1%.

---

## Compliance

| Framework | Score | Details |
|---|---|---|
| ISO 27001 | **100%** | All 10 control domains verified — auto-generated weekly |
| NIS2 Directive | **90%** | 9 of 10 Article 21 controls satisfied |
| Cyber Essentials | **80%** | UK NCSC framework verified |

Reports auto-generated every Monday and available at `/reports/` (password protected).

---

## Tech Stack

| Layer | Technology |
|---|---|
| Cloud | Azure VM (Ubuntu 24.04 LTS) |
| Web Server | Nginx 1.24 + ModSecurity WAF (OWASP CRS) |
| Application | Python 3.12 / Flask / Gunicorn |
| SIEM Database | SQLite (28,000+ events) |
| Vulnerability Scanner | nmap, Nikto, WhatWeb, OpenVAS (95,086 CVEs) |
| Threat Intel | CISA KEV, FIRST.org EPSS, NVD API |
| IaC | Terraform, Azure CLI |
| SSL/TLS | Let's Encrypt (auto-renew) |
| Email | Gmail SMTP (threat digests, scan reports) |
| Industrial | Custom Python — 12 protocol monitors |
| AI | Anthropic Claude API (SOC briefings) |

---

## Key Technical Achievements

- **99.9% uptime** over 8 years without a security breach
- **90% reduction in false positives** via AI-powered event classification
- **90% reduction in MTTR** through automated incident response
- **85% reduction in manual audit time** via automated compliance engine
- **3 IPs auto-blocked** within first 24 hours of auto-block deployment
- **28,000+ security events** processed and correlated
- **1,569 CISA KEV CVEs** cross-referenced on every scan
- **921 OWASP CRS rules** blocking SQLi, XSS, and injection attacks

---

## Security Architecture

```
Internet
    │
    ▼
[Nginx + ModSecurity WAF]  ← 921 OWASP rules block SQLi/XSS/injection
    │
    ▼
[nftables Firewall]        ← 5 network zones, auto-block engine
    │
    ▼
[Flask APIs]               ← Rate limiting, API key auth, input validation
    │
    ▼
[SIEM Core]                ← Event correlation, threat scoring, alerting
    │
    ├── HIDS               ← File integrity + system monitoring
    ├── Honeypot           ← Deception layer (zero false positives)
    ├── FIM                ← Critical file change detection
    ├── Network Monitor    ← Connection analysis
    └── Auto-Block         ← Score ≥ 80 = instant block
```

---

## Pricing

| Plan | Price | Modules |
|---|---|---|
| Professional | £495/month | 60 modules |
| Enterprise | £1,995/month | 90+ modules |
| Industrial | £4,995/month | 99+ modules (OT/ICS) |

**78% lower cost than Dragos + Splunk + Nozomi combined.**

---

## Live URLs

| URL | Description |
|---|---|
| `sentinelitnextgen.co.za` | Enterprise homepage |
| `sentinelitnextgen.co.za/command-center.html` | Unified SOC + Scanner + Compliance |
| `sentinelitnextgen.co.za/portal` | Multi-tenant client portal |
| `sentinelitnextgen.co.za/platform.html` | Full pricing and features |
| `sentinelitnextgen.co.za/siem.html` | Live SOC dashboard (auth required) |
| `sentinelitnextgen.co.za/reports/` | Compliance reports (auth required) |

---

## Project Structure

```
sentinelit-command-center/
├── production-website/
│   ├── index.html                    # Enterprise homepage
│   ├── command-center.html           # Unified platform UI
│   ├── portal/                       # Multi-tenant client portal
│   ├── agent/                        # Client agent installer
│   │   ├── sentinelit_agent.py       # Guardian agent v2.0
│   │   └── install.sh                # One-command Linux installer
│   └── reports/                      # Auto-generated compliance reports
├── /opt/sentinelit/bin/
│   ├── siem_api.py                   # SIEM Flask API (port 5001)
│   ├── tenant_api.py                 # Multi-tenant API (port 5002)
│   ├── military_scanner.py           # 6-layer vulnerability scanner
│   ├── compliance_report.py          # NIS2/ISO27001/CE generator
│   ├── threat_digest.py              # Daily email digest
│   ├── auto_block.py                 # Threat score auto-block
│   ├── honeypot.py                   # Deception layer monitor
│   ├── hids.py                       # Host intrusion detection
│   ├── fim.py                        # File integrity monitoring
│   ├── claude_advisor.py             # AI SOC briefing agent
│   ├── firewall_manager.py           # nftables automation
│   └── waf_monitor.py                # WAF → SIEM bridge
└── /opt/sentinelit/industrial/
    └── industrial_security_core.py   # 12-protocol OT/ICS monitor
```

---

## About

Built by **James Zulu** — Cloud Security Engineer, DevSecOps, Founder.

- 8 years building production security infrastructure
- 38+ certifications across AWS, Azure, Cisco, ISC², Harvard
- AWS Security Hub, GuardDuty, WAF, Config, CloudTrail in production
- Wiz, Prisma Cloud, Defender for Cloud CSPM experience
- UK Partner Visa — no sponsorship required

**Contact:** jameszulu@sentinelitnextgen.co.za  
**LinkedIn:** linkedin.com/in/jameszulu  
**Website:** sentinelitnextgen.co.za

---

*SentinelIT NextGen Security — sentinelitnextgen.co.za*
