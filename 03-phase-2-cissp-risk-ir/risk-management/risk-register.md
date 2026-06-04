# Risk Register

## Purpose

This risk register documents security risks identified during Phase 2 of the CISSP Risk Management and Incident Response Expansion. The risks are based on the Elastic SIEM lab, the new Windows Enterprise VM endpoint, authentication testing, Nmap-related network telemetry review, and Elastic Agent health monitoring.

---

## Risk Register

| Risk ID | Risk Description | Likelihood | Impact | Risk Level | Evidence | Mitigation |
|---|---|---|---|---|---|---|
| R1 | Windows failed logon activity may indicate brute force attempts or unauthorized access attempts | Medium | High | High | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png) | Monitor Event ID 4625, alert on repeated failures, enforce account lockout and strong passwords |
| R2 | Failed logons followed by a successful logon may indicate credential compromise | Medium | High | High | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png), [Successful Logon 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png) | Correlate failed and successful logons, investigate unusual successful access after repeated failures |
| R3 | Network reconnaissance against the Windows endpoint may identify exposed services such as RDP | Medium | Medium | Medium | [Nmap-Related Network Events](../screenshots/phase-2-nmap-related-windows-network-events.png) | Restrict exposed services, limit RDP access, review firewall rules, monitor network telemetry |
| R4 | Limited scan visibility may prevent full detection of port scanning activity | Medium | Medium | Medium | [Nmap-Related Network Events](../screenshots/phase-2-nmap-related-windows-network-events.png) | Enable Windows Firewall logging, add network-flow logging, validate Elastic Defend network visibility |
| R5 | Elastic Agent failure may interrupt endpoint monitoring and reduce SIEM visibility | Medium | High | High | [Agent Healthy PowerShell](../screenshots/windows-agent-powershell-healthy.png), [Fleet Healthy](../screenshots/fleet-windows-agent-healthy.png) | Monitor agent health in Fleet, investigate missed check-ins, restart or redeploy the agent if needed |
| R6 | Weak authentication controls may allow password guessing or unauthorized access | Medium | High | High | [Failed Logon 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png) | Use strong passwords, enforce lockout thresholds, monitor privileged accounts, review authentication logs |
| R7 | Incomplete governance documentation may make incident response inconsistent | Medium | Medium | Medium | [Brute Force Playbook](../incident-response/brute-force-incident-playbook.md), [Nmap Playbook](../incident-response/nmap-scan-incident-playbook.md), [Logging Policy](../security-governance/logging-monitoring-policy.md) | Maintain playbooks, update policies, document lessons learned after testing |
| R8 | Excessive log volume may consume storage and slow investigation | Medium | Medium | Medium | Endpoint and Discover event volume observed during testing | Implement log retention policies, review noisy event sources, tune data collection where appropriate |

---

## Risk Details

### R1: Windows Failed Logon Activity

Failed Windows logons were generated during Phase 2 by entering the wrong password on the Windows Enterprise VM. The events appeared in Kibana Discover as Event ID 4625.

Evidence:

![Failed Windows logon Event ID 4625](../screenshots/phase-2-windows-failed-logon-4625-discover.png)

This risk maps to authentication monitoring and account protection.

---

### R2: Failed Logons Followed by Successful Logon

During brute force investigations, analysts should check whether repeated failed logons were followed by a successful logon. This may indicate that an attacker eventually guessed or obtained valid credentials.

Evidence:

![Successful Windows logon Event ID 4624](../screenshots/phase-2-windows-successful-logon-4624-discover.png)

This risk supports correlation between Event ID 4625 and Event ID 4624.

---

### R3: Network Reconnaissance Against Windows Endpoint

Nmap-related testing was performed from the Kali attacker VM. Kibana Discover showed Windows endpoint network telemetry from the Kali source IP.

Evidence:

![Nmap-related Windows network events](../screenshots/phase-2-nmap-related-windows-network-events.png)

This risk maps to reconnaissance detection and network security monitoring.

---

### R4: Limited Port Scan Visibility

Not every Nmap probe appeared as a separate event in Kibana Discover. This creates a visibility gap and shows that additional logging may be required for full scan detection.

Recommended improvements:

- Enable Windows Firewall logging.
- Add network-flow logging.
- Validate Elastic Defend network event coverage.
- Correlate endpoint telemetry with attacker command output.

---

### R5: Elastic Agent Failure

During testing, the Windows Elastic Agent stopped and had to be restarted. This is a major operational risk because endpoint visibility depends on agent health.

Evidence:

![Elastic Agent healthy in PowerShell](../screenshots/windows-agent-powershell-healthy.png)

![Windows endpoint healthy in Fleet](../screenshots/fleet-windows-agent-healthy.png)

Mitigation includes monitoring Fleet health and investigating missed check-ins quickly.

---

## Risk Review Process

Risks are reviewed after:

- New endpoint deployments
- Authentication testing
- Attack simulations
- Detection validation activities
- Agent failures or missed check-ins
- Major lab architecture changes

---

## CISSP Domain Alignment

| CISSP Domain | Risk Register Relevance |
|---|---|
| Domain 1: Security and Risk Management | Risk identification, mitigation, governance, and documentation |
| Domain 4: Communication and Network Security | Network reconnaissance and segmentation risks |
| Domain 5: Identity and Access Management | Failed and successful logon monitoring |
| Domain 6: Security Assessment and Testing | Detection validation and visibility gap identification |
| Domain 7: Security Operations | SIEM monitoring, incident response, evidence collection, and agent health monitoring |

---

## Summary

This risk register connects the Phase 2 technical evidence to CISSP risk management concepts. It demonstrates that the Windows Enterprise VM expansion supports practical risk identification, monitoring, mitigation planning, and security operations documentation.
