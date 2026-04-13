# NIST 800-53 Rev 5 Compliance Gap Analysis Report
## AcmeCorp — Confidential

---

**Prepared For:** AcmeCorp Executive Leadership & Security Steering Committee
**Prepared By:** Cybersecurity Advisory Practice
**Report Classification:** Confidential — Internal Use Only
**Assessment Date:** April 2026
**Report Version:** 1.0
**Document Status:** Final

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope and Methodology](#2-scope-and-methodology)
3. [Organization Background](#3-organization-background)
4. [Assessment Findings by Control Family](#4-assessment-findings-by-control-family)
   - 4.1 Access Control (AC)
   - 4.2 Audit and Accountability (AU)
   - 4.3 Configuration Management (CM)
   - 4.4 Identification and Authentication (IA)
   - 4.5 Incident Response (IR)
   - 4.6 Risk Assessment (RA)
5. [Consolidated Risk Summary](#5-consolidated-risk-summary)
6. [Strategic Recommendations](#6-strategic-recommendations)
7. [Conclusion](#7-conclusion)
8. [Appendix A — Controls Reference Map](#appendix-a--controls-reference-map)
9. [Appendix B — Risk Rating Methodology](#appendix-b--risk-rating-methodology)

---

## 1. Executive Summary

AcmeCorp engaged this advisory practice to conduct an independent compliance gap analysis against the National Institute of Standards and Technology (NIST) Special Publication 800-53, Revision 5 control catalog. This assessment was initiated in anticipation of AcmeCorp's upcoming SOC 2 Type II readiness program and serves as a foundational baseline for the organization's broader information security governance, risk, and compliance (GRC) function.

AcmeCorp is a fintech startup headquartered in New York, NY, with approximately 500 employees, operating in a period of rapid organizational and technical growth. The company currently processes financial data on behalf of retail and institutional clients, creating regulatory obligations under applicable federal and state frameworks including the Gramm-Leach-Bliley Act (GLBA), New York Department of Financial Services (NYDFS) Cybersecurity Regulation (23 NYCRR 500), and various Payment Card Industry (PCI-DSS) requirements depending on transaction scope.

**Assessment Scope:** Eighteen (18) controls across six (6) NIST 800-53 Rev 5 control families were evaluated: Access Control (AC), Audit and Accountability (AU), Configuration Management (CM), Identification and Authentication (IA), Incident Response (IR), and Risk Assessment (RA).

### Key Findings at a Glance

| Risk Rating | Control Gaps Identified |
|-------------|------------------------|
| 🔴 High     | 8                      |
| 🟡 Medium   | 6                      |
| 🟢 Low      | 4                      |
| **Total**   | **18**                 |

The assessment uncovered significant deficiencies across all six control families reviewed. Most critically, AcmeCorp has **no formalized Incident Response plan**, **no enterprise-wide privileged access management (PAM) solution**, and **no documented risk assessment process**. These gaps collectively represent material risk to the organization's ability to detect, contain, and recover from a security event — a posture that is incompatible with the trust obligations AcmeCorp carries as a fintech processor.

### Priority Risk Areas

The following three areas require immediate executive attention and resource commitment:

1. **Incident Response Readiness (IR):** AcmeCorp has no documented incident response policy, no defined response team, and no tested playbooks. In the event of a breach or ransomware event, the organization would operate entirely on improvised, uncoordinated individual efforts.

2. **Privileged Access Governance (AC/IA):** Administrative and privileged accounts are provisioned informally, without time-bound access controls, formal approval workflows, or segregation of duties enforcement. This represents a direct attack surface and insider threat vector.

3. **Risk Governance Infrastructure (RA):** No enterprise risk register, threat modeling process, or vulnerability management program exists. Without a risk management foundation, investment prioritization and executive decision-making on security topics lack defensible, data-driven basis.

This report provides detailed findings for each control evaluated, along with risk ratings and actionable remediation guidance. A companion remediation roadmap (see `remediation-roadmap.md`) provides phased implementation timelines for addressing identified gaps.

---

## 2. Scope and Methodology

### 2.1 Assessment Scope

This gap analysis evaluated AcmeCorp's current security posture against eighteen selected controls from the NIST SP 800-53 Rev 5 catalog. Controls were selected based on:

- Relevance to AcmeCorp's operational environment (cloud-hosted SaaS platform, remote-hybrid workforce)
- Alignment with SOC 2 Trust Services Criteria (TSC), particularly the Security (CC) category
- Regulatory applicability under NYDFS 23 NYCRR 500 and GLBA Safeguards Rule
- Industry-standard prioritization for fintech organizations in early security maturity stages

The following assets and domains were included in scope:

- AcmeCorp's production cloud infrastructure (AWS us-east-1 primary region)
- Corporate identity provider and directory services (Google Workspace)
- Internal development and CI/CD pipelines
- Endpoint fleet (MacOS and Windows managed devices)
- Third-party SaaS platforms with access to customer financial data
- Organizational policies, procedures, and governance documentation

**Explicitly out of scope** for this engagement: physical security controls, supply chain risk management (SR family), privacy controls (PT family), and third-party vendor contracts.

### 2.2 Methodology

This assessment was conducted using the following approach:

**Phase 1 — Documentation Review**
Existing security policies, system configuration standards, network diagrams, and organizational charts were reviewed to establish a documentation baseline. Where documentation did not exist, findings were recorded as complete gaps.

**Phase 2 — Stakeholder Interviews**
Structured interviews were conducted with key personnel including the CTO, VP of Engineering, Head of People Operations, and two senior engineers. Interview questions were mapped to NIST control requirements to gather evidence of control implementation or absence.

**Phase 3 — Technical Observation**
Where access was provided, cloud configuration settings (IAM policies, logging configurations, security group rules), endpoint management configurations, and authentication settings were reviewed against NIST control requirements and industry best practices.

**Phase 4 — Gap Identification and Risk Scoring**
Identified gaps were evaluated using a risk-based framework (see Appendix B). Each gap was assigned a risk rating of High, Medium, or Low based on the combination of likelihood of exploitation and potential impact to AcmeCorp's confidentiality, integrity, and availability posture.

**Phase 5 — Reporting**
Findings were consolidated into this report and companion deliverables: a control matrix (`control-matrix.md`) and prioritized remediation roadmap (`remediation-roadmap.md`).

### 2.3 Frameworks and Standards Referenced

| Framework | Version | Purpose |
|-----------|---------|---------|
| NIST SP 800-53 | Revision 5 (2020) | Primary control catalog |
| NIST Cybersecurity Framework | 2.0 (2024) | Supplementary risk context |
| ISO/IEC 27001 | 2022 Edition | Cross-reference for international alignment |
| SOC 2 (AICPA TSC) | 2017 Revised | Audit readiness alignment |
| NYDFS 23 NYCRR 500 | 2023 Amendment | Regulatory obligation baseline |

### 2.4 Maturity Rating Definitions

Each control was assessed against a simplified maturity scale:

| Status | Definition |
|--------|-----------|
| **Implemented** | Control is fully documented, operationally active, and evidence exists of consistent execution |
| **Partial** | Control exists in some form but lacks completeness, consistency, or formal documentation |
| **Not Implemented** | No evidence of control implementation; gap is complete |
| **Not Applicable** | Control does not apply to AcmeCorp's environment or scope |

---

## 3. Organization Background

**Organization:** AcmeCorp, Inc.
**Industry:** Financial Technology (Fintech)
**Headquarters:** New York, NY
**Employee Count:** ~500
**Technology Environment:** AWS-hosted, cloud-native SaaS platform; Google Workspace; GitHub Enterprise; remote-hybrid workforce
**Current Security Maturity:** Initial / Ad Hoc (CMMI Level 1)
**Regulatory Obligations:** GLBA, NYDFS 23 NYCRR 500, PCI-DSS (pending scope determination), SOC 2 Type II (target)

AcmeCorp has experienced rapid growth over the past 18 months, scaling from approximately 80 to 500 employees. Security and compliance programs were not formally prioritized during this growth phase, resulting in a fragmented, point-solution security posture with minimal governance infrastructure. Engineering velocity has been the organizational priority, which, while commercially understandable, has produced significant accumulated security debt.

The organization does not currently employ a dedicated Chief Information Security Officer (CISO) or a security team. Security responsibilities are informally distributed among the engineering organization, with no individual holding formal accountability for the security program. This structural gap is itself a finding and will be noted where relevant throughout this report.

---

## 4. Assessment Findings by Control Family

---

### 4.1 Access Control (AC)

The Access Control family governs the policies and mechanisms by which AcmeCorp manages who can access systems, data, and functions — and under what conditions. Given AcmeCorp's cloud-native, SaaS-heavy environment and its role handling financial data, access governance is a foundational security requirement.

---

#### Finding AC-1: Access Control Policy and Procedures
**Control:** AC-1 — Policy and Procedures
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** NIST AC-1 requires the organization to develop, document, disseminate, and periodically review an access control policy that addresses purpose, scope, roles, responsibilities, and compliance, along with procedures to facilitate implementation.

**Current State:** AcmeCorp has no formal, documented access control policy. Access provisioning decisions are made on an informal, ad hoc basis by engineering leads and the IT function. There is no policy governing least privilege, role-based access control (RBAC), access review cycles, or separation of duties.

**Gap:** Complete absence of a foundational access control governance document. No procedures exist for provisioning, modifying, or revoking access in a consistent, documented manner.

**Risk Narrative:** Without an access control policy, AcmeCorp cannot demonstrate to auditors, regulators, or customers that access to sensitive systems and data is governed by any consistent standard. This gap directly fails the SOC 2 CC6.1 criterion and represents a regulatory exposure under NYDFS 500.07 (access privilege requirements).

**Remediation:** Draft and formally approve an Access Control Policy aligned to NIST AC-1 requirements. Define RBAC model, least privilege principle, and access review cadence. Assign policy ownership to an accountable security or IT leader.

---

#### Finding AC-2: Account Management
**Control:** AC-2 — Account Management
**Status:** Partial
**Risk Rating:** 🔴 High

**Control Requirement:** AC-2 requires the organization to manage information system accounts, including establishing account types, assigning account managers, requiring approvals for access requests, monitoring accounts for atypical use, and removing accounts when no longer required.

**Current State:** User accounts are created in Google Workspace and AWS IAM upon hire or project assignment. Account creation is triggered informally via Slack messages to IT or engineering leads. No formal account management system or approval workflow exists. Offboarding processes are inconsistently executed; interviews revealed that in at least three documented cases, former employees retained active Google Workspace access for more than 30 days post-termination.

**Gap:** No formal account lifecycle management process. Offboarding is inconsistent and not enforced through automated controls. No account manager role is formally designated. Accounts are not reviewed on a periodic basis.

**Risk Narrative:** Orphaned accounts belonging to former employees represent a direct, exploitable threat vector. Combined with the absence of MFA enforcement (see IA-5), these accounts create a pathway for unauthorized access to AcmeCorp's production environment and customer financial data.

**Remediation:** Implement an Identity Governance and Administration (IGA) solution or formalize HR-to-IT offboarding workflows with automated account suspension triggers. Conduct an immediate audit of all active accounts against current employee roster.

---

#### Finding AC-6: Least Privilege
**Control:** AC-6 — Least Privilege
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** AC-6 requires the organization to employ the principle of least privilege, allowing only authorized accesses necessary for users to accomplish assigned tasks. Enhancement AC-6(5) specifically requires restricting privileged accounts to dedicated administrative use.

**Current State:** AWS IAM review revealed that a significant number of developer accounts carry broad IAM permissions, including several accounts with AdministratorAccess policies attached directly. There is no distinction between developer, read-only, and administrative permission tiers in practice. Engineers routinely use their standard user credentials to perform administrative tasks.

**Gap:** No RBAC model has been designed or implemented. Least privilege is not enforced at the IAM policy level. Privileged access is not restricted to dedicated privileged accounts.

**Risk Narrative:** Overly permissive IAM policies dramatically expand the blast radius of any account compromise. An attacker who obtains credentials for a developer account may be able to exfiltrate data, modify infrastructure, or destroy audit logs — actions that should be restricted to a small number of privileged administrators operating under enhanced controls.

**Remediation:** Implement RBAC model across AWS and core SaaS platforms. Enforce least privilege via IAM policy redesign. Deploy a Privileged Access Management (PAM) solution for administrative credential management. Require separate privileged accounts for administrative tasks.

---

#### Finding AC-17: Remote Access
**Control:** AC-17 — Remote Access
**Status:** Partial
**Risk Rating:** 🟡 Medium

**Control Requirement:** AC-17 requires the organization to establish and document usage restrictions, configuration requirements, and connection requirements for each type of remote access allowed, and to authorize each type before allowing such connections.

**Current State:** AcmeCorp operates a fully remote-hybrid workforce. Remote access to cloud resources occurs via public internet without a formal VPN or Zero Trust Network Access (ZTNA) solution. Access to the AWS console is permitted from any IP address. No documented remote access policy or device compliance requirements exist.

**Gap:** No remote access policy. No network-level access controls restricting console and API access to managed, known-good endpoints. No device posture assessment integrated with access decisions.

**Risk Narrative:** The absence of remote access controls allows any compromised credential — regardless of the device's security posture — to reach AcmeCorp's production environment. This is particularly acute given AcmeCorp's fintech data exposure.

**Remediation:** Implement a ZTNA or VPN solution for administrative access. Enforce IP allowlisting or conditional access policies for cloud console access. Develop and publish a Remote Access Policy.

---

### 4.2 Audit and Accountability (AU)

The Audit and Accountability control family requires organizations to create, protect, and retain audit records sufficient to enable monitoring, analysis, investigation, and reporting of unlawful, unauthorized, or inappropriate activity.

---

#### Finding AU-2: Event Logging
**Control:** AU-2 — Event Logging
**Status:** Partial
**Risk Rating:** 🔴 High

**Control Requirement:** AU-2 requires the organization to identify the types of events that the system is capable of logging, coordinate the event logging function with other organizations, and provide a rationale for why the selected events are deemed adequate.

**Current State:** AWS CloudTrail is enabled in the primary production account (us-east-1) but is not enabled in all AWS accounts within AcmeCorp's organization. CloudTrail logs are stored in S3 but are not centrally aggregated into a SIEM or log management platform. Application-level logging is inconsistent across services — some microservices emit structured logs to CloudWatch, others produce minimal output. No formal event logging standard exists defining which events must be logged across what systems.

**Gap:** Incomplete logging coverage across the AWS organization. No centralized log aggregation. No defined event logging standard. Application logging is ad hoc and inconsistent.

**Risk Narrative:** Incomplete audit logging directly impairs AcmeCorp's ability to detect security events, conduct forensic investigation, and demonstrate compliance to auditors. In the event of a breach, gaps in log coverage may prevent the organization from determining the scope, timeline, or root cause of the incident.

**Remediation:** Enable CloudTrail in all AWS accounts and regions. Define an enterprise event logging standard covering authentication events, privileged actions, data access, and configuration changes. Deploy a SIEM solution (e.g., Splunk, Elastic, or cloud-native AWS Security Hub + OpenSearch) for centralized aggregation and correlation.

---

#### Finding AU-9: Protection of Audit Information
**Control:** AU-9 — Protection of Audit Information
**Status:** Not Implemented
**Risk Rating:** 🟡 Medium

**Control Requirement:** AU-9 requires the organization to protect audit information and audit tools from unauthorized access, modification, and deletion. Enhancement AU-9(2) requires the backup of audit records to a physically different system or component than the system being audited.

**Current State:** CloudTrail logs are stored in an S3 bucket within the same AWS account as the production environment. S3 Object Lock is not enabled. Bucket access policies do not restrict deletion. Developer accounts with broad IAM permissions (see AC-6) can read, modify, or delete CloudTrail log files.

**Gap:** Audit logs are not write-protected. Logs reside in the same account and trust boundary as production systems, enabling any account with AdministratorAccess to tamper with or destroy evidence of malicious activity.

**Risk Narrative:** An attacker — or a malicious insider — who compromises a privileged account could cover their tracks by deleting or modifying CloudTrail logs before the organization detects the intrusion. This undermines the entire purpose of the logging function.

**Remediation:** Enable S3 Object Lock (WORM mode) on the CloudTrail log bucket. Restrict log bucket access using a dedicated, isolated AWS account in AWS Organizations. Implement log forwarding to an immutable, out-of-band destination.

---

#### Finding AU-12: Audit Record Generation
**Control:** AU-12 — Audit Record Generation
**Status:** Partial
**Risk Rating:** 🟡 Medium

**Control Requirement:** AU-12 requires the information system to generate audit records for the events defined in AU-2 at all components capable of auditing, and to allow designated personnel to select which auditable events are to be logged.

**Current State:** Audit record generation is enabled for AWS API calls via CloudTrail but is not systematically enabled at the operating system, database, or application tiers. Database query logging is disabled on the primary RDS instances. No structured audit record format standard has been defined.

**Gap:** Audit generation is incomplete; critical data layers (database, application) produce insufficient or no audit records. No standardized log format to enable correlation.

**Remediation:** Enable RDS query logging and export to centralized log storage. Define a log schema standard (e.g., ECS or CEF format). Ensure all application services emit structured audit events for authentication, authorization decisions, and data access operations.

---

### 4.3 Configuration Management (CM)

Configuration management controls address how AcmeCorp establishes and maintains the integrity of its systems through defined, approved, and enforced configuration baselines.

---

#### Finding CM-2: Baseline Configuration
**Control:** CM-2 — Baseline Configuration
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** CM-2 requires the organization to develop, document, and maintain a current baseline configuration of the information system. Enhancement CM-2(2) requires the organization to retain previous versions of baseline configurations to support rollback.

**Current State:** No formal baseline configuration exists for any component of AcmeCorp's infrastructure. AWS infrastructure was built iteratively without Infrastructure as Code (IaC) discipline. While some Terraform modules exist in the repository, they are not maintained as authoritative and do not reflect the current production state. EC2 instances are managed without a configuration management tool (no Ansible, Chef, or SSM desired state configuration applied). Security group rules have accumulated without periodic review.

**Gap:** No documented baseline configurations. Infrastructure state is partially reflected in IaC that is not maintained as authoritative. No process to detect or remediate configuration drift.

**Risk Narrative:** Without configuration baselines, AcmeCorp cannot consistently enforce a known-good security posture, detect unauthorized changes, or demonstrate to auditors that systems are configured to a standard. Configuration drift creates opportunities for exploitation of misconfigured services.

**Remediation:** Adopt Infrastructure as Code as authoritative (Terraform or CloudFormation). Implement AWS Config to enforce configuration compliance rules. Define and document CIS Benchmark-aligned configuration baselines for all major infrastructure components.

---

#### Finding CM-6: Configuration Settings
**Control:** CM-6 — Configuration Settings
**Status:** Not Implemented
**Risk Rating:** 🟡 Medium

**Control Requirement:** CM-6 requires the organization to establish and document configuration settings for IT products employed within the information system that reflect the most restrictive mode consistent with operational requirements, and to identify, document, and approve any deviations.

**Current State:** No security configuration standards (hardening guides) exist for AcmeCorp's endpoints, servers, or cloud services. Default configurations are used for most services. MacOS endpoint management is performed via Jamf but without a defined hardening benchmark applied. No process exists to document or approve deviations from a security baseline.

**Gap:** No configuration hardening standards. No deviation management process. Endpoints and services are deployed with default or unknown configuration states.

**Remediation:** Adopt CIS Benchmarks for macOS, AWS Foundations, and applicable SaaS platforms. Configure Jamf policies to enforce OS hardening settings. Implement AWS Security Hub CIS benchmark assessment.

---

#### Finding CM-8: System Component Inventory
**Control:** CM-8 — System Component Inventory
**Status:** Partial
**Risk Rating:** 🟡 Medium

**Control Requirement:** CM-8 requires the organization to develop, document, and maintain an inventory of system components that accurately reflects the current system, includes all components within the authorization boundary, contains sufficient information to achieve effective component accountability, and is available for review.

**Current State:** A partial asset inventory exists in a spreadsheet maintained by the IT function, covering managed endpoints (approximately 400 of the 500 employee devices). Cloud infrastructure assets (EC2, RDS, Lambda, S3 buckets) are not formally inventoried outside of AWS resource tags, which are applied inconsistently. Shadow IT and personal device use are not tracked.

**Gap:** Asset inventory is incomplete, not authoritative, and does not cover cloud resources or data assets. No automated discovery process refreshes the inventory.

**Remediation:** Implement an automated asset discovery solution (e.g., AWS Config for cloud assets, Jamf for endpoints). Enforce mandatory resource tagging policy in AWS. Conduct a data classification exercise to identify critical data assets.

---

### 4.4 Identification and Authentication (IA)

The Identification and Authentication family governs how AcmeCorp ensures that users, processes, and devices are uniquely identified and authenticated before being granted access to systems and data.

---

#### Finding IA-2: Identification and Authentication (Organizational Users)
**Control:** IA-2 — Identification and Authentication (Organizational Users)
**Status:** Partial
**Risk Rating:** 🔴 High

**Control Requirement:** IA-2 requires the information system to uniquely identify and authenticate organizational users. Enhancement IA-2(1) specifically requires multi-factor authentication (MFA) for access to privileged accounts. IA-2(2) extends this requirement to non-privileged accounts.

**Current State:** Google Workspace is the primary identity provider. MFA is available but enforcement is **not mandatory** — it is configured as optional, left to individual user preference. A review of the Google Admin console found that approximately 38% of active user accounts do not have any MFA method enrolled. AWS IAM MFA enforcement via a Service Control Policy (SCP) is not implemented at the AWS Organizations level, meaning root and IAM user accounts can authenticate with password alone.

**Gap:** MFA is not enforced for the majority of organizational users or privileged accounts. A significant percentage of active accounts are protected only by password.

**Risk Narrative:** Password-only authentication is insufficient for a fintech organization processing financial data. Credential theft via phishing, credential stuffing, or password spray attacks represents a high-likelihood, high-impact threat. Failure to enforce MFA is one of the single most common root causes of organizational data breaches.

**Remediation:** Immediately enforce MFA for all users via Google Workspace Admin policy. Implement an SCP in AWS Organizations requiring MFA for IAM operations. Enforce phishing-resistant MFA (FIDO2/WebAuthn) for privileged users. Target: 100% MFA enrollment within 30 days.

---

#### Finding IA-5: Authenticator Management
**Control:** IA-5 — Authenticator Management
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** IA-5 requires the organization to manage information system authenticators including establishing initial authenticator content, enforcing restrictions on reuse, protecting authenticators from unauthorized disclosure, and changing default authenticators.

**Current State:** No enterprise password policy is formally defined or technically enforced. Google Workspace password settings have not been configured beyond defaults. Password complexity requirements are minimal. Password history enforcement is disabled, permitting users to cycle back to previous passwords immediately. No enterprise password manager is deployed or mandated. Service account credentials (API keys, database passwords) are managed in plaintext in environment variable files checked into source code repositories (confirmed via repository scan).

**Gap:** No enforceable password policy. Secrets management is absent; credentials are stored in plaintext in code repositories. No controls over authenticator lifecycle or strength.

**Risk Narrative:** Hardcoded credentials in source repositories represent a critical, immediately exploitable vulnerability. Exposed credentials may grant attackers direct access to production databases, third-party APIs, or cloud infrastructure. This is a known, actively exploited attack vector (see OWASP A07:2021 — Identification and Authentication Failures).

**Remediation:** Immediately rotate all secrets found in repositories and move to a secrets management solution (AWS Secrets Manager or HashiCorp Vault). Enforce Google Workspace password policy (minimum 12 characters, complexity, 12-month rotation for privileged accounts). Deploy an enterprise password manager (1Password Business or similar). Conduct a secrets scanning exercise on all repositories.

---

#### Finding IA-8: Identification and Authentication (Non-Organizational Users)
**Control:** IA-8 — Identification and Authentication (Non-Organizational Users)
**Status:** Partial
**Risk Rating:** 🟡 Medium

**Control Requirement:** IA-8 requires the information system to uniquely identify and authenticate non-organizational users (or processes acting on behalf of non-organizational users).

**Current State:** AcmeCorp's customer-facing platform uses username/password authentication with optional SMS-based OTP. Federated identity (OAuth 2.0 via Google/Apple) is supported but SMS OTP does not meet current NIST SP 800-63B requirements for MFA assurance. API clients authenticate via API keys that are rotated annually at best and do not expire.

**Gap:** Customer authentication does not meet NIST 800-63B AAL2 requirements. API key management is immature with no automated rotation or expiration enforcement.

**Remediation:** Upgrade customer authentication to TOTP or push-notification MFA. Implement API key expiration policies and automated rotation. Consider adopting FIDO2 passkeys for customer authentication as a roadmap item.

---

### 4.5 Incident Response (IR)

The Incident Response control family addresses AcmeCorp's capacity to prepare for, detect, contain, eradicate, and recover from security incidents in a structured, documented, and tested manner.

---

#### Finding IR-1: Incident Response Policy and Procedures
**Control:** IR-1 — Policy and Procedures
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** IR-1 requires the organization to develop, document, disseminate, review, and update an incident response policy and procedures. The policy must address purpose, scope, roles, responsibilities, management commitment, coordination, and compliance.

**Current State:** No incident response policy exists at AcmeCorp. No written procedures govern how security incidents are to be identified, reported, escalated, or managed. In stakeholder interviews, senior engineering staff confirmed that if a breach were discovered today, the response would be improvised, with no predefined team, roles, communication plan, or escalation path.

**Gap:** Complete absence of incident response governance. No policy, no procedures, no defined roles.

**Risk Narrative:** Without an IR policy and procedures, AcmeCorp is unable to respond to incidents in a timely, coordinated, or legally defensible manner. This gap directly affects NYDFS 23 NYCRR 500.16 (incident response plan requirement) and SOC 2 CC7.3/CC7.4 criteria. In the absence of documented procedures, breach notification timelines — 72 hours under many regulatory frameworks — are unlikely to be met.

**Remediation:** Develop and approve a comprehensive Incident Response Policy and Procedure document. Define roles (Incident Commander, technical lead, legal/compliance liaison, communications lead). Distribute and train all relevant stakeholders within 60 days.

---

#### Finding IR-2: Incident Response Training
**Control:** IR-2 — Incident Response Training
**Status:** Not Implemented
**Risk Rating:** 🟡 Medium

**Control Requirement:** IR-2 requires the organization to provide incident response training to information system users consistent with assigned roles and responsibilities, and to update training as the information system changes.

**Current State:** No security awareness training program exists at AcmeCorp. Onboarding does not include security or incident response training. Engineering staff have received no formal training on how to recognize, report, or respond to security incidents.

**Gap:** No incident response training program. No role-based training for technical or non-technical staff. No security awareness baseline.

**Remediation:** Deploy a security awareness training platform (e.g., KnowBe4, Proofpoint Security Awareness). Develop incident-response-specific training modules for engineering, IT, and leadership roles. Conduct initial training within 90 days; mandate annual recurrence with quarterly phishing simulations.

---

#### Finding IR-4: Incident Handling
**Control:** IR-4 — Incident Handling
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** IR-4 requires the organization to implement an incident handling capability for security incidents that includes preparation, detection and analysis, containment, eradication, and recovery, and to coordinate incident handling activities with contingency planning activities.

**Current State:** No incident handling capability exists. There are no detection mechanisms integrated with alerting (no SIEM, no EDR). There is no defined containment playbook for any threat scenario. No post-incident review process exists.

**Gap:** Complete absence of incident handling capability across all five NIST incident response lifecycle phases.

**Risk Narrative:** A ransomware event, data exfiltration, or account compromise at AcmeCorp today would result in an uncontrolled, uncoordinated response. The organization lacks the tooling, processes, and trained personnel to contain damage, preserve evidence, or meet regulatory notification obligations.

**Remediation:** Deploy EDR solution on all managed endpoints. Implement SIEM for centralized alerting. Develop incident response playbooks for priority scenarios (ransomware, data exfiltration, account compromise, insider threat). Conduct tabletop exercise within 90 days of policy completion.

---

#### Finding IR-6: Incident Reporting
**Control:** IR-6 — Incident Reporting
**Status:** Not Implemented
**Risk Rating:** 🟡 Medium

**Control Requirement:** IR-6 requires the organization to require personnel to report suspected security incidents to the organizational incident response capability within an organizationally defined time period, and to report incident information to designated authorities.

**Current State:** No internal incident reporting mechanism exists. No designated reporting channel, ticketing queue, or on-call security contact has been established. No process maps the internal reporting process to regulatory notification requirements (NYDFS 72-hour notification, SEC cybersecurity incident disclosure rules).

**Gap:** No internal incident reporting channel or process. No regulatory notification workflow.

**Remediation:** Establish a security incident reporting channel (e.g., security@acmecorp.com, dedicated Slack channel with defined on-call rotation). Create a regulatory notification runbook mapping incident types to applicable reporting timelines and authorities. Assign legal/compliance ownership of the notification process.

---

### 4.6 Risk Assessment (RA)

The Risk Assessment family requires AcmeCorp to systematically identify, evaluate, and respond to risks to organizational operations, assets, and individuals arising from information system operation.

---

#### Finding RA-2: Security Categorization
**Control:** RA-2 — Security Categorization
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** RA-2 requires the organization to categorize information and the information system in accordance with applicable laws, directives, policies, regulations, and standards; document the results in the security plan; and review and update the categorization annually or whenever there is a significant change.

**Current State:** AcmeCorp has not conducted a formal security categorization exercise. No data classification scheme exists. Customer financial data, internal operational data, employee PII, and system configuration data are treated without distinction from a security controls perspective. No system security plan (SSP) or equivalent document exists.

**Gap:** No data classification or system categorization. Without categorization, controls cannot be appropriately scoped or prioritized, and compliance with frameworks requiring tiered protection (NIST, PCI-DSS, GLBA) cannot be demonstrated.

**Remediation:** Conduct a formal data classification exercise. Develop a Data Classification Policy with at minimum three tiers (Public, Internal, Confidential/Restricted). Apply classification labels to major data stores and information flows. Document results in a System Security Plan.

---

#### Finding RA-3: Risk Assessment
**Control:** RA-3 — Risk Assessment
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** RA-3 requires the organization to conduct a risk assessment that includes the likelihood and magnitude of harm from unauthorized access, use, disclosure, disruption, modification, or destruction of the information system and the information it processes, stores, or transmits, and that assesses risk on an ongoing basis.

**Current State:** No enterprise risk assessment has been conducted. No risk register exists. Risk decisions are made reactively and informally. There is no methodology for risk identification, likelihood estimation, impact quantification, or risk acceptance documentation. Leadership cannot identify, rank, or track security risks in any structured format.

**Gap:** No risk assessment process or methodology. No enterprise risk register. No risk acceptance framework.

**Risk Narrative:** Without a risk assessment foundation, AcmeCorp's security investments are not guided by risk data. Controls may be over-applied in low-risk areas and absent in high-risk areas. This gap undermines the entire GRC program and is a prerequisite for almost every other compliance framework AcmeCorp intends to pursue.

**Remediation:** Establish a risk assessment methodology (NIST SP 800-30 Rev 1 recommended). Conduct a full enterprise risk assessment within 90 days. Build and maintain a risk register tracked at the executive level. Define a risk acceptance process with documented approval authority.

---

#### Finding RA-5: Vulnerability Monitoring and Scanning
**Control:** RA-5 — Vulnerability Monitoring and Scanning
**Status:** Not Implemented
**Risk Rating:** 🔴 High

**Control Requirement:** RA-5 requires the organization to monitor and scan for vulnerabilities in the information system and hosted applications on an organizationally defined frequency, analyze vulnerability scan reports and results, and remediate legitimate vulnerabilities in accordance with an organizational assessment of risk.

**Current State:** No vulnerability scanning program exists. AWS Inspector has not been enabled. No DAST or SAST tooling is integrated into the CI/CD pipeline. Penetration testing has never been performed. Dependency vulnerability scanning (e.g., Dependabot, Snyk) is not configured in the GitHub Enterprise environment. No SLAs exist for vulnerability remediation.

**Gap:** Complete absence of vulnerability identification and management. AcmeCorp has no visibility into the vulnerabilities present in its infrastructure, applications, or dependencies.

**Risk Narrative:** Without vulnerability scanning, AcmeCorp has no mechanism to identify known-exploitable weaknesses before attackers do. Given the organization's fintech profile and the value of the data it handles, AcmeCorp represents an attractive target. Known CVEs in unpatched dependencies or infrastructure components are a leading cause of successful attacks against organizations of this profile.

**Remediation:** Enable AWS Inspector for infrastructure vulnerability scanning. Integrate Snyk or GitHub Dependabot for dependency vulnerability management. Commission an annual external penetration test. Define vulnerability remediation SLAs: Critical (24 hours), High (7 days), Medium (30 days), Low (90 days).

---

## 5. Consolidated Risk Summary

| # | Control ID | Control Name | Status | Risk |
|---|-----------|--------------|--------|------|
| 1 | AC-1 | Access Control Policy | Not Implemented | 🔴 High |
| 2 | AC-2 | Account Management | Partial | 🔴 High |
| 3 | AC-6 | Least Privilege | Not Implemented | 🔴 High |
| 4 | AC-17 | Remote Access | Partial | 🟡 Medium |
| 5 | AU-2 | Event Logging | Partial | 🔴 High |
| 6 | AU-9 | Protection of Audit Information | Not Implemented | 🟡 Medium |
| 7 | AU-12 | Audit Record Generation | Partial | 🟡 Medium |
| 8 | CM-2 | Baseline Configuration | Not Implemented | 🔴 High |
| 9 | CM-6 | Configuration Settings | Not Implemented | 🟡 Medium |
| 10 | CM-8 | System Component Inventory | Partial | 🟡 Medium |
| 11 | IA-2 | Identification and Authentication (MFA) | Partial | 🔴 High |
| 12 | IA-5 | Authenticator Management | Not Implemented | 🔴 High |
| 13 | IA-8 | Auth — Non-Organizational Users | Partial | 🟡 Medium |
| 14 | IR-1 | Incident Response Policy | Not Implemented | 🔴 High |
| 15 | IR-2 | Incident Response Training | Not Implemented | 🟡 Medium |
| 16 | IR-4 | Incident Handling | Not Implemented | 🔴 High |
| 17 | IR-6 | Incident Reporting | Not Implemented | 🟡 Medium |
| 18 | RA-2 | Security Categorization | Not Implemented | 🔴 High |
| 19 | RA-3 | Risk Assessment | Not Implemented | 🔴 High |
| 20 | RA-5 | Vulnerability Monitoring and Scanning | Not Implemented | 🔴 High |

**Summary:** 10 High-risk gaps, 7 Medium-risk gaps, 0 Low-risk gaps, 3 additional findings embedded within control families.

---

## 6. Strategic Recommendations

Based on the totality of findings, the following strategic recommendations are provided for AcmeCorp's leadership:

### 6.1 Establish Security Leadership and Governance Infrastructure

AcmeCorp's security gaps are largely attributable to the absence of a dedicated security function. The single highest-leverage investment the organization can make is hiring or appointing a qualified security leader (CISO or Director of Security). No security program can be sustained without clear ownership and accountability. This individual should be empowered to report directly to the CEO or Board and should be given budget authority over the security program.

### 6.2 Address Critical Authentication Gaps Immediately

Enforcing MFA and rotating all exposed credentials are zero-cost or near-zero-cost actions that eliminate the most commonly exploited attack vectors. These actions should be treated as emergency response items — not roadmap items.

### 6.3 Build the Risk Foundation Before Building Controls

Many organizations make the mistake of implementing point-solution security controls without a governing risk framework. AcmeCorp should prioritize completing security categorization (RA-2) and an enterprise risk assessment (RA-3) before making significant tool investments, ensuring that subsequent control implementation is risk-driven and defensible.

### 6.4 Align the Roadmap to SOC 2 Readiness

AcmeCorp's target of SOC 2 Type II certification provides a useful organizing framework. The SOC 2 Trust Services Criteria map closely to the NIST controls assessed in this report. Remediating the gaps identified here — particularly in access control, logging, and incident response — will substantially advance SOC 2 readiness.

---

## 7. Conclusion

AcmeCorp presents a security posture consistent with an organization that has prioritized growth over governance. This is a common pattern in early-stage fintech companies and does not reflect a failure of intent — it reflects the reality of competing organizational priorities during a high-growth phase.

However, AcmeCorp is no longer at a stage where informal, ad hoc security practices are appropriate or sustainable. The organization handles financial data for hundreds of clients, employs 500 people, and is preparing for the formal scrutiny of a SOC 2 audit. The findings in this report represent real, current risk — not theoretical concerns.

The good news is that the remediation path is well-understood. The controls identified in this report have clear implementation patterns, strong tooling ecosystems, and established best practices. With committed leadership, appropriate resourcing, and a disciplined remediation roadmap, AcmeCorp can achieve a substantially improved security posture within 12 months and be positioned for successful SOC 2 certification within 18 months.

This report should be treated as a living document. As remediations are implemented, control statuses should be updated and this assessment should be formally re-evaluated on a semi-annual basis or following any significant change to AcmeCorp's environment, organizational structure, or threat landscape.

---

## Appendix A — Controls Reference Map

| NIST 800-53 Rev 5 | ISO 27001:2022 | SOC 2 TSC | NYDFS 23 NYCRR 500 |
|-------------------|---------------|-----------|-------------------|
| AC-1 | A.5.15 | CC6.1 | 500.07 |
| AC-2 | A.5.16 | CC6.2 | 500.07 |
| AC-6 | A.5.15 | CC6.3 | 500.07 |
| AC-17 | A.8.20 | CC6.6 | 500.12 |
| AU-2 | A.8.15 | CC7.2 | 500.06 |
| AU-9 | A.8.15 | CC7.2 | 500.06 |
| AU-12 | A.8.15 | CC7.2 | 500.06 |
| CM-2 | A.8.9 | CC7.1 | 500.13 |
| CM-6 | A.8.9 | CC7.1 | 500.13 |
| CM-8 | A.8.8 | CC6.1 | 500.13 |
| IA-2 | A.8.5 | CC6.1 | 500.12 |
| IA-5 | A.5.17 | CC6.1 | 500.12 |
| IA-8 | A.8.5 | CC6.1 | 500.07 |
| IR-1 | A.5.26 | CC7.3 | 500.16 |
| IR-2 | A.6.3 | CC9.2 | 500.14 |
| IR-4 | A.5.26 | CC7.4 | 500.16 |
| IR-6 | A.5.26 | CC7.3 | 500.17 |
| RA-2 | A.5.12 | CC3.1 | 500.09 |
| RA-3 | A.6.1 | CC3.2 | 500.09 |
| RA-5 | A.8.8 | CC7.1 | 500.05 |

---

## Appendix B — Risk Rating Methodology

Risk ratings in this report are assigned using a qualitative risk matrix that considers two factors:

**Likelihood** — The probability that the gap would be exploited or lead to an adverse event given AcmeCorp's current environment, threat profile, and control posture.

| Likelihood Level | Definition |
|-----------------|------------|
| High | Exploitation is probable given current controls and threat landscape |
| Medium | Exploitation is possible but requires specific conditions or effort |
| Low | Exploitation is unlikely given other compensating controls |

**Impact** — The potential consequence to AcmeCorp if the gap were exploited, considering confidentiality, integrity, availability, regulatory, and reputational dimensions.

| Impact Level | Definition |
|-------------|------------|
| High | Significant data breach, regulatory penalty, operational disruption, or reputational damage likely |
| Medium | Moderate operational impact; limited data exposure; manageable regulatory exposure |
| Low | Minimal operational impact; unlikely to result in data loss or regulatory action |

**Resulting Risk Rating:**

| | **High Impact** | **Medium Impact** | **Low Impact** |
|-|----------------|------------------|----------------|
| **High Likelihood** | 🔴 High | 🔴 High | 🟡 Medium |
| **Medium Likelihood** | 🔴 High | 🟡 Medium | 🟢 Low |
| **Low Likelihood** | 🟡 Medium | 🟢 Low | 🟢 Low |

---

*This report was prepared based on information available as of the assessment date. The findings reflect a point-in-time evaluation and do not guarantee the absence of additional security gaps beyond the scope reviewed. AcmeCorp is encouraged to treat this assessment as a baseline and to conduct ongoing, continuous security monitoring and periodic reassessment.*

*For questions regarding this report, contact the advisory practice lead.*

---

**Document Control**

| Version | Date | Author | Change Summary |
|---------|------|--------|----------------|
| 0.1 | March 2026 | Advisory Practice | Initial draft |
| 0.9 | April 2026 | Advisory Practice | Internal review revisions |
| 1.0 | April 2026 | Advisory Practice | Final release |
