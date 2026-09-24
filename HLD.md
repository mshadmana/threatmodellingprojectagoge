# Solaris Care Connect 360 - High-Level Architecture

## Overview

Solaris Care Connect 360 is a healthcare platform that manages approximately 50,000 active patient records. The platform provides patient monitoring, prescription management, secure doctor-patient messaging and integrations with external healthcare providers.

The architecture below identifies the main system components and trust relationships used throughout the threat modelling assessment.

## System Components

### Frontend Applications
- Patient Mobile App
- Doctor Web Portal
- Admin Dashboard

### API Layer
- API Gateway
- Authentication Service

### Backend Services
- Patient Records Service
- Prescription Service
- Messaging Service
- Vitals Monitoring Service

### Data Layer
- PostgreSQL Patient Database
- Document Storage
- Redis Cache

### External Systems
- Insurance Provider APIs
- Pharmacy Network
- Lab Results Feed

## Architecture Diagram

```mermaid
flowchart TB

    subgraph ExternalUsers["External Users"]
        Patient["Patient Mobile App"]
        Doctor["Doctor Web Portal"]
        Admin["Admin Dashboard"]
    end

    subgraph APILayer["API Layer"]
        GW["API Gateway"]
        Auth["Authentication Service"]
    end

    subgraph Backend["Backend Services"]
        PR["Patient Records Service"]
        RX["Prescription Service"]
        MSG["Messaging Service"]
        VITAL["Vitals Monitoring"]
    end

    subgraph DataLayer["Data Layer"]
        DB[("PostgreSQL")]
        S3[("Document Storage")]
        Redis[("Redis Cache")]
    end

    subgraph ExternalSystems["External Systems"]
        INS["Insurance Provider APIs"]
        PHARM["Pharmacy Network"]
        LAB["Lab Results Feed"]
    end

    Patient --> GW
    Doctor --> GW
    Admin --> GW

    GW --> Auth
    GW --> PR
    GW --> RX
    GW --> MSG
    GW --> VITAL

    PR --> DB
    PR --> S3
    RX --> DB
    MSG --> Redis
    VITAL --> DB

    PR --> INS
    RX --> PHARM
    LAB --> VITAL
