# Detection Explanation

## Objective

The goal of this detection is to identify credential-dumping activity
associated with the LaZagne tool on Windows endpoints.

Credential dumping represents a high-risk post-exploitation behavior
and often precedes lateral movement or privilege escalation.

---

## Detection Logic

The rule detects LaZagne execution using multiple indicators:

- Process creation events (`NEW_PROCESS`, `EXISTING_PROCESS`)
- File path matching (`LaZagne.exe`)
- Command-line inspection (`LaZagne`)
- Known malicious hash matching (optional)

Using multiple indicators reduces false positives while maintaining
high detection confidence.

---

## Telemetry Sources

The following endpoint telemetry is evaluated:

- Process execution events
- File path metadata
- Command-line arguments
- Binary hash values
- Platform identification

---

## False Positive Considerations

False positives are unlikely in normal enterprise environments.
If triggered, analysts should verify:

- Binary origin and file location
- Execution context
- User activity at time of execution

---

## Severity Justification

This detection is marked as **High** severity due to the direct risk
credential-dumping tools pose to enterprise security.
