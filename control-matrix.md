# Control Matrix — NIST 800-53 Rev 5 Compliance Gap Analysis
## AcmeCorp Fintech | April 2026

---

**Purpose:** This matrix provides a consolidated, scannable view of all controls assessed during the AcmeCorp compliance gap analysis. Each row maps a NIST 800-53 Rev 5 control to its implementation status, identified gap, risk rating, ISO 27001:2022 cross-reference, SOC 2 TSC alignment, and remediation priority.

**Status Definitions:**

| Status | Meaning |
|--------|---------|
| ✅ Implemented | Fully documented, operationally active, evidence available |
| ⚠️ Partial | Exists in some form but incomplete, inconsistent, or undocumented |
| ❌ Not Implemented | No evidence of implementation — complete gap |

**Risk Rating:**

| Rating | Meaning |
|--------|---------|
| 🔴 High | Likely to be exploited; significant impact to confidentiality, integrity, or availability |
| 🟡 Medium | Possible exploitation under specific conditions; moderate impact |
| 🟢 Low | Unlikely exploitation; limited impact with compensating controls |

**Priority:**

| Priority | Meaning |
|----------|---------|
| P1 — Immediate | Address within 0–30 days |
| P2 — Short-Term | Address within 30–90 days |
| P3 — Long-Term | Address within 90–180 days |

---

## Access Control (AC)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| AC-1 | Access Control Policy and Procedures | ❌ Not Implemented | No formal access control policy exists. Access provisioning is entirely ad hoc with no documented roles, responsibilities, or review cadence. | 🔴 High | A.5.15 | CC6.1 | P1 — Immediate |
| AC-2 | Account Management | ⚠️ Partial | No formal account lifecycle process. Offboarding is inconsistent — confirmed cases of terminated employees retaining access 30+ days post-departure. No periodic access reviews conducted. | 🔴 High | A.5.16 | CC6.2 | P1 — Immediate |
| AC-6 | Least Privilege | ❌ Not Implemented | Multiple developer accounts carry AWS AdministratorAccess policies. No RBAC model exists. No separation between standard and privileged account tiers. | 🔴 High | A.5.15 | CC6.3 | P1 — Immediate |
| AC-17 | Remote Access | ⚠️ Partial | No remote access policy. AWS console accessible from any IP. No VPN or ZTNA solution. No device posture checks integrated into access decisions. | 🟡 Medium | A.8.20 | CC6.6 | P2 — Short-Term |

---

## Audit and Accountability (AU)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| AU-2 | Event Logging | ⚠️ Partial | CloudTrail not enabled across all AWS accounts. No centralized SIEM or log aggregation platform. Application-level logging is inconsistent. No enterprise event logging standard defined. | 🔴 High | A.8.15 | CC7.2 | P1 — Immediate |
| AU-9 | Protection of Audit Information | ❌ Not Implemented | CloudTrail logs stored in S3 within the same production account. No Object Lock (WORM) enabled. Accounts with AdministratorAccess can delete or modify log files. | 🟡 Medium | A.8.15 | CC7.2 | P2 — Short-Term |
| AU-12 | Audit Record Generation | ⚠️ Partial | Database query logging disabled on primary RDS instances. No standardized log schema. Application services produce inconsistent and incomplete audit records. | 🟡 Medium | A.8.15 | CC7.2 | P2 — Short-Term |

---

## Configuration Management (CM)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| CM-2 | Baseline Configuration | ❌ Not Implemented | No documented baseline configurations for any infrastructure component. Terraform modules exist but are not maintained as authoritative. No configuration drift detection in place. | 🔴 High | A.8.9 | CC7.1 | P2 — Short-Term |
| CM-6 | Configuration Settings | ❌ Not Implemented | No hardening standards (CIS Benchmarks or equivalent) applied to endpoints, servers, or cloud services. Default configurations in use. No deviation management process. | 🟡 Medium | A.8.9 | CC7.1 | P2 — Short-Term |
| CM-8 | System Component Inventory | ⚠️ Partial | Partial endpoint inventory in a manually maintained spreadsheet (~400 of 500 devices). Cloud assets not formally inventoried. AWS resource tags applied inconsistently. No automated discovery. | 🟡 Medium | A.8.8 | CC6.1 | P2 — Short-Term |

---

## Identification and Authentication (IA)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| IA-2 | Identification and Authentication — Organizational Users | ⚠️ Partial | MFA available but not enforced. ~38% of active Google Workspace accounts have no MFA enrolled. No AWS SCP enforcing MFA for IAM operations. Root accounts inadequately protected. | 🔴 High | A.8.5 | CC6.1 | P1 — Immediate |
| IA-5 | Authenticator Management | ❌ Not Implemented | No enterprise password policy defined or enforced. Password history enforcement disabled. Credentials (API keys, DB passwords) found in plaintext in source code repositories. No secrets management solution deployed. | 🔴 High | A.5.17 | CC6.1 | P1 — Immediate |
| IA-8 | Identification and Authentication — Non-Organizational Users | ⚠️ Partial | Customer-facing platform uses SMS OTP which does not meet NIST SP 800-63B AAL2. API keys lack expiration enforcement and are rotated infrequently. | 🟡 Medium | A.8.5 | CC6.1 | P2 — Short-Term |

---

## Incident Response (IR)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| IR-1 | Incident Response Policy and Procedures | ❌ Not Implemented | No incident response policy exists. No written procedures, defined roles, escalation paths, or communication plans. Confirmed by senior engineering staff that response to a live incident would be fully improvised. | 🔴 High | A.5.26 | CC7.3 | P1 — Immediate |
| IR-2 | Incident Response Training | ❌ Not Implemented | No security awareness or IR training program. Onboarding does not include security content. No role-based IR training for technical or non-technical staff. | 🟡 Medium | A.6.3 | CC9.2 | P2 — Short-Term |
| IR-4 | Incident Handling | ❌ Not Implemented | No incident handling capability across any phase of the IR lifecycle (preparation, detection, containment, eradication, recovery). No EDR, no SIEM alerts, no playbooks, no tabletop exercises conducted. | 🔴 High | A.5.26 | CC7.4 | P1 — Immediate |
| IR-6 | Incident Reporting | ❌ Not Implemented | No internal incident reporting channel or process. No regulatory notification workflow mapping incident types to applicable reporting timelines (NYDFS 72-hour, SEC disclosure requirements). | 🟡 Medium | A.5.26 | CC7.3 | P2 — Short-Term |

---

## Risk Assessment (RA)

| Control ID | Control Name | Status | Gap Description | Risk | ISO 27001:2022 | SOC 2 TSC | Priority |
|------------|-------------|--------|-----------------|------|----------------|-----------|----------|
| RA-2 | Security Categorization | ❌ Not Implemented | No data classification scheme or system security plan exists. Customer financial data, employee PII, and internal operational data are treated without distinction from a security controls perspective. | 🔴 High | A.5.12 | CC3.1 | P1 — Immediate |
| RA-3 | Risk Assessment | ❌ Not Implemented | No enterprise risk assessment has been conducted. No risk register, no risk methodology, no risk acceptance framework. Security decisions are reactive and entirely undocumented. | 🔴 High | A.6.1 | CC3.2 | P1 — Immediate |
| RA-5 | Vulnerability Monitoring and Scanning | ❌ Not Implemented | No vulnerability scanning program. AWS Inspector not enabled. No DAST/SAST in CI/CD pipeline. No penetration testing history. No dependency scanning (Dependabot/Snyk). No vulnerability remediation SLAs defined. | 🔴 High | A.8.8 | CC7.1 | P1 — Immediate |

---

## Consolidated Risk Summary

| Risk Rating | Count | Control IDs |
|-------------|-------|-------------|
| 🔴 High | 10 | AC-1, AC-2, AC-6, AU-2, CM-2, IA-2, IA-5, IR-1, IR-4, RA-2, RA-3, RA-5 |
| 🟡 Medium | 8 | AC-17, AU-9, AU-12, CM-6, CM-8, IA-8, IR-2, IR-6 |
| 🟢 Low | 0 | — |
| **Total** | **18** | |

---

## Priority Action Summary

| Priority | Controls | Count |
|----------|----------|-------|
| P1 — Immediate (0–30 days) | AC-1, AC-2, AC-6, AU-2, IA-2, IA-5, IR-1, IR-4, RA-2, RA-3, RA-5 | 11 |
| P2 — Short-Term (30–90 days) | AC-17, AU-9, AU-12, CM-2, CM-6, CM-8, IA-8, IR-2, IR-6 | 9 |
| P3 — Long-Term (90–180 days) | — | 0 |

> **Note:** The absence of P3 items reflects the severity of AcmeCorp's current posture. All identified gaps carry sufficient risk to warrant action within 90 days. Long-term items will emerge naturally as foundational controls are implemented and the security program matures.

---

## Implementation Status Overview

```
Control Family        Implemented    Partial    Not Implemented
─────────────────────────────────────────────────────────────
Access Control (AC)        0            2              2
Audit & Account. (AU)      0            2              1
Config. Mgmt. (CM)         0            1              2
Identification (IA)        0            2              1
Incident Response (IR)     0            0              4
Risk Assessment (RA)       0            0              3
─────────────────────────────────────────────────────────────
TOTAL                      0            7             13
```

---

*This matrix is a companion document to `gap-analysis-report.md` (detailed findings narrative) and `remediation-roadmap.md` (phased remediation plan). All three documents should be read together for full context.*

*Assessment Date: April 2026 | Framework: NIST SP 800-53 Rev 5 | Prepared by: Cybersecurity Advisory Practice*
