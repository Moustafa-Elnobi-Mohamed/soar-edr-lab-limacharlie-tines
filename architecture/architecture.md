# SOAR–EDR Architecture

## Overview

This project implements an end-to-end **SOAR–EDR pipeline** using **LimaCharlie** for endpoint detection and response and **Tines** for security orchestration and automation.

The architecture is designed to detect credential-dumping activity, enrich alerts, notify security teams, and isolate compromised endpoints with minimal mean time to respond (MTTR).  
The lab mirrors a real-world SOC workflow where automation is combined with analyst oversight.

---

## High-Level Architecture

![SOAR–EDR Architecture Diagram](architecture-diagram.png)

---

## Core Components

### Endpoint (Windows VM)
- Windows-based endpoint hosted in a cloud environment
- LimaCharlie sensor installed and running as a system service
- Simulated attacker activity using credential-dumping tooling (LaZagne)

### LimaCharlie (EDR)
- Collects real-time endpoint telemetry
- Custom detection rule monitors:
  - Process creation events
  - File path and command-line execution
  - Known malicious indicators (hashes)
- Generates structured detection events with rich context

### Tines (SOAR)
- Receives detection events from LimaCharlie
- Enriches and normalizes detection data
- Orchestrates alerting and response actions
- Executes automated containment after analyst approval

### Notification Channels
- **Slack**: Near-real-time SOC alerting
- **Email**: Secondary notification and audit trail

---

## Detection Flow

1. Credential-dumping activity is executed on the endpoint.
2. LimaCharlie sensor captures process and command-line telemetry.
3. Custom detection logic matches malicious indicators.
4. A detection event is generated and forwarded for orchestration.

---

## Automation & Response Flow

1. Tines retrieves the detection event from LimaCharlie.
2. Detection data is enriched and formatted.
3. Alerts are sent to Slack and Email.
4. Analyst approval is requested before containment.
5. Endpoint isolation is executed via LimaCharlie.
6. Isolation status is confirmed and communicated back to Slack.

---

## MITRE ATT&CK Mapping

MITRE ATT&CK is used in this project as a **post-detection classification framework** to contextualize observed behavior and validate security control coverage.

### Observed Techniques

- **T1555 – Credentials from Password Stores**  
  LaZagne attempts to extract stored credentials from browsers and system vaults.

- **T1003 – OS Credential Dumping**  
  The tool targets operating system credential storage mechanisms.

- **T1204 – User Execution**  
  The malicious tool is manually executed on the endpoint.

### Containment & Mitigation

- **T1562 – Impair Defenses (Prevented)**  
  Endpoint isolation prevents further attacker movement and post-exploitation activity.

| ATT&CK Tactic        | Technique ID | Description                          |
|----------------------|--------------|--------------------------------------|
| Credential Access    | T1555        | Credentials from Password Stores     |
| Credential Access    | T1003        | OS Credential Dumping                |
| Execution            | T1204        | User Execution                       |
| Containment          | T1562        | Impair Defenses (Blocked via isolate)|

---

## Security Controls Demonstrated

- Endpoint behavioral detection
- Automated alert enrichment
- Multi-channel notifications
- Analyst-approved automated containment
- Endpoint isolation verification

---

## Design Goals

- Reduce Mean Time to Detect (MTTD)
- Reduce Mean Time to Respond (MTTR)
- Prevent lateral movement through rapid containment
- Balance automation with human oversight

---

## Real-World Relevance

This architecture reflects workflows used in modern SOC environments, including:
- Tier 1 alert ingestion and enrichment
- Tier 2 response orchestration
- Automated containment with validation

The design is modular and can be extended with SIEM integration, ticketing systems, or additional enrichment sources.

---

## Summary

This SOAR–EDR architecture demonstrates a realistic, production-aligned security automation pipeline that integrates detection, orchestration, alerting, and response while maintaining operational safety and auditability.
