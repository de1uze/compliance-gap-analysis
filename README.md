# 🔐 NIST 800-53 & ISO 27001 Compliance Gap Analysis
### AcmeCorp Fintech — GRC Portfolio Project

---

## What Is This?

This repository contains a **professional compliance gap analysis** built as a GRC portfolio project. It simulates the kind of work a Security Analyst or GRC Consultant would deliver to a real client — written at enterprise consulting quality, not academic style.

The fictional subject is **AcmeCorp**, a 500-person fintech startup in New York preparing for their first SOC 2 Type II audit with no formal security program in place.

---

## The Scenario

| | |
|---|---|
| **Company** | AcmeCorp, Inc. (fictional) |
| **Industry** | Financial Technology (Fintech) |
| **Size** | ~500 employees |
| **Location** | New York, NY |
| **Situation** | Rapid growth, no formal security program, preparing for SOC 2 audit |

---

## Frameworks Used

| Framework | Version | Role |
|-----------|---------|------|
| NIST SP 800-53 | Revision 5 (2020) | Primary control catalog |
| ISO/IEC 27001 | 2022 Edition | Cross-reference mapping |
| SOC 2 (AICPA TSC) | 2017 Revised | Audit readiness alignment |
| NYDFS 23 NYCRR 500 | 2023 Amendment | Regulatory context |
| NIST Cybersecurity Framework | 2.0 (2024) | Supplementary risk framing |

---

## What's In This Repo

| File | Description |
|------|-------------|
| `gap-analysis-report.md` | Full consultant-style report — scope, methodology, 20 control findings with risk ratings, strategic recommendations |
| `control-matrix.md` | Compact table mapping every control ID to its status, gap, risk rating, and remediation priority |
| `remediation-roadmap.md` | Phased action plan organized as quick wins (0–30 days), medium-term (30–90 days), and long-term (90+ days) |

---

## Key Findings Summary

- **20 controls** reviewed across 6 NIST 800-53 control families
- **10 High-risk gaps** — including no incident response plan, no MFA enforcement, and hardcoded credentials in source repositories
- **7 Medium-risk gaps** — including incomplete audit logging, no asset inventory, and weak customer authentication
- Control families assessed: **AC, AU, CM, IA, IR, RA**

---

## Control Families Covered

| Family | Name | Controls Reviewed |
|--------|------|------------------|
| AC | Access Control | AC-1, AC-2, AC-6, AC-17 |
| AU | Audit & Accountability | AU-2, AU-9, AU-12 |
| CM | Configuration Management | CM-2, CM-6, CM-8 |
| IA | Identification & Authentication | IA-2, IA-5, IA-8 |
| IR | Incident Response | IR-1, IR-2, IR-4, IR-6 |
| RA | Risk Assessment | RA-2, RA-3, RA-5 |

---

## About

**Pratik Shringarpure**
MS Cybersecurity — Yeshiva University, New York *(GPA 3.98, graduating Jan 2027)*
Built for: SOC Analyst / Security Analyst portfolio
GitHub: [github.com/de1uze](https://github.com/de1uze)
