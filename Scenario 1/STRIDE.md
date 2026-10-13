# Scenario 1 – STRIDE Threat Analysis

## Scenario

This STRIDE assessment analyses the **AI-assisted phishing and privileged credential compromise** scenario against Solaris Care Connect 360.

The attacker attempts to obtain valid administrator credentials through phishing, authenticate through legitimate application workflows and use the compromised identity to access sensitive patient information.

The analysis considers threats across the following components:

- Privileged Administrator
- Admin Dashboard
- API Gateway
- Authentication Service
- Patient Records Service
- PostgreSQL Patient Database

---

## STRIDE Assessment

| STRIDE Category | Threat Scenario | Affected Components | Security Impact | Related Risk | Required Controls |
|---|---|---|---|---|---|
| **Spoofing** | An attacker uses stolen administrator credentials to impersonate a legitimate privileged user | Admin Dashboard, Authentication Service | Unauthorised authenticated access | S1-R1, S1-R2 | Phishing-resistant MFA, conditional access, login anomaly detection |
| **Tampering** | A compromised administrator account modifies patient or clinical records | Patient Records Service, PostgreSQL | Loss of integrity of healthcare information | S1-R3 | RBAC, least privilege, Privileged Access Management, audit logging |
| **Repudiation** | The attacker performs actions through a legitimate account and malicious activity cannot be reliably attributed | API Gateway, Patient Records Service, audit records | Reduced accountability and incident-investigation capability | S1-R4 | Centralised logging, tamper-resistant audit trails, SIEM correlation |
| **Information Disclosure** | The compromised account accesses or exports PHI and PII without authorisation | Patient Records Service, PostgreSQL | Exposure of sensitive patient information | S1-R3, S1-R5 | RBAC, patient-record access monitoring, encryption, DLP |
| **Denial of Service** | A compromised privileged account abuses administrative functionality or application requests to disrupt service availability | API Gateway, backend services | Loss or degradation of healthcare service availability | Secondary | Rate limiting, availability monitoring, restricted administrative actions |
| **Elevation of Privilege** | The attacker uses the compromised account to obtain permissions beyond those required for the administrator's normal role | Authentication Service, Admin Dashboard, backend services | Increased access and larger compromise scope | S1-R2, S1-R3 | Least privilege, PAM, step-up authentication, separation of duties |

---

## STRIDE Analysis by Attack Stage

```mermaid
flowchart LR

    A["AI-Assisted<br/>Phishing"]
    B["Administrator<br/>Credentials Stolen"]
    C["Attacker Authenticates<br/>as Administrator"]
    D["Privileged Access<br/>to Solaris"]
    E["Patient Records<br/>Accessed"]
    F["Potential Data<br/>Modification / Disclosure"]

    A --> B --> C --> D --> E --> F

    S["SPOOFING<br/>Impersonate administrator"] -.-> C
    T["TAMPERING<br/>Modify patient records"] -.-> F
    R["REPUDIATION<br/>Hide or deny malicious actions"] -.-> D
    I["INFORMATION DISCLOSURE<br/>Expose PHI / PII"] -.-> E
    DOS["DENIAL OF SERVICE<br/>Abuse privileged functions"] -.-> D
    EOP["ELEVATION OF PRIVILEGE<br/>Gain excessive permissions"] -.-> D
```

---

# S – Spoofing

## Threat

The attacker obtains the administrator's username and password through an AI-assisted phishing campaign and then presents those credentials to Solaris Care Connect 360.

Because the credentials belong to a legitimate user, the Authentication Service may initially treat the attacker as the real administrator.

### Affected Components

- Privileged Administrator
- Admin Dashboard
- API Gateway
- Authentication Service

### Security Impact

Successful spoofing allows an attacker to operate using a trusted privileged identity.

### Control Gap

Password-only authentication would provide insufficient protection against credential phishing.

### Required Controls

- phishing-resistant MFA
- conditional access
- login anomaly detection
- secure email filtering
- security awareness training

### Related Risks

- **S1-R1 – Administrator credentials compromised**
- **S1-R2 – Stolen credentials used for authentication**

---

# T – Tampering

## Threat

After authentication, a compromised privileged account may attempt to modify sensitive information such as:

- patient demographic information
- medical records
- clinical information
- account settings
- prescription-related data
- other sensitive application data

### Affected Components

- Patient Records Service
- PostgreSQL Patient Database
- administrative functionality

### Security Impact

Unauthorised changes could compromise the **integrity of healthcare information** and potentially affect subsequent clinical or operational decisions.

### Control Gap

Excessive administrator permissions would increase the attacker's ability to modify sensitive records.

### Required Controls

- RBAC
- least privilege
- Privileged Access Management
- strong server-side authorisation
- audit logging for record modifications

### Related Risk

- **S1-R3 – Excessive privileges expose sensitive patient information**

---

# R – Repudiation

## Threat

Because the attacker is using a legitimate administrator account, malicious actions may initially appear to have been performed by the real administrator.

Without sufficient audit evidence, it may be difficult to determine:

- who performed an action
- when the action occurred
- which records were accessed
- which records were changed
- where the request originated
- which authenticated session performed the action

### Affected Components

- API Gateway
- Authentication Service
- Patient Records Service
- security logs
- audit records

### Security Impact

Weak accountability makes incident investigation, attribution and regulatory reporting more difficult.

### Control Gap

Security-relevant events must be logged centrally and protected against unauthorised alteration.

### Required Controls

- centralised audit logging
- SIEM monitoring
- time-synchronised logs
- tamper-resistant audit records
- correlation of authentication and patient-access events

### Related Risk

- **S1-R4 – Malicious authenticated activity is not detected quickly**

---

# I – Information Disclosure

## Threat

The attacker uses the compromised administrator identity to access patient information that they are not authorised to view.

Sensitive information could include:

- Protected Health Information (PHI)
- Personally Identifiable Information (PII)
- medical histories
- prescriptions
- laboratory results
- patient demographic information
- insurance information

### Affected Components

- Patient Records Service
- PostgreSQL Patient Database
- Document Storage
- Admin Dashboard

### Security Impact

Unauthorised disclosure could result in significant patient privacy, regulatory and reputational consequences.

### Control Gap

A privileged identity should not automatically provide unrestricted access to all patient information.

### Required Controls

- RBAC
- least privilege
- encryption in transit and at rest
- patient-record access monitoring
- Data Loss Prevention
- alerts for unusual or bulk record access

### Related Risks

- **S1-R3 – Excessive privileged access**
- **S1-R5 – PHI / PII accessed or potentially exfiltrated**

---

# D – Denial of Service

## Threat

Denial of service is not the primary objective of this phishing scenario.

However, a compromised privileged account could potentially abuse administrative functionality or generate disruptive application activity.

Examples include:

- disabling legitimate user accounts
- abusing administrative operations
- generating excessive application requests
- interfering with application availability
- disrupting access to healthcare functionality

### Affected Components

- API Gateway
- Authentication Service
- backend services

### Security Impact

Disruption could reduce availability of a healthcare platform relied upon by patients and clinical users.

### Control Gap

Privileged access should not allow a single compromised identity to cause widespread service disruption without safeguards.

### Required Controls

- rate limiting
- availability monitoring
- restricted administrative actions
- approval for high-impact operations
- incident-response procedures

### Scenario Relevance

This is considered a **secondary threat** within Scenario 1 rather than a primary attack objective.

---

# E – Elevation of Privilege

## Threat

After compromising an administrator account, the attacker may attempt to obtain permissions beyond those normally assigned to that identity.

Examples include:

- assigning additional roles
- creating new privileged accounts
- changing access-control settings
- gaining access to additional backend services
- granting permissions to attacker-controlled accounts

### Affected Components

- Admin Dashboard
- Authentication Service
- API Gateway
- backend services

### Security Impact

Successful privilege escalation increases the attacker's access and the potential blast radius of the incident.

### Control Gap

Administrative access should be tightly scoped and sensitive privilege changes should require additional controls.

### Required Controls

- least privilege
- Privileged Access Management
- separation of duties
- step-up authentication
- monitoring of role and permission changes
- alerts for creation of privileged identities

### Related Risks

- **S1-R2 – Stolen credentials used for privileged authentication**
- **S1-R3 – Excessive privileges increase compromise impact**

---

# STRIDE Control Gap Summary

| STRIDE Category | Primary Control Gap | Priority |
|---|---|---|
| **Spoofing** | Stolen credentials could be accepted as legitimate | **Critical** |
| **Tampering** | Compromised privileged identities may modify sensitive records | **High** |
| **Repudiation** | Malicious activity may be difficult to attribute without strong audit evidence | **High** |
| **Information Disclosure** | Privileged access may expose large volumes of PHI/PII | **Critical** |
| **Denial of Service** | Privileged functionality could potentially be abused to affect availability | **Medium** |
| **Elevation of Privilege** | Compromised accounts may attempt to obtain additional permissions | **High** |

---

# Control Mapping

| Threat Category | Primary Preventive Controls | Primary Detective Controls |
|---|---|---|
| **Spoofing** | MFA, conditional access, secure email filtering | Login anomaly detection |
| **Tampering** | RBAC, least privilege, PAM | Audit logging, change monitoring |
| **Repudiation** | Strong authentication, protected audit architecture | SIEM, centralised audit logging |
| **Information Disclosure** | RBAC, encryption, least privilege | Patient-access monitoring, DLP |
| **Denial of Service** | Rate limiting, restricted administrative operations | Availability monitoring |
| **Elevation of Privilege** | PAM, separation of duties, step-up authentication | Role-change and privileged-account monitoring |

---

# Relationship to Scenario Risks

The STRIDE analysis maps directly to the scenario-specific risk register:

```text
Spoofing
   ↓
S1-R1 / S1-R2
Credential compromise and valid-account abuse
```

```text
Tampering / Elevation of Privilege
   ↓
S1-R3
Excessive privileged access and potential record modification
```

```text
Repudiation
   ↓
S1-R4
Malicious authenticated activity may remain undetected
```

```text
Information Disclosure
   ↓
S1-R5
PHI / PII accessed or potentially exfiltrated
```

This creates traceability between:

**attack behaviour → STRIDE category → scenario risk → required control**

---

# STRIDE Conclusion

The most significant STRIDE concerns for Scenario 1 are **Spoofing** and **Information Disclosure**.

The attack begins with identity compromise: the attacker attempts to impersonate a legitimate privileged administrator using stolen credentials.

If successful, the trusted identity can then be used to access sensitive healthcare information through legitimate application functionality.

Defence therefore requires multiple layers:

> **prevent credential abuse → restrict privileged access → detect abnormal behaviour → protect patient information → maintain reliable audit evidence**

---

## Related Documentation

- [Scenario 1 Overview](./README.md)
- [Attacker Flow](./Attacker%20Flow.md)
- [Risks Summary](./Risks%20Summary.md)
- [Required Security Controls](./Controls%20Required.md)
- [MITRE ATT&CK Sequence Summary](./MITRE%20ATT%26CK%20Sequence%20Summary.md)
