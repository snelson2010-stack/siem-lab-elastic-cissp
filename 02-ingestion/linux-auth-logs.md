# 🛡️ Linux Authentication Monitoring Lab  
## SSH Authentication Logging & Brute Force Detection (Elastic Stack)

---

## 📌 1. Objective

This lab demonstrates centralized monitoring of SSH authentication activity on a Linux endpoint using the Elastic Stack.

A controlled authentication stress test was performed using Hydra to simulate repeated login attempts. The goal is to validate:

- Host-based authentication logging
- SIEM ingestion via Elastic Agent
- Detection visibility in Kibana
- Log pipeline integrity and reliability

This lab aligns with CISSP domains:
- Security Operations (logging and monitoring)
- Identity and Access Management (authentication controls)
- Security Assessment and Testing

---

## ⚔️ 2. Controlled Authentication Stress Test (Hydra)

![Hydra Brute Force](images/hydra-bruteforce-ssh.png)

A controlled password-guessing simulation was executed against an SSH service (port 22) to generate authentication failure events for monitoring and analysis.

### Observations:
- Multiple automated SSH login attempts were generated
- Invalid credential combinations were tested
- SSH service responded with repeated authentication failures
- Behavior is consistent with brute force attack patterns

### Security Relevance:
This test validates detection capabilities for:
- Credential stuffing attempts
- Brute force authentication behavior
- Threshold-based alerting scenarios

---

## 🐧 3. Host-Level Authentication Logging (Linux Endpoint)

![Linux Auth Logs](images/linux-auth-log-terminal.png)

The Linux endpoint generated authentication logs during the controlled test, capturing SSH access attempts in real time.

### Observations:
- `Failed password` entries recorded in system logs
- Repeated invalid username attempts detected
- Multiple connection attempts over SSH (port 22)
- Clear pattern of unauthorized authentication attempts

### Security Relevance:
This represents **audit logging at the endpoint level**, supporting:
- Accountability and traceability of authentication events
- Incident investigation and forensic analysis
- Monitoring of unauthorized access attempts

---

## ⚙️ 4. Log Collection & Agent Health (Elastic Agent)

![Elastic Agent Status](images/elastic-agent-status-healthy.png)

Elastic Agent is operating in a **healthy state**, forwarding authentication logs from the Linux endpoint to Elasticsearch for centralized analysis.

### Observations:
- Agent status is active and healthy
- Log forwarding is functioning normally
- Authentication events are successfully ingested into SIEM
- No disruption in telemetry pipeline observed

### Security Relevance:
This confirms:
- Continuous log visibility from endpoint to SIEM
- Reliable security monitoring pipeline
- Integrity of authentication telemetry collection

---

## 📡 5. Log Flow Architecture

```text
Linux SSH Authentication Events
        ↓
System Auth Logs (/var/log/auth.log)
        ↓
Elastic Agent (Log Forwarding)
        ↓
Elasticsearch (Central Storage & Indexing)
        ↓
Kibana (Visualization & Detection)
Security Relevance:

This represents a centralized logging architecture, essential for:

Security monitoring and alerting
Incident response investigations
Compliance and audit requirements
📊 6. Security Monitoring (Kibana Dashboard)

(Insert Kibana dashboard screenshot here if available)

Recommended visualizations:

Failed SSH login attempts over time
Top source IP addresses attempting access
Authentication success vs failure ratio
Event frequency trends
Security Relevance:

Enables detection of:

Brute force attempts
Suspicious authentication patterns
Anomalous login behavior
🚨 7. Key Findings
Controlled SSH authentication stress test successfully generated log events
Linux endpoint properly recorded authentication failures
Elastic Agent is healthy and reliably forwarding logs
SIEM provides centralized visibility into authentication activity
Log pipeline integrity is essential for accurate detection
🧠 8. Lessons Learned
Centralized logging is critical for maintaining visibility into authentication activity across endpoints.
Endpoint logs provide the authoritative source of authentication truth, while SIEM enables correlation and analysis.
Log pipeline health (Elastic Agent status) directly impacts detection capability and visibility.
Controlled attack simulation is effective for validating detection rules and alert thresholds.
Authentication failures alone are not sufficient for incident confirmation; correlation improves accuracy.
🔐 9. CISSP Domain Mapping
🛡️ Domain 7: Security Operations
Monitoring SSH authentication events
SIEM-based log analysis and detection
Ensuring log ingestion and pipeline reliability
🔑 Domain 5: Identity and Access Management (IAM)
SSH authentication monitoring
Detection of repeated failed login attempts
Unauthorized access identification
🧪 Domain 6: Security Assessment and Testing
Controlled brute force simulation (Hydra)
Validation of logging and detection controls
Testing SIEM rule effectiveness
🏗️ Domain 3: Security Architecture and Engineering
Centralized logging pipeline design (Linux → Agent → SIEM)
Telemetry collection architecture
Log integrity and system reliability assurance
📡 Domain 4: Communication and Network Security (Secondary)
SSH traffic monitoring over port 22
Network-based authentication visibility
🧠 10. Conclusion

This lab demonstrates effective implementation of centralized authentication monitoring using Elastic Stack.

Key outcomes:

Host-based logs successfully captured authentication events
SIEM provided centralized visibility and analysis
Log pipeline health is critical for security assurance
Controlled testing validated detection readiness
