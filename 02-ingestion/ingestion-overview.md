# Ingestion Overview

## Purpose
This document defines how security telemetry is collected, processed, normalized, and prepared for analysis within an Elastic SIEM environment. The goal is to ensure consistent, high-quality, and queryable data for detection engineering, incident response, and SOC monitoring.

---

## Ingestion Architecture

### Flow Diagram

```mermaid
flowchart LR
A[Data Sources] --> B[Elastic Agent]
B --> C[Ingest Pipelines]
C --> D[ECS Normalization]
D --> E[Elasticsearch Indices]
E --> F[Kibana Dashboards & Alerts]
1. Data Sources
Linux authentication logs (/var/log/auth.log)
SSH login attempts (success/failure)
System security events
Future: Windows logs, firewall logs, DNS logs
2. Elastic Agent Collection
Collects endpoint logs
Sends telemetry to Elasticsearch
Applies System/Security integrations
Managed via Elastic Fleet
3. Ingestion Pipeline
Raw Log → Parsing → Field Extraction → ECS Mapping → Indexing
Parses unstructured logs
Extracts security fields
Normalizes structure
Handles malformed events
4. ECS Field Mapping (SSH Example)
Raw Field	ECS Field
src_ip	source.ip
username	user.name
timestamp	@timestamp
auth result	event.outcome
ssh event	event.action
5. Pipeline Validation

KQL Checks:

system.auth.ssh.event: "Accepted"
system.auth.ssh.event: "Failed"

Validation checks:

Correct indexing (logs-*)
ECS fields populated
No pipeline errors
Accurate timestamps
Correct IP extraction
6. Index Output
logs-system.auth-*
logs-ssh-*

Used for:

Dashboards
Detection rules
Incident response
Threat hunting
7. SOC Use Cases
Authentication monitoring
Brute force detection
Centralized log analysis
Incident investigation
Compliance auditing
8. CISSP Domain Alignment

## CISSP Domain Alignment

**Domain 3 – Security Architecture & Engineering**
- Ingestion pipeline design
- ECS normalization model

**Domain 5 – Identity & Access Management**
- SSH authentication monitoring
- User behavior tracking

**Domain 6 – Security Assessment & Testing**
- Pipeline validation using KQL
- Log integrity verification

**Domain 7 – Security Operations**
- Continuous monitoring
- Alerting and dashboards
- Incident response support

**Domain 8 – Software Development Security**
- Secure parsing and processing of log data through ingest pipelines  
- Validation of structured data before indexing  
- Prevention of malformed or malicious log injection through controlled field extraction  
- Ensuring data integrity through ECS normalization and pipeline testing  

Continuous monitoring
Alerting and dashboards
Incident response support
9. Lessons Learned
ECS consistency is critical for correlation
Misconfigured fields break detections silently
SSH logs provide high-value attack signals
Validate ingestion before building dashboards
