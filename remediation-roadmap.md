# Remediation Roadmap — NIST 800-53 Rev 5 Compliance Gap Analysis
## AcmeCorp Fintech | April 2026

---

**Document Purpose:** This roadmap translates the findings from the AcmeCorp compliance gap analysis into a prioritized, phased action plan. Recommendations are sequenced to maximize risk reduction in the shortest timeframe while building toward a sustainable, audit-ready security program.

**Companion Documents:**
- `gap-analysis-report.md` — Detailed findings narrative
- `control-matrix.md` — Control-by-control status and risk ratings

---

## Guiding Principles

Before reviewing the roadmap, the following principles govern how recommendations are sequenced:

**1. Foundation before tooling.** Policy and governance gaps are addressed before technology investments. A SIEM deployed without a logging standard, or an EDR without an incident response plan, provides false confidence rather than real protection.

**2. Highest risk first.** Actions that eliminate the most exploitable attack vectors — exposed credentials, missing MFA, absent IR capability — are prioritized regardless of effort level.

**3. Quick wins build momentum.** Several critical gaps can be closed in hours or days with near-zero cost. These are front-loaded to demonstrate immediate security improvement to leadership and auditors.

**4. Align to SOC 2 readiness.** Every recommendation maps to a SOC 2 Trust Services Criteria (TSC) requirement. AcmeCorp's SOC 2 preparation is the organizing commercial objective of this program.

---

## Roadmap Overview

| Phase | Timeframe | Focus | Controls Addressed |
|-------|-----------|-------|--------------------|
| Phase 1 — Quick Wins | 0–30 days | Eliminate immediate critical risk | AC-1, AC-2, AU-2, IA-2, IA-5, IR-1, RA-2, RA-3 |
| Phase 2 — Foundation Build | 30–90 days | Establish governance and detection infrastructure | AC-6, AC-17, AU-9, AU-12, CM-2, CM-8, IA-8, IR-2, IR-4, IR-6, RA-5 |
| Phase 3 — Maturity & Sustainment | 90–180 days | Harden, automate, and operationalize | CM-6, all families — continuous improvement |

---

## Phase 1 — Quick Wins (0–30 Days)

> **Objective:** Eliminate the highest-severity, most exploitable gaps with urgent, high-leverage actions. Several items in this phase are zero or near-zero cost and can be implemented within hours.

---

### 1.1 Enforce Multi-Factor Authentication Across the Organization
**Controls Addressed:** IA-2
**Risk Eliminated:** 🔴 High
**Effort:** Low — configuration change, no new tooling required
**Owner:** IT / Engineering Lead

**Actions:**
1. Navigate to Google Workspace Admin Console → Security → Authentication → 2-Step Verification
2. Set enforcement to **mandatory for all users** with no grace period exceptions for new hires beyond 24 hours
3. Set allowed second factors to **authenticator app or hardware key only** — remove SMS as an option for privileged users
4. Enable **AWS SCP** at the Organizations root requiring MFA for all IAM console and API actions
5. Audit current MFA enrollment status; export list of non-compliant accounts and notify directly
6. Set a **72-hour deadline** for full enrollment; suspend non-compliant accounts after deadline

**Success Metric:** 100% MFA enrollment across all active Google Workspace and AWS IAM accounts within 30 days.

**SOC 2 TSC:** CC6.1 | **NYDFS:** 500.12

---

### 1.2 Emergency Secrets Rotation and Repository Remediation
**Controls Addressed:** IA-5
**Risk Eliminated:** 🔴 High
**Effort:** Medium — requires engineering coordination
**Owner:** Engineering Lead / CTO

**Actions:**
1. Run **Trufflehog or GitHub's secret scanning** across all repositories immediately to identify all exposed credentials
2. **Rotate every identified secret immediately** — assume all exposed credentials are compromised
3. Revoke and reissue: AWS access keys, database passwords, third-party API keys, Stripe/payment API credentials
4. Deploy **AWS Secrets Manager** or **HashiCorp Vault** as the authoritative secrets store
5. Update all applications to retrieve secrets from the secrets manager at runtime — eliminate all plaintext environment variable files from repos
6. Enable **GitHub Advanced Security — Secret Scanning** with push protection to block future credential commits
7. Add `.env` and credential file patterns to `.gitignore` globally across all repositories
8. Document a Secrets Management Policy and distribute to all engineers

**Success Metric:** Zero plaintext credentials in any repository. All secrets managed through a centralized secrets management solution.

**SOC 2 TSC:** CC6.1 | **NYDFS:** 500.12

---

### 1.3 Account Lifecycle Audit and Offboarding Process
**Controls Addressed:** AC-2
**Risk Eliminated:** 🔴 High
**Effort:** Medium — requires HR and IT coordination
**Owner:** IT / People Operations

**Actions:**
1. Pull current active user list from Google Workspace and AWS IAM
2. Cross-reference against HRIS active employee roster — identify all orphaned accounts
3. **Immediately suspend all accounts belonging to former employees**
4. Review and remove unnecessary third-party OAuth application authorizations from all accounts
5. Document a formal **Joiner-Mover-Leaver (JML) process** defining:
   - Day 1 provisioning checklist triggered by HRIS new hire record
   - Role change access modification triggered by manager request
   - Day 0 offboarding: automatic account suspension upon HR termination record
6. Implement **automated offboarding trigger** — connect HRIS (e.g., Rippling, Workday) to Google Workspace via SCIM provisioning to auto-suspend accounts on termination
7. Schedule **quarterly access review** cadence as recurring calendar event with IT owner

**Success Metric:** Zero orphaned accounts belonging to former employees. JML process documented and SCIM automation active within 30 days.

**SOC 2 TSC:** CC6.2 | **NYDFS:** 500.07

---

### 1.4 Draft and Approve Access Control Policy
**Controls Addressed:** AC-1
**Risk Eliminated:** 🔴 High
**Effort:** Low — documentation effort only
**Owner:** Security Lead / CTO (interim)

**Actions:**
1. Draft Access Control Policy document covering:
   - Purpose, scope, and applicability
   - Least privilege and need-to-know principles
   - Role-Based Access Control (RBAC) model definition
   - Account types (standard user, privileged user, service account, guest)
   - Access request and approval workflow
   - Periodic access review requirements (quarterly minimum)
   - Separation of duties requirements
   - Consequences for policy violations
2. Route for review and formal approval by CTO or designated security owner
3. Publish to company intranet or policy management platform (e.g., Confluence)
4. Distribute to all employees and require acknowledgment

**Success Metric:** Access Control Policy formally approved, published, and acknowledged by 100% of staff within 30 days.

**SOC 2 TSC:** CC6.1 | **NYDFS:** 500.07

---

### 1.5 Enable Enterprise-Wide Audit Logging
**Controls Addressed:** AU-2
**Risk Eliminated:** 🔴 High
**Effort:** Low-Medium — configuration and tooling
**Owner:** Engineering / DevOps

**Actions:**
1. Enable **AWS CloudTrail** in all AWS accounts across all regions — ensure management events, data events (S3, Lambda), and Insights events are captured
2. Enable **CloudTrail log file validation** to detect tampering
3. Configure CloudTrail to deliver logs to a **centralized, dedicated S3 bucket** in an isolated logging AWS account
4. Enable **AWS Config** in all accounts to record configuration change history
5. Enable **VPC Flow Logs** for all production VPCs
6. Define an enterprise **Event Logging Standard** document specifying:
   - Authentication events (login, logout, failed attempts, MFA events)
   - Privileged actions (IAM changes, security group modifications, key creation/deletion)
   - Data access events (S3 access to sensitive buckets, RDS query activity)
   - Application events (user actions, API calls, errors)
7. Enable **RDS query logging** and ship to CloudWatch Logs

**Success Metric:** CloudTrail active in 100% of AWS accounts and regions. Event logging standard documented and distributed.

**SOC 2 TSC:** CC7.2 | **NYDFS:** 500.06

---

### 1.6 Initiate Security Categorization and Data Classification
**Controls Addressed:** RA-2
**Risk Eliminated:** 🔴 High
**Effort:** Medium — requires cross-functional workshop
**Owner:** CTO / Engineering Lead / Legal

**Actions:**
1. Conduct a **data discovery workshop** with Engineering, Legal, and People Operations to inventory all major data types AcmeCorp processes, stores, or transmits
2. Define a **Data Classification Policy** with minimum three tiers:
   - **Public** — Marketing content, press releases
   - **Internal** — Operational data, internal communications
   - **Confidential / Restricted** — Customer financial data, PII, credentials, audit logs
3. Apply classification labels to all major data stores (S3 buckets, RDS databases, Google Drive)
4. Define control requirements by classification tier (encryption at rest, access restrictions, retention periods)
5. Begin drafting a **System Security Plan (SSP)** documenting AcmeCorp's system boundary, data flows, and control implementation

**Success Metric:** Data Classification Policy approved. All major data stores labeled. SSP draft initiated.

**SOC 2 TSC:** CC3.1 | **NYDFS:** 500.09

---

### 1.7 Conduct Initial Enterprise Risk Assessment
**Controls Addressed:** RA-3
**Risk Eliminated:** 🔴 High
**Effort:** Medium — facilitated workshop + documentation
**Owner:** CTO / Security Lead

**Actions:**
1. Adopt **NIST SP 800-30 Rev 1** as AcmeCorp's risk assessment methodology
2. Conduct a structured risk identification workshop with Engineering, Legal, Finance, and People Operations
3. Build an **Enterprise Risk Register** (spreadsheet or GRC tool) capturing:
   - Risk ID and description
   - Threat source and threat event
   - Likelihood rating (High / Medium / Low)
   - Impact rating (High / Medium / Low)
   - Inherent risk score
   - Current controls (if any)
   - Residual risk score
   - Risk owner
   - Treatment decision (Accept / Mitigate / Transfer / Avoid)
4. Establish **quarterly risk register review** as a standing agenda item for leadership
5. Define a **Risk Acceptance Process** — document who has authority to formally accept risk at each level

**Success Metric:** Enterprise risk register published and reviewed by leadership. Risk assessment methodology formally adopted.

**SOC 2 TSC:** CC3.2 | **NYDFS:** 500.09

---

### 1.8 Publish Incident Response Policy
**Controls Addressed:** IR-1
**Risk Eliminated:** 🔴 High
**Effort:** Low-Medium — documentation effort
**Owner:** CTO / Legal / Security Lead

**Actions:**
1. Draft **Incident Response Policy** covering:
   - Purpose, scope, and definitions (what constitutes a security incident)
   - Incident severity classification (Critical / High / Medium / Low)
   - Roles and responsibilities:
     - **Incident Commander** — coordinates overall response
     - **Technical Lead** — leads containment and eradication
     - **Legal / Compliance Liaison** — manages regulatory obligations
     - **Communications Lead** — manages internal and external communications
   - Five-phase IR lifecycle: Preparation → Detection & Analysis → Containment → Eradication & Recovery → Post-Incident Review
   - Regulatory notification obligations (NYDFS 72-hour, SEC disclosure)
   - Policy review cadence (annual minimum)
2. Establish a **security incident reporting channel**: security@acmecorp.com and a dedicated #security-incidents Slack channel
3. Define an **on-call security rotation** for incident response coverage
4. Obtain formal executive approval and publish policy

**Success Metric:** IR Policy formally approved and published. Incident reporting channel established. Roles assigned.

**SOC 2 TSC:** CC7.3 | **NYDFS:** 500.16

---

## Phase 2 — Foundation Build (30–90 Days)

> **Objective:** Build the governance infrastructure, detection capability, and technical controls that transform AcmeCorp from reactive to proactive. This phase requires meaningful tool investment and cross-functional project coordination.

---

### 2.1 Implement Privileged Access Management and RBAC
**Controls Addressed:** AC-6
**Risk Eliminated:** 🔴 High
**Effort:** High — architecture and engineering effort
**Owner:** Engineering Lead / DevOps

**Actions:**
1. Design a formal **RBAC model** for AWS IAM with minimum four tiers:
   - Read-Only (developers, audit access)
   - Developer (scoped write access per service/environment)
   - Operations (infrastructure management, no data access)
   - Administrator (break-glass only, time-limited, MFA-required)
2. Remove all directly attached `AdministratorAccess` policies from developer IAM users
3. Deploy **AWS IAM Identity Center** (SSO) as the centralized access broker for all AWS accounts
4. Implement **time-limited privileged access** — administrative sessions auto-expire after 4 hours
5. Evaluate and deploy a **PAM solution** (CyberArk, BeyondTrust, or AWS Systems Manager Session Manager for server access)
6. Apply RBAC model to Google Workspace, GitHub Enterprise, and core SaaS platforms
7. Document all role definitions and associated permissions

**Success Metric:** Zero developer accounts with AdministratorAccess. RBAC model fully deployed across AWS and core platforms.

**SOC 2 TSC:** CC6.3 | **NYDFS:** 500.07

---

### 2.2 Deploy SIEM and Centralized Log Management
**Controls Addressed:** AU-2, AU-9, AU-12
**Risk Eliminated:** 🔴 High / 🟡 Medium
**Effort:** High — significant tooling and configuration effort
**Owner:** Engineering / DevOps / Security

**Actions:**
1. Evaluate and select a SIEM solution appropriate to AcmeCorp's scale:
   - **Budget-conscious:** Elastic SIEM (open source) + AWS OpenSearch
   - **Mid-market:** Sumo Logic, Datadog Security
   - **Enterprise:** Splunk Cloud
2. Ingest all log sources into SIEM: CloudTrail, VPC Flow Logs, RDS query logs, application logs, Google Workspace audit logs, endpoint logs
3. Implement **S3 Object Lock (WORM)** on the centralized CloudTrail log bucket — set retention period minimum 13 months
4. Move the logging S3 bucket to a **dedicated, isolated AWS account** within AWS Organizations with restricted cross-account access
5. Define and implement **baseline detection rules** covering:
   - Brute force / credential stuffing attempts
   - Impossible travel / anomalous login geography
   - Privilege escalation events
   - Mass data download / exfiltration indicators
   - CloudTrail disabled or log deletion attempted
6. Configure **alerting and escalation** — critical alerts route to on-call via PagerDuty or equivalent

**Success Metric:** SIEM operational with all log sources ingested. Baseline detection rules active. Log immutability enforced.

**SOC 2 TSC:** CC7.2 | **NYDFS:** 500.06

---

### 2.3 Establish Vulnerability Management Program
**Controls Addressed:** RA-5
**Risk Eliminated:** 🔴 High
**Effort:** Medium — tooling configuration + process establishment
**Owner:** Engineering / DevOps

**Actions:**
1. Enable **AWS Inspector v2** across all AWS accounts for EC2, ECR container image, and Lambda function vulnerability scanning
2. Enable **GitHub Dependabot** or integrate **Snyk** into CI/CD pipeline for dependency vulnerability scanning on all repositories
3. Integrate **SAST tooling** (Semgrep or CodeQL) into GitHub Actions CI pipeline — fail builds on critical findings
4. Commission an **external penetration test** from a qualified third-party firm — scope to cover external attack surface and internal AWS environment
5. Define **Vulnerability Remediation SLAs**:
   - Critical (CVSS 9.0–10.0): 24 hours
   - High (CVSS 7.0–8.9): 7 days
   - Medium (CVSS 4.0–6.9): 30 days
   - Low (CVSS 0.1–3.9): 90 days
6. Assign a vulnerability management owner responsible for tracking remediation against SLAs
7. Schedule **quarterly vulnerability review** as standing meeting

**Success Metric:** AWS Inspector and Dependabot active across all accounts and repositories. First penetration test completed. Remediation SLAs formally documented.

**SOC 2 TSC:** CC7.1 | **NYDFS:** 500.05

---

### 2.4 Deploy EDR and Build Incident Handling Capability
**Controls Addressed:** IR-4, IR-6
**Risk Eliminated:** 🔴 High / 🟡 Medium
**Effort:** High — tooling, process, and training effort
**Owner:** IT / Engineering / Security

**Actions:**
1. Deploy an **EDR solution** to 100% of managed endpoints:
   - Recommended: CrowdStrike Falcon, SentinelOne, or Microsoft Defender for Endpoint
2. Integrate EDR alerts into SIEM for correlated detection
3. Develop **Incident Response Playbooks** for priority scenarios:
   - **Ransomware / Malware Outbreak** — isolation, forensics, recovery
   - **Data Exfiltration** — detection, containment, regulatory notification trigger
   - **Account Compromise / Credential Theft** — revocation, investigation, remediation
   - **Insider Threat** — evidence preservation, HR and legal escalation
   - **DDoS / Availability Event** — mitigation, communication, escalation
4. Establish **regulatory notification runbook** mapping incident types and severity levels to applicable reporting obligations, timelines, and regulatory contact information (NYDFS, SEC, FTC, state AG as applicable)
5. Conduct **first tabletop exercise** — simulate a ransomware scenario with the defined IR team
6. Establish a **post-incident review process** — blameless post-mortems documented within 5 business days of incident closure

**Success Metric:** EDR deployed on 100% of managed endpoints. Five IR playbooks completed. First tabletop exercise conducted.

**SOC 2 TSC:** CC7.4 | **NYDFS:** 500.16

---

### 2.5 Infrastructure Baseline and Asset Inventory
**Controls Addressed:** CM-2, CM-8
**Risk Eliminated:** 🔴 High / 🟡 Medium
**Effort:** Medium — IaC discipline and tooling
**Owner:** DevOps / Engineering

**Actions:**
1. Designate existing **Terraform codebase as authoritative** — freeze manual console changes in production
2. Conduct a **Terraform import exercise** to bring existing unmanaged AWS resources under IaC management
3. Enable **AWS Config** with conformance packs aligned to CIS AWS Foundations Benchmark
4. Define and enforce a mandatory **AWS resource tagging policy** covering: Environment, Owner, CostCenter, DataClassification, CreatedDate
5. Implement automated remediation for untagged resources via AWS Config Rules
6. Establish a **cloud asset inventory** using AWS Config as the authoritative source of truth
7. Reconcile endpoint inventory — ensure 100% of devices are enrolled in Jamf (or equivalent MDM)
8. Document baseline configurations for: EC2 AMIs, security groups, IAM permission boundaries, RDS parameter groups

**Success Metric:** Terraform established as authoritative IaC. AWS Config active with CIS conformance pack. 100% of cloud assets tagged and inventoried.

**SOC 2 TSC:** CC7.1, CC6.1 | **NYDFS:** 500.13

---

### 2.6 Remote Access Policy and Zero Trust Controls
**Controls Addressed:** AC-17
**Risk Eliminated:** 🟡 Medium
**Effort:** Medium
**Owner:** IT / Engineering

**Actions:**
1. Draft and publish a **Remote Access Policy** defining:
   - Approved devices (managed, MDM-enrolled only)
   - Required security posture (OS patched, EDR active, disk encrypted, screen lock enforced)
   - Approved access methods and prohibited workarounds
2. Implement **IP allowlisting or conditional access** for AWS Console and API access — restrict to corporate IP ranges or VPN egress
3. Evaluate **ZTNA solution** (Cloudflare Access, Tailscale, or Zscaler Private Access) for internal resource access
4. Enforce **device compliance checks** via Google Workspace Context-Aware Access or equivalent — non-compliant devices denied access to corporate SaaS

**Success Metric:** Remote Access Policy published. Conditional access enforced for AWS console. Device compliance checks active.

**SOC 2 TSC:** CC6.6 | **NYDFS:** 500.12

---

### 2.7 Security Awareness and IR Training Program
**Controls Addressed:** IR-2
**Risk Eliminated:** 🟡 Medium
**Effort:** Low-Medium — platform selection and content deployment
**Owner:** People Operations / Security Lead

**Actions:**
1. Select and deploy a **security awareness training platform** (KnowBe4, Proofpoint Security Awareness, or SANS Security Awareness)
2. Assign mandatory **baseline security awareness training** to all employees — target completion within 60 days of deployment
3. Deploy **role-based IR training** modules:
   - All staff: How to recognize and report a phishing email or security incident
   - Engineering: Secure coding basics, secrets hygiene, incident escalation
   - Leadership: Regulatory obligations, breach notification decision-making
4. Launch **quarterly phishing simulation** program — track click rates and improve over time
5. Establish **annual training recurrence** as an HR-enforced requirement tied to performance review cycle

**Success Metric:** 100% of employees complete baseline training within 60 days. Quarterly phishing simulations running. Role-based IR modules completed by technical staff.

**SOC 2 TSC:** CC9.2 | **NYDFS:** 500.14

---

### 2.8 Customer Authentication Upgrade
**Controls Addressed:** IA-8
**Risk Eliminated:** 🟡 Medium
**Effort:** Medium — product engineering effort
**Owner:** Product Engineering

**Actions:**
1. Remove SMS OTP as the sole MFA option for customers — add **TOTP (Google Authenticator / Authy)** and **push notification MFA**
2. For high-value or institutional accounts, enforce MFA as mandatory (not optional)
3. Implement **API key expiration** — set maximum key lifetime of 90 days with automated rotation reminders
4. Implement **API key scoping** — keys issued with minimum required permissions per integration
5. Add **FIDO2 / passkey support** to the product authentication roadmap for future implementation

**Success Metric:** TOTP MFA available to all customers. API key expiration enforced. SMS-only MFA path eliminated.

**SOC 2 TSC:** CC6.1 | **NYDFS:** 500.07

---

## Phase 3 — Maturity and Sustainment (90–180 Days)

> **Objective:** Harden configurations to industry benchmarks, automate compliance monitoring, and build the operational rhythms that sustain the security program long-term. This phase positions AcmeCorp for SOC 2 Type II audit readiness.

---

### 3.1 Apply CIS Benchmark Hardening Standards
**Controls Addressed:** CM-6
**Effort:** Medium
**Owner:** DevOps / IT

**Actions:**
1. Apply **CIS macOS Benchmark** via Jamf configuration profiles to all managed endpoints
2. Apply **CIS AWS Foundations Benchmark** via AWS Security Hub — resolve all Level 1 findings within 30 days of enablement
3. Define a **deviation management process** — document, approve, and track any intentional departures from baseline with compensating controls
4. Apply hardening standards to: EC2 AMI golden images, RDS parameter groups, S3 bucket policies, Lambda execution roles

**Success Metric:** CIS Benchmark compliance >90% across endpoints and AWS environment. Deviation register established.

**SOC 2 TSC:** CC7.1 | **NYDFS:** 500.13

---

### 3.2 Hire Dedicated Security Leadership
**Controls Addressed:** All families — governance foundation
**Effort:** High — recruiting
**Owner:** CEO / Board

**Actions:**
1. Define role requirements for **Director of Security or CISO** — emphasize GRC, cloud security, and fintech regulatory experience
2. Establish direct reporting line to CEO or Board
3. Charter the security function with budget authority, headcount, and executive sponsorship
4. Upon hire, transition program ownership from CTO to the security leader
5. Consider a **virtual CISO (vCISO)** engagement as an interim measure while recruiting

**Success Metric:** Security leader hired or vCISO engaged. Security function formally chartered with dedicated budget.

---

### 3.3 Continuous Compliance Monitoring
**Controls Addressed:** All families — ongoing
**Effort:** Medium
**Owner:** Security Lead / DevOps

**Actions:**
1. Enable **AWS Security Hub** as the centralized compliance dashboard — aggregate findings from Inspector, Config, GuardDuty, and Macie
2. Enable **Amazon GuardDuty** for threat detection across all AWS accounts
3. Enable **Amazon Macie** for sensitive data discovery in S3
4. Implement a **GRC platform** (Drata, Vanta, or Secureframe) to automate SOC 2 evidence collection — map controls to continuous monitoring checks
5. Establish a **monthly security metrics review** for leadership: vulnerability counts by severity, MFA compliance rate, training completion rate, open risk register items, security incidents

**Success Metric:** AWS Security Hub, GuardDuty, and Macie active across all accounts. GRC platform collecting automated SOC 2 evidence. Monthly security metrics dashboard operational.

**SOC 2 TSC:** All | **NYDFS:** 500.09

---

### 3.4 SOC 2 Type II Readiness Assessment
**Controls Addressed:** All families
**Effort:** Medium — external engagement
**Owner:** Security Lead / Legal / Finance

**Actions:**
1. Engage a qualified **SOC 2 auditor** (Big 4 or specialized firm) for a readiness assessment 3 months before target audit period
2. Complete a **gap-to-audit remediation sprint** addressing any remaining findings from the readiness assessment
3. Ensure all policies are reviewed, approved, and dated within 12 months of audit period start
4. Compile **evidence library** — policy documents, access review records, training completion reports, vulnerability scan results, incident logs, configuration screenshots
5. Begin **12-month SOC 2 Type II observation period** — maintain control operation consistently throughout

**Success Metric:** SOC 2 readiness assessment completed. Evidence library established. Audit period initiated.

---

## Remediation Progress Tracker

Use this table to track completion as items are implemented. Update status and date as controls are remediated.

| Control ID | Remediation Action | Priority | Target Date | Owner | Status | Completion Date |
|------------|-------------------|----------|-------------|-------|--------|-----------------|
| IA-2 | Enforce MFA — all users | P1 | Day 7 | IT Lead | ⬜ Not Started | — |
| IA-5 | Rotate secrets, deploy Secrets Manager | P1 | Day 14 | Eng Lead | ⬜ Not Started | — |
| AC-2 | Account audit + JML process | P1 | Day 21 | IT / HR | ⬜ Not Started | — |
| AC-1 | Draft Access Control Policy | P1 | Day 30 | CTO | ⬜ Not Started | — |
| AU-2 | Enable CloudTrail all accounts | P1 | Day 14 | DevOps | ⬜ Not Started | — |
| IR-1 | Draft IR Policy + assign roles | P1 | Day 30 | CTO / Legal | ⬜ Not Started | — |
| RA-2 | Data classification workshop | P1 | Day 30 | CTO / Legal | ⬜ Not Started | — |
| RA-3 | Enterprise risk assessment | P1 | Day 30 | CTO | ⬜ Not Started | — |
| AC-6 | RBAC model + PAM deployment | P2 | Day 60 | Eng Lead | ⬜ Not Started | — |
| AU-9 | S3 Object Lock + isolated log account | P2 | Day 45 | DevOps | ⬜ Not Started | — |
| AU-12 | RDS logging + log schema standard | P2 | Day 45 | DevOps | ⬜ Not Started | — |
| RA-5 | AWS Inspector + Snyk + pen test | P2 | Day 75 | DevOps | ⬜ Not Started | — |
| IR-4 | EDR + SIEM + playbooks + tabletop | P2 | Day 90 | IT / Eng | ⬜ Not Started | — |
| IR-6 | Notification runbook + reporting channel | P2 | Day 60 | Legal | ⬜ Not Started | — |
| CM-2 | Terraform as authoritative + AWS Config | P2 | Day 75 | DevOps | ⬜ Not Started | — |
| CM-8 | Cloud asset inventory + tagging policy | P2 | Day 60 | DevOps | ⬜ Not Started | — |
| AC-17 | Remote access policy + ZTNA eval | P2 | Day 75 | IT | ⬜ Not Started | — |
| IR-2 | Security awareness training platform | P2 | Day 90 | HR / Security | ⬜ Not Started | — |
| IA-8 | Customer MFA upgrade + API key expiry | P2 | Day 90 | Product Eng | ⬜ Not Started | — |
| CM-6 | CIS Benchmark hardening | P3 | Day 150 | DevOps / IT | ⬜ Not Started | — |

---

## Estimated Resource Requirements

| Category | Phase 1 (0–30 days) | Phase 2 (30–90 days) | Phase 3 (90–180 days) |
|----------|--------------------|--------------------|----------------------|
| Engineering Hours | ~80 hrs | ~240 hrs | ~120 hrs |
| IT / Ops Hours | ~40 hrs | ~80 hrs | ~40 hrs |
| Legal / Compliance Hours | ~20 hrs | ~20 hrs | ~40 hrs |
| Tooling Cost (est. monthly) | ~$500–$1,000 | ~$5,000–$15,000 | ~$10,000–$25,000 |
| External Services | — | Pen test: $15K–$30K | SOC 2 audit: $30K–$60K |

> **Note:** Tooling cost estimates are directional and will vary based on vendor selection, negotiation, and AcmeCorp's AWS consumption. A GRC platform (Drata/Vanta) in Phase 3 typically runs $15K–$40K annually and can significantly reduce internal effort for SOC 2 evidence collection.

---

## Key Milestones

| Milestone | Target | Significance |
|-----------|--------|-------------|
| MFA enforced — 100% of users | Day 7 | Eliminates #1 account compromise vector |
| Secrets rotated — all repos clean | Day 14 | Closes critical credential exposure |
| IR Policy approved and published | Day 30 | Meets NYDFS 500.16 minimum requirement |
| SIEM operational | Day 60 | Enables detection and audit log protection |
| First tabletop exercise | Day 90 | Validates IR capability |
| First penetration test completed | Day 75 | Identifies unknown attack surface |
| SOC 2 readiness assessment | Day 150 | Go / no-go decision for audit period start |
| SOC 2 Type II audit period begins | Day 180 | 12-month observation period starts |

---

*This roadmap is a living document and should be reviewed and updated monthly as items are completed and new risks are identified. Ownership of this roadmap should transfer to AcmeCorp's designated security leader upon appointment.*

*Companion documents: `gap-analysis-report.md` | `control-matrix.md` | `README.md`*

*Assessment Date: April 2026 | Framework: NIST SP 800-53 Rev 5 | Prepared by: Cybersecurity Advisory Practice*
