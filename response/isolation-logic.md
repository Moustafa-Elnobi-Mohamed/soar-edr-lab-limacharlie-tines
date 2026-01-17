# Isolation Decision Logic

When a detection is received, the SOAR workflow prompts the analyst
to approve or deny endpoint isolation.

- YES → Endpoint is isolated using LimaCharlie
- NO → Alert remains open for further investigation

This approach balances automation with operational safety
and prevents unnecessary service disruption.
