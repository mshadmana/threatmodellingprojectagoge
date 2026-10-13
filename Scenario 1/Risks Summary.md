# Scenario 1 – Risk Summary

## Scenario

**AI-Assisted Phishing and Credential Compromise**

This scenario considers an attacker using AI-assisted social engineering to target a privileged administrator of Solaris Care Connect 360.

If the phishing attempt succeeds, the attacker may obtain valid administrator credentials, authenticate through legitimate application workflows and gain access to sensitive patient information.

---

## Risk Assessment

| Risk ID | Risk Description | Primary Asset | Likelihood | Impact | Inherent Risk | Recommended Controls |
|---|---|---|---|---|---|---|
| R1 | AI-assisted phishing successfully compromises a privileged administrator's credentials | Administrator identity | High | High | **High** | Phishing-resistant MFA, secure email filtering, security awareness training |
| R2 | Stolen credentials are used to authenticate to Solaris Care Connect 360 as a legitimate privileged user | Authentication service / privileged account | High | High | **High** | MFA, conditional access, login anomaly detection, credential rotation |
| R3 | A compromised administrator account provides excessive access to sensitive patient information | Patient PHI / PII | Medium | High | **High** | RBAC, least privilege, privileged access management, step-up authentication |
| R4 | Malicious activity performed through a valid authenticated session is not detected quickly enough | Application sessions / audit records | Medium | High | **High** | Centralised logging, SIEM alerting, behavioural analytics, session revocation |
| R5 | Sensitive patient information is accessed or exfiltrated through legitimate application functionality | Patient records | Medium | High | **High** | Data access monitoring, rate limiting, DLP controls, audit logging and alerting |

---

## Risk Rating Method

Risks are assessed qualitatively using **Likelihood** and **Impact**.

### Likelihood

- **High** – The attack is credible and could reasonably occur without strong preventative controls.
- **Medium** – The attack is feasible but requires additional conditions or access.
- **Low** – The attack would require uncommon circumstances or several security failures.

### Impact

- **High** – Could result in significant compromise of patient information, privileged access or healthcare operations.
- **Medium** – Could cause limited data exposure or operational disruption.
- **Low** – Limited effect on confidentiality, integrity or availability.

### Risk Matrix

| Likelihood ↓ / Impact → | Low | Medium | High |
|---|---|---|---|
| **High** | Medium | High | **High** |
| **Medium** | Low | Medium | **High** |
| **Low** | Low | Low | Medium |

---

## Key Risk

The primary risk in this scenario is the compromise of a **privileged administrator identity**.

Because the attacker uses valid credentials after the initial phishing attack, subsequent activity may appear legitimate to the platform. This makes identity controls, least privilege, session monitoring and audit logging particularly important.

```mermaid
flowchart LR
    A["AI-Assisted<br/>Phishing"] --> B["Administrator<br/>Credential Compromise"]
    B --> C["Valid Account<br/>Authentication"]
    C --> D["Privileged<br/>Application Access"]
    D --> E["Patient Records<br/>Access"]
    E --> F["PHI / PII<br/>Disclosure"]

    B -. Prevent .-> G["Phishing-Resistant MFA"]
    C -. Detect .-> H["Login Anomaly Detection"]
    D -. Limit .-> I["RBAC / Least Privilege"]
    E -. Monitor .-> J["Audit Logging / SIEM"]
    F -. Reduce .-> K["DLP / Data Access Controls"]
```

## Risk Treatment Priority

The recommended treatment priority is:

1. Prevent successful use of stolen credentials through phishing-resistant MFA.
2. Restrict privileged accounts using RBAC and least privilege.
3. Detect anomalous authentication and privileged-user behaviour.
4. Monitor access to sensitive patient records.
5. Rapidly revoke compromised sessions and credentials following detection.
