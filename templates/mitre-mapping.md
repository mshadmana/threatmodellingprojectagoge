# MITRE ATT&CK Mapping – Solaris Care Connect 360

## Overview

This document maps key threats identified in the Solaris Care Connect 360 STRIDE threat register to relevant **MITRE ATT&CK tactics and techniques**.

MITRE ATT&CK provides a behavioural view of how adversaries may carry out attacks against the platform.

The mapping complements STRIDE:

- **STRIDE** identifies what can go wrong.
- **MITRE ATT&CK** describes how an attacker may perform the attack.
- **Risk scoring** determines which threats require the highest remediation priority.

---

# Threat-to-Technique Mapping

| STRIDE ID | Threat | MITRE ATT&CK Technique | Technique ID | Tactic | Primary Mitigation |
|---|---|---|---|---|---|
| **S1** | Credential Phishing | Phishing: Spearphishing Link | **T1566.002** | Initial Access | MFA, email filtering, security awareness training |
| **S2** | Fake Doctor Accounts | Create Account | **T1136** | Persistence | Account approval, account monitoring, MFA |
| **S3** | Stolen Access Token | Steal Application Access Token | **T1528** | Credential Access | Token validation, secure token storage, short expiry |
| **S4** | MITM on Insurance Integration | Adversary-in-the-Middle | **T1557** | Credential Access / Collection | Mutual TLS, certificate validation, encrypted communications |
| **S5** | Spoofed Laboratory Results | External Remote Services / Trusted Integration Abuse | **T1133** | Persistence / Initial Access | Strong API authentication, mutual TLS, message validation |
| **T3** | Audit Log Tampering | Indicator Removal: Clear Windows Event Logs / File Deletion | **T1070** | Defense Evasion | Centralised tamper-resistant logging |
| **I1** | SQL Injection Data Breach | Exploit Public-Facing Application | **T1190** | Initial Access | Input validation, prepared statements, WAF |
| **I5** | Exposed Backup Data | Unsecured Credentials / Data from Information Repositories | **T1213** | Collection | Secure backup permissions and encryption |
| **E1** | Patient-to-Doctor Privilege Escalation | Valid Accounts | **T1078** | Privilege Escalation | RBAC, least privilege, access reviews |
| **E2** | Doctor-to-Administrator Escalation | Account Manipulation | **T1098** | Persistence / Privilege Escalation | PAM, role-change monitoring |
| **E3** | SQL Injection to DBA | Exploit Public-Facing Application | **T1190** | Initial Access | Database hardening, least privilege, WAF |
| **E4** | Container Escape | Escape to Host | **T1611** | Privilege Escalation | Container hardening, runtime controls |
| **E5** | IDOR / Cross-Patient Access | Valid Accounts | **T1078** | Defense Evasion / Privilege Escalation | Object-level authorisation and RBAC |

---

# Key Attack Techniques

## T1566.002 – Phishing: Spearphishing Link

### Application to Solaris

An attacker sends a targeted phishing email containing a malicious link to a patient, doctor or privileged administrator.

The victim may be directed to a fraudulent authentication portal designed to capture valid credentials.

### Associated Threat

**S1 – Credential Phishing**

### Mitigations

- phishing-resistant MFA
- secure email gateway
- URL filtering
- security awareness training
- phishing simulations

---

## T1078 – Valid Accounts

### Application to Solaris

After obtaining legitimate credentials, an attacker may authenticate through normal Solaris Care Connect 360 workflows.

Because the credentials are valid, attacker activity may initially appear legitimate.

### Associated Threats

- **S1 – Credential Phishing**
- **E1 – Patient-to-Doctor Privilege Escalation**
- **E5 – Cross-Patient Record Access**

### Mitigations

- Multi-Factor Authentication
- conditional access
- login anomaly detection
- least privilege
- session monitoring

---

## T1528 – Steal Application Access Token

### Application to Solaris

An attacker who obtains a valid application or session token may impersonate an authenticated user without knowing the user's password.

### Associated Threat

**S3 – Stolen Access Token**

### Mitigations

- secure token storage
- short token lifetime
- token revocation
- secure session management
- TLS
- anomaly detection

---

## T1190 – Exploit Public-Facing Application

### Application to Solaris

Internet-facing application components such as the API Gateway or Patient Records functionality may be targeted through application vulnerabilities.

A successful SQL injection attack could allow access to sensitive patient records.

### Associated Threats

- **I1 – SQL Injection Data Breach**
- **E3 – SQL Injection to DBA**

### Mitigations

- parameterised SQL queries
- input validation
- Web Application Firewall
- vulnerability scanning
- secure coding practices
- patch management
- least-privilege database accounts

---

## T1098 – Account Manipulation

### Application to Solaris

An attacker with sufficient privileges may alter account roles, permissions or authentication settings to maintain or expand access.

### Associated Threat

**E2 – Doctor-to-Administrator Escalation**

### Mitigations

- Role-Based Access Control
- Privileged Access Management
- separation of duties
- monitoring of role changes
- approval workflows

---

## T1136 – Create Account

### Application to Solaris

An attacker with administrative access may create an unauthorised doctor or administrator account to establish persistent access.

### Associated Threat

**S2 – Fake Doctor Accounts**

### Mitigations

- account creation approval
- MFA
- identity verification
- monitoring of new privileged accounts
- regular account reviews

---

## T1611 – Escape to Host

### Application to Solaris

If Solaris backend services are deployed in containers, an attacker who compromises a container may attempt to escape into the underlying host.

### Associated Threat

**E4 – Container Escape**

### Mitigations

- run containers as non-root
- minimise Linux capabilities
- use seccomp/AppArmor profiles
- patch container runtimes
- runtime monitoring
- avoid privileged containers

> The applicability of this technique depends on the final deployment architecture.

---

# Example Healthcare Data-Breach Attack Chain

```mermaid
flowchart LR

    A["T1566.002<br/>Spearphishing Link"]
    B["T1078<br/>Valid Accounts"]
    C["Privileged Application<br/>Access"]
    D["T1213<br/>Data from Information Repositories"]
    E["Sensitive Patient<br/>Data Collected"]
    F["T1567<br/>Exfiltration Over Web Service"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

This represents one possible attack chain in which:

1. a user is targeted through phishing
2. legitimate credentials are compromised
3. the attacker authenticates using a valid account
4. healthcare information is accessed
5. patient data is collected
6. the data is exfiltrated through an external web service

---

# SQL Injection Attack Chain

```mermaid
flowchart LR

    A["Internet-Facing<br/>Application"]
    B["T1190<br/>Exploit Public-Facing Application"]
    C["SQL Injection"]
    D["Database Access"]
    E["Patient PHI / PII<br/>Collected"]
    F["Potential<br/>Exfiltration"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

The SQL injection path demonstrates why controls must exist at multiple layers:

**WAF → input validation → parameterised queries → least-privilege database account → database monitoring**

---

# Recommended Mitigations

| Control | Purpose | Relevant Threats |
|---|---|---|
| **Multi-Factor Authentication** | Reduce effectiveness of stolen credentials | S1, E1 |
| **Security Awareness Training** | Reduce successful phishing | S1 |
| **Web Application Firewall** | Block common malicious application requests | I1, E3 |
| **Prepared Statements** | Prevent SQL injection from altering query structure | I1, E3 |
| **Privileged Access Management** | Restrict and monitor privileged identities | E1, E2 |
| **Least Privilege** | Limit attacker access following compromise | E1, E2, E3 |
| **Network Segmentation** | Restrict lateral movement between trust zones | E3, E4 |
| **Data Loss Prevention** | Detect or restrict unauthorised sensitive-data movement | I1, I5 |
| **Centralised Logging / SIEM** | Detect suspicious authentication and data access | S1, S3, E1 |
| **Patch Management** | Reduce exposure to known application and infrastructure vulnerabilities | I1, E4 |

---

# Detection Opportunities

| ATT&CK Behaviour | Detection Opportunity |
|---|---|
| Phishing delivery | Email gateway and URL inspection |
| Use of stolen credentials | Authentication anomaly detection |
| New privileged account | Identity-management alerts |
| Role or permission changes | Privileged-access monitoring |
| SQL injection attempts | WAF and application logs |
| Abnormal database queries | Database Activity Monitoring |
| Large patient-record access | SIEM / behavioural analytics |
| Large outbound data transfer | DLP / network monitoring |
| Container escape behaviour | Runtime security monitoring |

---

# Relationship to STRIDE

The MITRE ATT&CK mapping builds on threats identified in the STRIDE register.

Examples include:

```text
S1 Credential Phishing
        ↓
T1566.002 Spearphishing Link
        ↓
T1078 Valid Accounts
        ↓
Patient data access
```

and:

```text
I1 SQL Injection
        ↓
T1190 Exploit Public-Facing Application
        ↓
Database compromise
        ↓
PHI / PII disclosure
```

This creates a traceable relationship between:

**system threat → attacker technique → security control**

---

# Key Finding

Two attack paths require particularly strong defensive coverage:

### Identity-Based Attack

**Phishing → stolen credentials → valid account → privileged access → patient-data access**

Primary controls:

- MFA
- security awareness
- RBAC
- PAM
- anomaly detection

### Application-Based Attack

**Internet-facing application → SQL injection → database access → PHI/PII exposure**

Primary controls:

- WAF
- input validation
- parameterised queries
- database least privilege
- database activity monitoring

These attack paths are carried forward into the formal risk assessment and security-control mapping.

---

## Related Documentation

- [STRIDE Threat Register](./stride-threats.md)
- [System Architecture](../HLD.md)
- [Data Flow Diagrams](../Data%20Flow%20Diagram.md)
- [Scenario 1 MITRE ATT&CK Analysis](../Scenario%201/MITRE%20ATT%26CK%20Sequence%20Summary.md)
