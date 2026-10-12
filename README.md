# Sentinel AI
## Real-World Cyber Defense, Security Monitoring, and AI-Assisted Investigation

Sentinel AI is a defensive cybersecurity platform designed to collect security events from authorized sources, normalize and analyze those events, display findings in a live web dashboard, and help analysts investigate potential incidents.

The project is being built from scratch with a clean, modular architecture. Its goal is to move beyond attack simulations and demo-only dashboards toward real security-event monitoring in an authorized lab or production environment.

> **Project type:** Cybersecurity monitoring and incident investigation platform  
> **Primary language:** Python  
> **Backend:** Flask REST API  
> **Frontend:** HTML, CSS, JavaScript  
> **Database:** PostgreSQL  
> **Security integrations:** Wazuh and Suricata (planned integration modules)  
> **Deployment target:** Cloud server using Docker  
> **Development environment:** Windows 10/11  
> **Project status:** Initial design and implementation

---

## 1. Project Overview

Organizations generate security-related information from endpoints, servers, applications, and network sensors. Reviewing these records manually can make it difficult to identify suspicious activity, understand its context, and prioritize incidents.

Sentinel AI is intended to provide one web-based workspace for collecting and reviewing security events from configured, authorized sources. It will organize events, apply detection rules, correlate related activity, calculate transparent risk indicators, and present results to analysts through a centralized dashboard.

AI-assisted features will help summarize evidence and suggest investigation steps. They will support analyst decisions rather than replace human review or claim that an event is malicious without sufficient evidence.

The system will only report a source as connected when the integration has been configured and its operation has been verified. Test data will be clearly distinguished from real collected events.

## 2. Problem Statement

Simulation-only cybersecurity projects can demonstrate concepts, but simulated events do not show whether a monitoring system can collect, process, store, and present real security telemetry. A dashboard with hardcoded numbers also cannot provide reliable operational monitoring.

Sentinel AI addresses these limitations by using a structured backend, persistent storage, authenticated event ingestion, real security-tool integrations, and a dashboard whose metrics are derived from collected records.

## 3. Project Objectives

- Collect security events from authorized endpoints, network sensors, and security platforms.
- Normalize events into a consistent format for analysis.
- Detect suspicious patterns using transparent, configurable detection rules.
- Correlate related events to help identify incidents.
- Display event counts, severity levels, assets, timelines, and incident status in a web dashboard.
- Provide AI-assisted summaries and investigation recommendations with supporting evidence.
- Record analyst actions in an audit trail.
- Protect access through authentication and role-based authorization.
- Support local development on Windows and controlled cloud deployment.
- Maintain a clear distinction between verified telemetry, test records, alerts, and confirmed incidents.

## 4. Scope

### Included in the intended system

- Secure user login and role-based access.
- Dashboard backed by persistent event data.
- Event ingestion API for trusted collectors.
- Event normalization, validation, deduplication, and storage.
- Event filtering and investigation views.
- Severity classification and configurable detection rules.
- Correlation of related events.
- Asset inventory and source health indicators.
- Incident lifecycle management and analyst notes.
- Audit logging and report generation.
- Wazuh and Suricata integration modules.
- AI-assisted event explanation and investigation summaries.
- Docker-based deployment configuration and automated tests.

### Outside the initial scope

- Unauthorized scanning or exploitation of third-party systems.
- Uncontrolled automatic blocking or destructive response actions.
- Treating AI output as proof of compromise.
- Claiming enterprise-grade protection before testing, review, and operational integration are complete.

## 5. Main Features

### 5.1 Security Dashboard

The dashboard will show data derived from the backend and database, such as:

- Total events received during a selected period.
- Open, resolved, and investigated alerts.
- Event counts by severity.
- Recent events and incident timelines.
- Monitored assets and last-seen timestamps.
- Collector connection and ingestion health.
- Trends over time.

Empty or unconfigured integrations must be shown accurately. The interface must not present placeholder data as live telemetry.

### 5.2 Security Event Collection

The collection layer will accept events from approved sources. It will validate incoming payloads, record source and timestamp information, normalize supported fields, and preserve relevant event evidence.

Planned sources include:

- Wazuh security alerts and endpoint telemetry.
- Suricata EVE JSON network-security events.
- Approved system or application logs through a configured collector.
- The authenticated Sentinel AI ingestion API.

The exact collection method will depend on the source configuration and deployment environment.

### 5.3 Detection and Event Correlation

The detection engine will evaluate events against documented rules. Examples include repeated authentication failures, unusual event frequency, or multiple related alerts from one monitored asset.

Detection results should include the rule identifier, severity, timestamp, relevant evidence, and reason the rule matched. A risk score is a prioritization aid, not a definitive measure of compromise.

### 5.4 Incident Management

Analysts will be able to review related events, assign an investigation status, add notes, document evidence, and record resolution details. Changes to incident status and other important actions will be auditable.

### 5.5 AI-Assisted Investigation

AI features may summarize selected event evidence, explain why a detection rule fired, group related observations, and suggest defensive investigation steps.

AI output must:
- Refer to available event evidence.
- Identify uncertainty and missing information.
- Avoid inventing indicators, timestamps, or conclusions.
- Require analyst review before an incident is considered confirmed.
- Never trigger high-impact response actions without explicit authorization.

### 5.6 Users, Roles, and Audit Trail

The system will support separate permissions for administrators and security analysts. Sensitive actions will be logged with the acting user, action, time, and relevant record identifier.

## 6. Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| Backend | Python, Flask | Application logic and REST API |
| Frontend | HTML5, CSS3, JavaScript | Responsive dashboard interface |
| UI framework | Bootstrap | Consistent responsive layout |
| Charts | Chart.js | Event and severity visualizations |
| Database | PostgreSQL | Persistent application and incident data |
| Security monitoring | Wazuh | Endpoint telemetry and security alerts |
| Network detection | Suricata | Network threat detection events |
| Data processing | Python modules | Normalization, validation, detection, correlation |
| AI assistance | Pluggable model/API integration | Evidence-based summaries and recommendations |
| Containers | Docker, Docker Compose | Reproducible development and deployment |
| Web serving | Gunicorn behind HTTPS reverse proxy | Hosted application serving |
| Testing | pytest (planned) | Unit and integration testing |

Technology choices may be adjusted as implementation and integration requirements are verified.

## 7. High-Level Architecture

```text
Authorized Endpoints / Servers / Network Sensors
                     |
          +----------+----------+
          |                     |
        Wazuh                Suricata
          |                     |
          +----------+----------+
                     |
             Collector Adapters
                     |
          Validation and Normalization
                     |
           Detection and Correlation
                     |
              PostgreSQL Database
                     |
              Flask REST API
                     |
        Authenticated Web Dashboard
                     |
        Analyst Review and Incidents
                     |
       AI-Assisted Evidence Summaries
```

The AI component will receive only the information needed for a supported analysis task. Secrets, passwords, and unnecessary personal data must not be sent to an external AI service.

## 8. Proposed Project Structure

```text
sentinel-ai/
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── extensions.py
│   ├── models/
│   ├── auth/
│   ├── dashboard/
│   ├── events/
│   ├── collectors/
│   │   ├── wazuh/
│   │   └── suricata/
│   ├── detection/
│   ├── incidents/
│   ├── ai_assistant/
│   ├── assets/
│   ├── audit/
│   ├── api/
│   ├── templates/
│   └── static/
├── migrations/
├── tests/
│   ├── unit/
│   └── integration/
├── deployment/
│   ├── Dockerfile
│   └── compose.yaml
├── scripts/
├── docs/
├── .env.example
├── .gitignore
├── requirements.txt
├── wsgi.py
└── README.md
```

This is the intended structure for the clean rebuild. Files and modules will be added as their functionality is implemented and tested.

## 9. Data Model (Planned)

The initial data model is expected to include:

- **User:** account, password hash, role, and account status.
- **Asset:** monitored device or system, asset type, owner/label, and last-seen status.
- **SecurityEvent:** source, event type, event time, ingestion time, severity, asset, normalized fields, and event evidence.
- **DetectionRule:** rule name, logic/version, severity, and enabled status.
- **Alert:** a detection result linked to one or more security events.
- **Incident:** case title, severity, status, assignee, timestamps, notes, and resolution.
- **AuditLog:** actor, action, timestamp, and affected record.
- **CollectorStatus:** source identifier, health status, last successful collection, and error details.

The schema will be finalized during implementation and maintained with database migrations.

## 10. Event Processing Workflow

1. An authorized source generates security telemetry.
2. A configured collector forwards the event to Sentinel AI or an integrated security platform.
3. The ingestion layer authenticates the source where supported and validates the payload.
4. The normalizer maps supported fields to the common event schema.
5. Duplicate and malformed records are handled according to documented rules.
6. Detection rules evaluate the event and relevant context.
7. Matching detections create or update alerts and incidents.
8. The database stores the event and investigation history.
9. The dashboard retrieves current metrics and records through the backend API.
10. An analyst reviews the evidence and decides on the appropriate response.

## 11. Installation and Development

### Prerequisites

- Windows 10/11.
- Python 3.11 or a compatible supported version.
- Git.
- Visual Studio Code or another Python IDE.
- Docker Desktop for container-based development.
- PostgreSQL for local non-container development, if desired.

### Initial setup

The exact commands will be finalized with the implementation. The expected workflow is:

1. Clone or extract the project.
2. Create and activate a Python virtual environment.
3. Install dependencies from `requirements.txt`.
4. Copy `.env.example` to `.env`.
5. Configure a strong application secret and database credentials.
6. Run database migrations.
7. Create the first administrator securely.
8. Start the Flask development server.
9. Open the dashboard locally.
10. Configure an approved event source and verify collection before relying on monitoring data.

Never commit `.env` or real credentials to source control.

## 12. Cloud Deployment Plan

The target deployment is a cloud-hosted application. A planned deployment will include:

- Dockerized application services.
- PostgreSQL with persistent storage and tested backups.
- HTTPS through a reverse proxy or managed ingress.
- Restricted inbound network rules.
- Secret management outside source control.
- Secure administrator provisioning and role-based access.
- Application, collector, and database health monitoring.
- Database migrations and a rollback plan.
- Defined event retention and privacy policies.
- Centralized error logging and alerting.
- Tested backup restoration and incident recovery procedures.

Security sensors and collectors should communicate with the hosted platform over authenticated, encrypted channels where supported. The public web application must not expose management interfaces for monitoring tools.

## 13. Security and Privacy Requirements

- Use strong password hashing and secure session-cookie settings.
- Add CSRF protection, login throttling, and appropriate session expiration.
- Enforce authorization on every protected API and record.
- Validate and limit incoming event payloads.
- Keep secrets in environment variables or a managed secret store.
- Use TLS for network communication.
- Restrict database and monitoring-tool access.
- Avoid storing credentials or unnecessary sensitive information in event payloads.
- Define retention, deletion, and backup policies.
- Log important analyst and administrator actions.
- Review dependencies and run security tests before deployment.
- Use only systems and networks for which monitoring is authorized.

## 14. Testing and Validation

The project should include tests for:

- Login, logout, and role-based permissions.
- API authentication and malformed payload rejection.
- Event validation, normalization, and deduplication.
- Severity classification and detection rules.
- Database persistence and migration behavior.
- Incident status changes and audit records.
- Dashboard metrics against stored event records.
- Collector failure, reconnection, and stale-source status.
- AI output handling, evidence grounding, and failure behavior.
- Deployment configuration and backup restoration.

A feature will be considered complete only when its behavior has been implemented and tested. Planned integrations must not be described as operational until verified with actual telemetry.

## 15. Implementation Roadmap

### Phase 1 — Foundation
Create the modular Flask application, configuration, database models, migrations, authentication, and role permissions.

### Phase 2 — Dashboard and Event Management
Build the responsive dashboard, database-backed metrics, event list, filters, event details, incident records, and audit trail.

### Phase 3 — Real Telemetry
Implement and verify the Wazuh integration, Suricata event ingestion, source-health checks, normalization, and error handling.

### Phase 4 — Detection Engine
Develop documented rules, severity assignment, deduplication, event correlation, and explainable risk prioritization.

### Phase 5 — AI Assistance
Add evidence-grounded summaries, investigation guidance, and analyst-reviewed recommendations.

### Phase 6 — Cloud Deployment and Security Review
Containerize the system, deploy behind HTTPS, secure secrets and network access, run automated tests, and verify backups and monitoring.

## 16. Expected Outcome

The intended outcome is a working, modular cybersecurity platform that can ingest actual security telemetry from configured and authorized sources, persist events, detect defined suspicious patterns, and present findings through a professional web dashboard.

The project will also demonstrate secure web development, event processing, database design, defensive security monitoring, incident handling, and carefully controlled AI assistance.

The project must not be described as production-ready or as detecting real attacks until its integrations, security controls, and tests have been implemented and verified.

## 17. Ethical Use Disclaimer

Sentinel AI is designed for defensive cybersecurity monitoring, authorized security research, and incident investigation. Monitoring must be limited to systems and networks for which permission has been granted. Detection alerts are signals for investigation, not automatic proof of malicious activity. Human analysts remain responsible for validating findings and approving consequential response actions.

---

**Sentinel AI — From security telemetry to evidence-based cyber defense.**
