---
title : "Test the Full Autonomous Flow"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.6.7 </b> "
---

With all 4 Action Groups connected, it's time to validate the entire chain end-to-end using real attack traffic.

1. From your local machine, run a port scan against your target instance:
```
nmap -sV <target-public-ip>
```

2. Watch the flow: VPC Flow Logs → S3 → `preprocess-logs` → DynamoDB → (if flagged) invoke Agent → `getNetworkMetrics` → `checkTrafficAnomaly` → (based on severity) `sendAlert` and/or `blockIP`.

3. Open **CloudWatch Logs** for each Lambda in order to trace the execution and confirm each step ran correctly.

![cloudwatch trace](/images/5-Workshop/5.6-AI-Agent/cloudwatch-trace.png)

4. Confirm you receive a real email alert from SNS.

![email alert](/images/5-Workshop/5.6-AI-Agent/final-email-alert.png)

5. Check ```netmon-app-agent-decisions``` in DynamoDB — you should see a new audit record with the full decision trail (severity, action taken, timestamp).

![audit record](/images/5-Workshop/5.6-AI-Agent/audit-record.png)

6. Repeat the same scan a second time within 60 minutes and confirm the deduplication logic correctly suppresses a duplicate alert (or escalates severity, per your design).

{{% notice tip %}}
If nothing happens after a scan, check `SYSTEM_PAUSED` on `preprocess-logs` first — this is the single most common reason the pipeline appears "stuck" during testing.
{{% /notice %}}

Congratulations — you now have a fully autonomous, AI-driven network threat detection and response pipeline running end-to-end on AWS.