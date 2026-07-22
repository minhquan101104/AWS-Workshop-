---
title : "Action Group 2: checkTrafficAnomaly"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.6.4 </b> "
---

This is the most complex Lambda in the system — it decides whether traffic is actually a threat, with three layers of intelligence: whitelist filtering, alert deduplication, and severity escalation.

#### Build the Lambda logic

1. Create Lambda ```checkTrafficAnomaly``` (Python 3.12), with least-privilege access to `Scan`/`Query` on both ```netmon-app-traffic-stats``` and ```netmon-app-agent-decisions```.

2. Implement the detection logic in this order:
+ **Whitelist check**: skip known-safe IPs (e.g. internal team IPs) entirely — return `whitelistedSkipped: true`
+ **Anomaly threshold**: flag if request count/pattern exceeds a defined threshold within the time window
+ **Deduplication (60 min)**: check ```netmon-app-agent-decisions``` for a recent alert on the same IP within the last 60 minutes — if found, return `skippedDuplicates: true` instead of re-alerting
+ **Escalation**: if this is the 2nd+ detection for the same IP, escalate severity to `HIGH` and set `recommendBlock: true`
+ **Distributed scan detection**: optionally check if multiple source IPs are hitting the target in a short window (coordinated scan pattern)

3. Return a concise JSON summary (not raw data) to keep the Agent's token usage low:
```json
{"anomalyDetected": true, "severity": "HIGH", "recommendBlock": true, "whitelistedSkipped": false, "skippedDuplicates": false}
```

#### Register the Action Group

4. In your Agent → **Action groups** → **Add**.
+ Action group name: ```anomalyActions```
+ Function name: ```checkTrafficAnomaly```, parameter `sourceIp` (string, required)

5. **Save and exit** → **Prepare**.

6. Test with a repeated-attack scenario: confirm that when the same source IP is flagged multiple times within 60 minutes, the `details` field records the exact repeat count and automatically recommends blocking once the threshold is exceeded — demonstrating both the frequency-tracking and severity-escalation logic working correctly.

![escalation evidence](/images/5-Workshop/5.6-AI-Agent/checkanomaly-test.png)