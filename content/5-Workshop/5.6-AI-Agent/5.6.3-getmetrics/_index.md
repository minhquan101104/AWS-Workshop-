---
title : "Action Group 1: getNetworkMetrics"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

This is the Agent's "eyes" — a read-only tool that retrieves traffic statistics for a given source IP from DynamoDB.

#### Build the Lambda

1. Create a new Lambda function ```getNetworkMetrics``` (Python 3.12).
2. Attach a least-privilege IAM role with only `dynamodb:GetItem` / `dynamodb:Query` on ```netmon-app-traffic-stats```.
3. The function must accept the Agent's invocation event and return a response in Bedrock's required format:

```json
{
    "messageVersion": "1.0",
    "response": {
        "actionGroup": "networkActions",
        "function": "getNetworkMetrics",
        "functionResponse": {
            "responseBody": {
                "TEXT": {
                    "body": "{\"sourceIp\": \"x.x.x.x\", \"requestCount\": 42, \"lastSeen\": \"...\"}"
                }
            }
        }
    }
}
```

{{% notice tip %}}
Getting this response shape wrong is the most common integration bug — the Agent will silently fail to parse the tool's output if `functionResponse.responseBody.TEXT.body` isn't an exact match to this structure.
{{% /notice %}}


#### Register it as an Action Group

4. Open your Agent → **Action groups** → **Add**.
+ Action group name: ```networkActions```
+ Action group type: **Define with function details**
+ Function name: ```getNetworkMetrics```, with a clear description and one parameter `sourceIp` (string, required)
+ Select the Lambda function you just created


5. Click **Save and exit**, then **Prepare** the Agent to apply changes.

6. Test in the Agent Builder chat panel: ask something like *"What's the traffic history for IP 203.0.113.5?"* and confirm the Agent calls the tool and returns data.

![test in builder](/images/5-Workshop/5.6-AI-Agent/getmetrics-test.png)