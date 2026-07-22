---
title : "Source Code"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.10. </b> "
---

The full technical source code backing this system is maintained in a separate repository, so this report stays easy to read while the code stays clean and browsable on its own.

#### What's included

- **Lambda functions** — full source for all 5 functions (`preprocess-logs`, `getNetworkMetrics`, `checkTrafficAnomaly`, `sendAlert`, `blockIP`)
- **IAM policies** — least-privilege policy JSON for each Lambda's execution role
- **Bedrock Agent configuration** — agent instructions, Guardrail settings, Action Group schemas

📂 **[View full source code on GitHub](https://github.com/Twannatic/netmon-source-code)**