# Risk Register

| Risk ID | Risk Description | Likelihood | Impact | Risk Level | Mitigation |
|---|---|---|---|---|---|
| R1 | SSH brute force attack against Linux systems | Medium | High | High | Monitor failed logins, enforce strong authentication |
| R2 | Unauthorized privilege escalation | Medium | High | High | Monitor sudo and privileged account activity |
| R3 | Poor log visibility | Medium | Medium | Medium | Validate Elastic Agent health and ingestion pipelines |
| R4 | Misconfigured network segmentation | Low | High | Medium | Review firewall and VM network configurations |
| R5 | Weak authentication controls | Medium | High | High | Implement strong passwords and account monitoring |
| R6 | Elastic Agent failure | Medium | Medium | Medium | Monitor agent status and enrollment health |
| R7 | Excessive log retention consuming resources | Medium | Medium | Medium | Implement retention policies and monitoring |

## Risk Review Process

Risks are reviewed after major lab changes, new endpoint deployments, or security testing activities.

## CISSP Domain Alignment

- Domain 1: Security and Risk Management
- Domain 7: Security Operations
