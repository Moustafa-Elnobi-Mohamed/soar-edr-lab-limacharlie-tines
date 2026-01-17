# SOAR–EDR Lab: LimaCharlie + Tines Automation (Slack/Email Alerts + Endpoint Isolation)

## Summary
This project demonstrates an end-to-end SOAR–EDR workflow that detects credential-dumping activity on a Windows endpoint and orchestrates a controlled, analyst-approved response. I used LimaCharlie for endpoint telemetry, detection logic, and containment, and Tines to automate enrichment, notifications, approval, and isolation. The result is a production-aligned incident response pipeline that reduces mean time to detect (MTTD) and mean time to respond (MTTR) while maintaining operational safety and auditability.

## Core Skills Demonstrated
### Detection Engineering and EDR Operations
- Built a multi-indicator detection rule using process events, file path, command-line inspection, and hash matching
- Validated rule logic with test events and tuned metadata (severity, false-positive notes, ATT&CK tags)
- Investigated detection telemetry and confirmed indicator fidelity (process, hash, command line)

### SOAR Automation and Orchestration
- Designed and implemented a Tines workflow to ingest detection events, enrich context, and automate response actions
- Implemented human-in-the-loop approval to prevent unnecessary disruption
- Automated multi-channel notifications (Slack and email) and included investigation links and key event context

### Incident Response and Governance Mindset (GRC-aligned)
- Documented detection rationale, false-positive considerations, severity justification, and evidence chain
- Used MITRE ATT&CK mapping as post-detection classification to support reporting and control coverage discussions
- Produced a structured repository with separation of architecture, detections, response logic, and evidence for auditability

## Architecture
- Architecture documentation: `architecture/architecture.md`
- System diagram: `architecture/architecture-diagram.png`
- Tines workflow diagram: `architecture/tines-soar-workflow-diagram.png`

High-level flow:
1. Windows endpoint generates telemetry through LimaCharlie sensor
2. LimaCharlie detection rule matches suspicious credential-dumping indicators
3. Event is forwarded to Tines for orchestration
4. Tines enriches and formats alert data
5. Slack and email alerts are sent automatically
6. Analyst approval is requested for containment
7. Endpoint is isolated via LimaCharlie response action
8. Isolation status is verified and recorded

## What I Built Step-by-Step
### 1. Endpoint Setup and Telemetry
- Deployed a Windows endpoint and installed the LimaCharlie sensor
- Verified the sensor service was running and endpoint telemetry was reporting

Evidence:
- `evidence/01-limacharlie-agent-install-success.png`
- `evidence/02-limacharlie-service-running.png`

### 2. Detection Engineering (LimaCharlie)
- Authored a detection rule for LaZagne credential-dumping behavior using:
  - Process creation signals (`NEW_PROCESS`, `EXISTING_PROCESS`)
  - File path match (`LaZagne.exe`)
  - Command-line match (`LaZagne`)
  - Optional hash validation for higher confidence
- Validated the rule using LimaCharlie test events and confirmed reliable matching

Detection artifacts:
- Rule: `detections/lazagne-detection.yaml`
- Rationale: `detections/detection-explanation.md`
- ATT&CK mapping: `detections/mitre-mapping.md`

Evidence:
- `evidence/13-limacharlie-dr-rule-definition.png`
- `evidence/14-limacharlie-rule-test-match.png`
- `evidence/15-limacharlie-dr-rule-enabled.png`

### 3. Adversary Simulation (Controlled Lab)
- Executed LaZagne on the endpoint to generate a realistic detection event
- Verified telemetry and event details (hash, file path, command line) for rule alignment

Evidence:
- `evidence/03-lazagne-executed-endpoint.png`
- `evidence/04-limacharlie-detection-timeline.png`
- `evidence/05-limacharlie-event-details.png`

### 4. SOAR Workflow (Tines) with Approval Gate
- Created a Tines story to:
  - Ingest the LimaCharlie detection via webhook/API
  - Enrich and normalize fields used for triage
  - Notify Slack and email with structured alert content
  - Prompt the analyst to approve or deny endpoint isolation
  - Execute isolation in LimaCharlie if approved
  - Confirm the outcome and send a closure update

Response artifacts:
- Tines story export: `response/tines-workflow.json`
- Slack template: `response/slack-message-template.html`
- Email template: `response/email-alert-template.txt`
- Decision logic: `response/isolation-decision-logic.md`
- Response summary: `response/response-summary.md`

Evidence:
- `evidence/06-tines-analyst-isolation-prompt.png`
- `evidence/07-tines-slack-message-template.png`

### 5. Automated Notifications (Slack and Email)
- Sent enriched alerts to Slack for SOC visibility
- Sent email alerts for redundancy and audit trail

Evidence:
- `evidence/08-slack-detection-alert.png`
- `evidence/09-slack-isolation-confirmation.png`
- `evidence/10-email-detection-alert.png`

### 6. Containment and Verification (LimaCharlie Isolation)
- Isolated the endpoint using LimaCharlie response action triggered by Tines after approval
- Verified isolation in multiple ways:
  - LimaCharlie control-plane status shows network access isolated
  - Endpoint network connectivity was blocked (ping failure)
  - Slack confirmation message recorded the outcome

Evidence:
- `evidence/11-limacharlie-sensor-isolated-status.png`
- `evidence/12-endpoint-network-blocked-ping-failure.png`

## Evidence Chain (Quick Review)
For reviewers who want a fast end-to-end proof trail:
1. Sensor deployed and running: `evidence/01-02`
2. Detection created and tested: `evidence/13-15`
3. Tool execution and event captured: `evidence/03-05`
4. Tines approval gate: `evidence/06`
5. Slack/email notifications: `evidence/08-10`
6. Isolation confirmed: `evidence/11-12` and Slack closure `evidence/09`

## Security and Documentation Notes
- The Tines story export is sanitized; webhook URLs were randomized prior to export.
- This lab was executed in a controlled environment for defensive learning and documentation.
- Any sensitive identifiers should be redacted prior to public sharing.

## Repository Guide
- `architecture/`: system design and workflow diagrams, architecture write-up
- `detections/`: detection rule and detection documentation
- `response/`: automation artifacts and response logic (no screenshots)
- `evidence/`: proof screenshots demonstrating detection, orchestration, and containment

## Learning Resources and Cost Efficiency

This project was built using openly available, vendor documentation and free educational resources, including technical walkthroughs and security-focused content on YouTube. Artificial intelligence tools were used as an assistant for ideation, documentation refinement, and workflow validation, similar to how modern security teams leverage AI for productivity and analysis.

The lab environment was intentionally designed to be cost-efficient. Cloud resources were provisioned using free tiers and credits, and no paid subscriptions or services were required to design, deploy, detect, automate, or respond to the simulated incident.

This approach demonstrates the ability to:
- Learn and implement complex security workflows independently
- Leverage open-source and publicly available knowledge responsibly
- Control costs while still achieving production-aligned security outcomes

## Author
Moustafa Mohamed  
GitHub: https://github.com/Moustafa-Elnobi-Mohamed
