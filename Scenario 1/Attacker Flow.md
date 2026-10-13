# Scenario 1 – AI-Assisted Phishing and Credential Compromise

## Attack Overview

This scenario models an attacker using AI-assisted social engineering to create a convincing phishing campaign targeting a privileged administrator of the Solaris Care Connect 360 healthcare platform.

The objective is to obtain valid administrator credentials, authenticate to the platform as a legitimate user, and gain unauthorised access to sensitive patient information.

## Attacker Flow

```mermaid
sequenceDiagram
    autonumber

    actor Attacker
    actor Admin as Privileged Administrator
    participant Portal as Solaris Care Connect 360
    participant API as API Gateway
    participant Auth as Authentication Service
    participant Records as Patient Records Service
    participant DB as Patient Records Database

    Attacker->>Attacker: Research privileged administrator
    Attacker->>Attacker: Generate convincing AI-assisted phishing content

    Attacker->>Admin: Send targeted phishing email
    Admin->>Attacker: Click malicious link and visit fake login page
    Admin->>Attacker: Submit username and password

    Note over Attacker,Admin: Administrator credentials compromised

    Attacker->>Portal: Attempt login using stolen credentials
    Portal->>API: Forward authentication request
    API->>Auth: Validate supplied credentials
    Auth-->>API: Authentication successful
    API-->>Portal: Issue valid session / access token

    Note over Attacker,Auth: Attacker is authenticated as a privileged user

    Attacker->>Portal: Request patient records
    Portal->>API: Submit authenticated request
    API->>Records: Request sensitive patient information
    Records->>DB: Query patient records
    DB-->>Records: Return PHI / PII
    Records-->>API: Return patient data
    API-->>Portal: Return patient records
    Portal-->>Attacker: Sensitive healthcare data exposed

    Note over Attacker,DB: Impact: unauthorised disclosure of patient information
```

## Security Impact

A successful attack could result in:

- Compromise of a privileged administrator account.
- Unauthorised access to patient records.
- Exposure of personally identifiable information (PII) and protected health information (PHI).
- Abuse of a trusted authenticated session.
- Regulatory and reputational consequences.
- Potential follow-on attacks using the compromised account.

## Key Security Concerns

The scenario highlights the importance of:

- Multi-factor authentication (MFA).
- Phishing-resistant authentication.
- Role-based access control (RBAC).
- Least privilege.
- Login anomaly detection.
- Centralised audit logging.
- Security awareness training.
- Session monitoring and revocation.
