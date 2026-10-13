# STRIDE Threat Register – Solaris Care Connect 360

## Overview

This threat register applies the **STRIDE threat-modelling framework** across the Solaris Care Connect 360 healthcare platform.

Solaris Care Connect 360 manages approximately **50,000 active patient records** and processes sensitive information including Protected Health Information (PHI), Personally Identifiable Information (PII), prescriptions, laboratory results and authentication data.

STRIDE is used to identify threats across six categories:

- **S – Spoofing:** Impersonating a legitimate user or system
- **T – Tampering:** Unauthorised modification of data or systems
- **R – Repudiation:** Denying an action without sufficient evidence to prove otherwise
- **I – Information Disclosure:** Exposure of information to unauthorised parties
- **D – Denial of Service:** Preventing legitimate access to systems or services
- **E – Elevation of Privilege:** Obtaining permissions beyond those authorised

The severity values below represent an initial threat prioritisation. Numerical Likelihood × Impact scoring is performed separately in the platform risk register.

---

# Spoofing Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **S1** | Credential Phishing | Authentication Service | An attacker obtains valid user or privileged credentials through phishing and impersonates the legitimate account holder | Authentication | **High** |
| **S2** | Fake Doctor Accounts | Admin Dashboard | An attacker creates or causes the creation of an unauthorised healthcare-provider account | Authentication / Authorisation | **Critical** |
| **S3** | Stolen or Forged Access Token | API Gateway | An attacker submits a stolen or manipulated access token to impersonate an authenticated user | Authentication | **High** |
| **S4** | MITM on Insurance Integration | Insurance Provider API | An attacker impersonates or intercepts an external insurance service connection | Authentication / Confidentiality | **Medium** |
| **S5** | Spoofed Laboratory Results | Lab Results Feed | A malicious or compromised source submits fraudulent laboratory information as if it came from a trusted laboratory | Authentication / Integrity | **Critical** |

## Key Concern

Spoofing threats are particularly important because Solaris relies on trusted identities to determine access to sensitive healthcare information.

Strong authentication must therefore apply to both **human users** and **external system integrations**.

---

# Tampering Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **T1** | Patient Record Modification | Patient Records Service | An attacker makes unauthorised changes to medical or demographic information | Integrity | **Critical** |
| **T2** | Prescription Alteration | Prescription Service | Medication, dosage or prescription details are maliciously modified | Integrity / Patient Safety | **Critical** |
| **T3** | Audit Log Tampering | Audit / Logging Systems | Security or access records are modified or deleted to conceal malicious activity | Integrity / Accountability | **High** |
| **T4** | Insurance Claim Manipulation | Insurance Integration | Claim information is changed before or during processing | Integrity | **High** |
| **T5** | Vitals Data Tampering | Vitals Monitoring | Real-time patient vital-sign information is maliciously modified | Integrity / Patient Safety | **Critical** |

## Key Concern

Integrity is especially important in healthcare systems because modification of clinical information may affect subsequent medical or operational decisions.

---

# Repudiation Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **R1** | Prescription Approval Denial | Prescription Service | A doctor disputes having authorised or approved a prescription | Accountability | **High** |
| **R2** | Patient Access Denial | Patient Records Service | A patient disputes having accessed or performed an action against their record | Accountability | **Medium** |
| **R3** | Permission Change Denial | Admin Dashboard | An administrator denies changing user roles or permissions | Accountability | **High** |
| **R4** | Insurance Claim Receipt Denial | Insurance Provider API | An external provider disputes receiving or processing a submitted claim | Accountability | **Medium** |
| **R5** | Data Export Audit Gap | Patient Records / Data Export | Sensitive information is exported without sufficient evidence showing who performed the action | Accountability / Confidentiality | **High** |

## Key Concern

Strong audit evidence is required so sensitive actions can be reliably associated with the identity, session and system that performed them.

Audit records should therefore be centralised and protected against unauthorised alteration.

---

# Information Disclosure Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **I1** | SQL Injection Data Breach | Patient Records Service | SQL injection allows an attacker to retrieve sensitive patient information from the database | Confidentiality | **Critical** |
| **I2** | Excessive Data Return | API Gateway / Backend Services | An API response returns more sensitive information than the requester requires | Confidentiality | **High** |
| **I3** | Verbose Error Messages | Application Services | Error responses reveal internal paths, technologies, queries or configuration information | Confidentiality | **Medium** |
| **I4** | Unencrypted Data in Transit | Network Communications | Sensitive healthcare information is transmitted without adequate transport encryption | Confidentiality | **Critical** |
| **I5** | Exposed Backup Data | Data Storage | Backup files containing PHI or PII are accessible without sufficient authentication or authorisation | Confidentiality | **Critical** |
| **I6** | PHI Leakage in Logs | Application / Logging Services | Sensitive patient information is accidentally written into application or diagnostic logs | Confidentiality | **High** |

## Key Concern

Solaris processes large volumes of PHI and PII. Information-disclosure threats therefore have the potential to affect many patients simultaneously.

Sensitive information should be protected throughout its lifecycle:

**collection → processing → transmission → storage → logging → backup → deletion**

---

# Denial of Service Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **D1** | DDoS Against API Gateway | API Gateway | Large volumes of malicious traffic exhaust externally available application resources | Availability | **High** |
| **D2** | Database Resource Exhaustion | PostgreSQL | Expensive or repeated queries consume database resources and degrade availability | Availability | **High** |
| **D3** | Storage Exhaustion | Document Storage | Unrestricted or malicious file uploads consume available storage capacity | Availability | **Medium** |
| **D4** | Account Lockout Attack | Authentication Service | Repeated authentication attempts deliberately lock legitimate users out of their accounts | Availability | **Medium** |
| **D5** | Prescription Processing Exhaustion | Prescription Service | Excessive requests consume processing resources and prevent legitimate prescription activity | Availability | **High** |

## Key Concern

Availability has increased importance in a healthcare environment because disruption may prevent patients and healthcare professionals from accessing time-sensitive information or services.

---

# Elevation of Privilege Threats

| ID | Threat | Component | Description | Primary Security Concern | Severity |
|---|---|---|---|---|---|
| **E1** | Patient-to-Doctor Privilege Escalation | Authentication / Authorisation Service | A patient obtains permissions intended for a healthcare professional | Authorisation | **Critical** |
| **E2** | Doctor-to-Administrator Escalation | Authentication / Authorisation Service | A doctor obtains administrative privileges beyond their authorised role | Authorisation | **Critical** |
| **E3** | SQL Injection to Database Administration | PostgreSQL / Patient Records Service | Successful SQL injection provides elevated database privileges or administrative database access | Authorisation / Confidentiality | **Critical** |
| **E4** | Container Escape | Infrastructure | An attacker escapes an application container and gains access to the underlying host environment | Authorisation / Infrastructure | **Critical** |
| **E5** | IDOR / Cross-Patient Record Access | Patient Records Service | Weak object-level authorisation allows a user to access another patient's records | Authorisation / Confidentiality | **High** |

## Key Concern

Elevation-of-privilege threats can turn an initially limited compromise into a much larger incident.

The platform should enforce authorisation at each layer rather than assuming that successful authentication automatically grants access to requested resources.

> **Note:** E4 is retained from the project threat catalogue. Its applicability should be validated against the final deployment architecture if containerised infrastructure is used.

---

# STRIDE Summary

| STRIDE Category | Total Threats | Critical | High | Medium | Low |
|---|---:|---:|---:|---:|---:|
| **Spoofing** | 5 | 2 | 2 | 1 | 0 |
| **Tampering** | 5 | 3 | 2 | 0 | 0 |
| **Repudiation** | 5 | 0 | 3 | 2 | 0 |
| **Information Disclosure** | 6 | 3 | 2 | 1 | 0 |
| **Denial of Service** | 5 | 0 | 3 | 2 | 0 |
| **Elevation of Privilege** | 5 | 4 | 1 | 0 | 0 |
| **Total** | **31** | **12** | **13** | **6** | **0** |

---

# Highest-Priority Threat Areas

The initial STRIDE assessment identifies several particularly important areas:

1. **Patient-data confidentiality** – SQL injection, exposed backups and insecure transmission could expose PHI and PII.
2. **Healthcare-data integrity** – manipulation of patient records, prescriptions, laboratory information or vital signs could have serious consequences.
3. **Identity and privilege management** – stolen credentials or weak authorisation could allow attackers to impersonate trusted users or obtain elevated access.
4. **External trust relationships** – insurance, pharmacy and laboratory integrations create additional attack surfaces.
5. **Auditability** – insufficient or modifiable logs could prevent effective investigation of malicious activity.

---

# Primary Control Themes

## Authentication and Identity

- Multi-Factor Authentication (MFA)
- secure session management
- strong token validation
- phishing awareness
- external API authentication

## Authorisation

- Role-Based Access Control (RBAC)
- least privilege
- Privileged Access Management (PAM)
- object-level authorisation
- regular access reviews

## Application Security

- input validation
- parameterised SQL queries
- secure error handling
- Web Application Firewall (WAF)
- vulnerability and patch management

## Data Protection

- encryption in transit
- encryption at rest
- secure backup storage
- data minimisation
- Data Loss Prevention (DLP)

## Monitoring and Detection

- centralised security logging
- SIEM monitoring
- Database Activity Monitoring
- privileged-access monitoring
- anomaly detection
- tamper-resistant audit records

## Availability and Recovery

- rate limiting
- DDoS protection
- resource monitoring
- resilient architecture
- incident response
- backup and recovery procedures

---

# Relationship to Risk Assessment

STRIDE identifies **what can go wrong**.

The next stage of the threat model determines **which threats require the greatest remediation priority** by assigning numerical:

- **Likelihood: 1–5**
- **Impact: 1–5**
- **Risk Score = Likelihood × Impact**

The highest-priority STRIDE threats are carried forward into the formal risk register.

---

## Related Documentation

- [System Architecture](../diagrams/architecture.md)
- [Data Flow Diagrams](../diagrams/dfd-level1.md)
- [MITRE ATT&CK Mapping](./mitre-mapping.md)
- [Risk Register](./risk-register.md)
- [Scenario 1 – AI-Assisted Phishing](../Scenario%201/README.md)
