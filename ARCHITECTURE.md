# SentinelIT Architecture Deep Dive

## SIEM Event Flow

```
External threat / Internal event
        │
        ▼
┌───────────────────┐
│   Event Sources   │
│  ┌─────────────┐  │
│  │  Honeypot   │  │  ← Deception layer, zero false positives
│  │  HIDS       │  │  ← Host intrusion detection
│  │  FIM        │  │  ← File integrity monitoring  
│  │  WAF        │  │  ← OWASP CRS blocks → SIEM
│  │  Network    │  │  ← Connection analysis
│  │  API        │  │  ← Endpoint access logging
│  │  Agent      │  │  ← Client network events
│  └─────────────┘  │
└────────┬──────────┘
         │
         ▼
┌───────────────────┐
│   SIEM Core       │
│  events.db        │
│  correlations     │
│  threat_scores    │
└────────┬──────────┘
         │
         ▼
┌───────────────────┐     ┌──────────────┐
│  Auto-Block       │────▶│  nftables    │
│  score ≥ 80       │     │  firewall    │
└────────┬──────────┘     └──────────────┘
         │
         ▼
┌───────────────────┐     ┌──────────────┐
│  SIEM API :5001   │────▶│  Dashboard   │
│  Flask/REST       │     │  siem.html   │
└────────┬──────────┘     └──────────────┘
         │
         ▼
┌───────────────────┐     ┌──────────────┐
│  Claude Advisor   │────▶│  Gmail       │
│  Daily briefing   │     │  07:00 UTC   │
└───────────────────┘     └──────────────┘
```

## Multi-Tenant Architecture

```
┌─────────────────────────────────────────────┐
│              SentinelIT SOC                  │
│                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │ Client A │  │ Client B │  │ Client C │  │
│  │ tenant_1 │  │ tenant_2 │  │ tenant_3 │  │
│  │ agent_1  │  │ agent_2  │  │ agent_3  │  │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  │
│       │              │              │        │
│       ▼              ▼              ▼        │
│  ┌─────────────────────────────────────┐    │
│  │         tenants.db                   │    │
│  │  tenant_events (isolated per tenant) │    │
│  │  tenant_threats                      │    │
│  │  tenant_agents                       │    │
│  └─────────────────────────────────────┘    │
│       │                                      │
│       ▼                                      │
│  ┌─────────────────────────────────────┐    │
│  │    Tenant API :5002                  │    │
│  │    /api/tenant/stats                 │    │
│  │    /api/tenant/events                │    │
│  │    /api/tenant/agent/events  (POST)  │    │
│  └─────────────────────────────────────┘    │
│       │                                      │
│       ▼                                      │
│  sentinelitnextgen.co.za/portal              │
│  (per-client login, isolated view)           │
└─────────────────────────────────────────────┘
```

## NIST Risk-Based Prioritisation Model

Aligned with NVD overhaul announced April 15, 2026.
CVE submissions increased 263% between 2020–2025.

```
                    CVE Found
                        │
              ┌─────────┴──────────┐
              │                    │
         In CISA KEV?          EPSS Score?
              │                    │
             YES                  ≥70%
              │                    │
              ▼                    ▼
          P0 CRITICAL          P1 CRITICAL
          Fix today            Fix this week
          
              │
        CVSS ≥ 9.0?
        EPSS ≥ 30%?
              │
             YES
              │
              ▼
          P2 HIGH
          
              │
        CVSS ≥ 7.0?
        EPSS ≥ 10%?
              │
             YES
              │
              ▼
          P3 HIGH
          
              │
        CVSS ≥ 7.0?
              │
             YES
              │
              ▼
          P4 MEDIUM
          (theoretical risk only)
```

## Industrial Protocol Coverage

| Protocol | Vendor | SentinelIT | Dragos | Attack Vectors Monitored |
|---|---|---|---|---|
| Modbus TCP | Universal | ✅ | ✅ | Coil manipulation, register tampering |
| OPC UA | Universal | ✅ | ✅ | Subscription hijacking, node browsing |
| DNP3 | Energy/Water | ✅ | ✅ | Replay attacks, unsolicited response injection |
| EtherNet/IP | Rockwell | ✅ | ✅ | CIP service abuse, path traversal |
| PROFINET | Siemens | ✅ | ✅ | DCP flooding, RT frame manipulation |
| BACnet | Building | ✅ | ✅ | HVAC/access control injection |
| S7comm | Siemens | ✅ | ❌ | Stuxnet-class PLC attacks |
| FINS | Omron | ✅ | ❌ | Memory area reads, unauthorised uploads |
| MELSEC | Mitsubishi | ✅ | ❌ | Parameter tampering, forced output |
| IEC 61850 | Power | ✅ | ❌ | GOOSE injection, MMS attacks |
| CODESYS | Universal PLC | ✅ | ❌ | Runtime manipulation, code injection |
| HART-IP | Field instruments | ✅ | ❌ | Process variable manipulation |

**SentinelIT: 12 protocols. Dragos: 6 protocols.**
