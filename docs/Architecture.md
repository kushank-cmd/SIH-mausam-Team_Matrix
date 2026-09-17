# System Architecture — Components & High-Level Flow

## Overview & High-Level Flow

**Request Flow:** Client devices hit the API Gateway, which forwards work to the Application Layer. The Application Layer leans on AI/ML Services and the Data Storage Layer as it processes each request, then hands off to the Integration Layer to sync with the broader health ecosystem. Operations (notifications, monitoring, backup) run alongside every stage, and Security & Compliance underpins the entire stack.

---

## Architecture Flowchart

```text
+---------------------------------------------------------------------------------------------------+
| 1. CLIENT LAYER                                                                                   |
|    +-----------------------------------------------------------------------------------------+    |
|    |                                  FRONTEND INTERFACE                                     |    |
|    |  Patient & Doctor Apps: Web, Mobile, Tablet, Desktop                                    |    |
|    |  Inputs: Voice, Touch, Text, Document Uploads                                           |    |
|    |  Outputs: Multi-language Results & Interactive UI                                       |    |
|    +-----------------------------------------------------------------------------------------+    |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| 2. ROUTING & ACCESS                                                                               |
|    +---------------------------------------+       +-----------------------------------------+    |
|    |      API GATEWAY / LOAD BALANCER      |       |    AUTHENTICATION & USER MANAGEMENT    |    |
|    |  - SSL/TLS Termination                |       |  - Identity Verification & SSO          |    |
|    |  - Dynamic Request Routing            |<----->|  - Role-Based Access Control (RBAC)     |    |
|    |  - Rate Limiting & DDoS Protection    |       |  - Patient, Doctor, Admin Authorization |    |
|    +---------------------------------------+       +-----------------------------------------+    |
+---------------------------------------------------------------------------------------------------+
                                                 |
                                                 v
+---------------------------------------------------------------------------------------------------+
| 3. APPLICATION LAYER                                                                              |
|    +-----------------------------------------------------------------------------------------+    |
|    |                              BACKEND API (CORE ENGINE)                                  |    |
|    |  - Input Validation & Intake Workflow Coordination                                      |    |
|    |  - Orchestrates: AI History Engine, Document Intelligence, AYUSH Module, Clinical Logic  |    |
|    +-----------------------------------------------------------------------------------------+    |
+---------------------------------------------------------------------------------------------------+
                                           |           |
                     +---------------------+           +---------------------+
                     |                                                       |
                     v                                                       v
+------------------------------------------+   +----------------------------------------------------+
| 4. AI / ML SERVICES LAYER                |   | 5. DATA STORAGE LAYER                              |
|    +--------------------------------+    |   |    +------------------------------------------+    |
|    |    MACHINE LEARNING SERVICES   |    |   |    |            PRIMARY DATABASE              |    |
|    |  - LLM / NLP Clinical Engine   |    |   |    |  - Patient Profiles & Medical History    |    |
|    |  - Document OCR Engine         |    |   |    |  - Scans, Media & Document Store         |    |
|    |  - Red-Flag Detection Model    |    |   |    |  - Prediction, Analysis & Audit Logs     |    |
|    |  - Clinical Rec Engine         |    |   |    +------------------------------------------+    |
|    +--------------------------------+    |   +----------------------------------------------------+
+------------------------------------------+
                     |
                     v
+---------------------------------------------------------------------------------------------------+
| 6. ECOSYSTEM INTEGRATION                                                                          |
|    +-----------------------------------------------------------------------------------------+    |
|    |                                   INTEGRATION LAYER                                     |    |
|    |  - ABDM / ABHA Health Network Integration                                               |    |
|    |  - Hospital HIS / EMR System Connectors                                                 |    |
|    |  - Standardized Medical Terminology (SNOMED CT, ICD-10, LOINC)                          |    |
|    +-----------------------------------------------------------------------------------------+    |
+---------------------------------------------------------------------------------------------------+
                                                 ^
                                                 |
+---------------------------------------------------------------------------------------------------+
| 7. FOUNDATIONAL SERVICES (CROSS-CUTTING)                                                          |
|    +---------------------------------------+       +-----------------------------------------+    |
|    |         SECURITY & COMPLIANCE         |       |         OPERATIONS & MONITORING         |    |
|    |  - End-to-End Encryption (AES-256)    |       |  - Real-Time Health & Performance Metrics|    |
|    |  - Patient Consent Management         |       |  - Automated Backups & Disaster Recovery|    |
|    |  - Audit Logging & Regulatory Compliance |    |  - Multi-Channel Notifications (Push/SMS)|    |
|    +---------------------------------------+       +-----------------------------------------+    |
+---------------------------------------------------------------------------------------------------+
```

---

## Detailed Component Specifications

### 1. Frontend
* **Purpose:** Serves as the interactive touchpoint for both patients and healthcare providers.
* **Supported Platforms:** Web applications, native mobile apps (iOS & Android), tablet interfaces, and desktop environments.
* **Input Capabilities:** Captures multimodal inputs including speech/voice notes, direct touch, free text, and uploaded medical documents/images.
* **Output Capabilities:** Displays real-time clinical outputs, diagnostic summaries, and recommendations localized to the user's preferred language.

### 2. API Gateway / Load Balancer
* **Purpose:** Entry portal for all network communications between client apps and backend microservices.
* **Functions:**
  * Ingress management and traffic routing.
  * Load balancing across active application container clusters.
  * SSL/TLS termination, rate limiting, and protection against unauthorized spikes or DDoS attacks.

### 3. Authentication & User Management
* **Purpose:** Secures user access and controls capabilities based on identity and role.
* **Functions:**
  * Multi-factor authentication (MFA) and secure identity verification.
  * Granular Role-Based Access Control (RBAC) distinguishing Patients, Physicians, Specialists, and Administrators.
  * Session lifecycle management and token issuance (OAuth 2.0 / OIDC).

### 4. Backend API (Application Layer)
* **Purpose:** Core business logic and workflow engine of the platform.
* **Functions:**
  * Validates and sanitizes all incoming requests.
  * Orchestrates data flow across sub-modules:
    * **AI History Engine:** Assembles patient longitudinal context.
    * **Document Intelligence:** Manages uploaded files for extraction.
    * **AYUSH Module:** Incorporates traditional system workflows and clinical logic.
    * **Clinical Summary Engine:** Synthesizes structured insights for doctor review.

### 5. Machine Learning Services
* **Purpose:** Core analytical engine delivering predictive insights and automated document comprehension.
* **Sub-components:**
  * **LLM / NLP Model:** Understands clinical natural language, extracts symptoms, and assists in differential diagnosis drafting.
  * **OCR Engine:** Parses optical text from uploaded medical prescriptions, lab reports, and imaging documents.
  * **Red-Flag Detection Model:** Performs real-time risk triage to flag critical symptoms or urgent medical emergencies immediately.
  * **Recommendation Engine:** Generates evidence-based treatment suggestions and follow-up guidance.

### 6. Database (Data Storage Layer)
* **Purpose:** High-availability, secure persistent storage for structured and unstructured healthcare data.
* **Stored Datasets:**
  * Patient demographics and identity mapping.
  * Longitudinal electronic health records (EHR) and clinical encounter notes.
  * Binary object storage for uploaded scans, PDFs, and media.
  * Model inference logs, prediction histories, and complete audit trails.

### 7. Integration Layer
* **Purpose:** Enables seamless interoperability with national health networks and external healthcare provider infrastructures.
* **Integrations:**
  * **ABDM / ABHA:** Full compliance with Ayushman Bharat Digital Mission standards (ABHA ID verification, health record linking).
  * **Hospital HIS / EMR:** Bi-directional interface with legacy hospital systems via modern APIs (HL7 / FHIR).
  * **Terminology Services:** Standardizes clinical data using SNOMED CT, ICD-10, and LOINC codes.

### 8. Security & Compliance
* **Purpose:** Ensures strict compliance with healthcare data regulations (e.g., HIPAA, DISHA, GDPR).
* **Guarantees:**
  * Data encryption at rest (AES-256) and in transit (TLS 1.3).
  * Explicit, granular patient consent management.
  * Immutable event logging and access tracing for regulatory auditing.

### 9. Operations & Monitoring
* **Purpose:** Maintains platform stability, reliability, performance, and timely user notifications.
* **Capabilities:**
  * Real-time metrics collection, error logging, and performance dashboarding.
  * Automated database backup routines and disaster recovery mechanisms.
  * Notification dispatch engine (Push notifications, SMS, Email, In-app alerts).

---

## Architectural Summary Matrix

| Layer | Component | Core Responsibilities | Key Technologies / Protocols |
| :--- | :--- | :--- | :--- |
| **Client** | Frontend | Patient & Doctor interfaces across Web/Mobile/Tablet | React, React Native, WebRTC, i18n |
| **Routing** | API Gateway | Load distribution, routing, rate limiting | Nginx / Envoy, API Gateway |
| **Access Control** | Auth Management | Identity verification, RBAC, session security | OAuth2, OIDC, JWT |
| **Application** | Backend API | Intake coordination, module orchestration | Python / FastAPI / Node.js, REST/gRPC |
| **Intelligence** | ML Services | NLP reasoning, OCR, red-flag alert model | PyTorch, Hugging Face, Tesseract OCR |
| **Storage** | Database Layer | Patient records, telemetry, document blobs | PostgreSQL, Redis, S3-Compatible Storage |
| **Integration** | Integration Layer | ABDM/ABHA sync, hospital EMR integrations | FHIR, HL7, SNOMED CT, ICD-10 |
| **Security** | Security & Compliance | Consent workflow, encryption, audit logs | AES-256, TLS 1.3, Key Management Service |
| **Operations** | Ops & Monitoring | Monitoring, automated backups, alert dispatch | Prometheus, Grafana, Push Notifications |
