---
title : "Build the Data Processing Lambda"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

This Lambda is the bridge between the Networking/Data Collection layers and the AI Agent — it parses raw Flow Logs, computes traffic metrics, and stores them in DynamoDB for the Agent to later query.

#### Create the DynamoDB table

1. Open the [DynamoDB console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#tables) → **Create table**.
+ Table name: ```netmon-app-traffic-stats```
+ Partition key: ```sourceIp``` (String)
+ Sort key: ```timeBucket``` (String) — groups traffic into 5-minute buckets per IP, avoiding a hot partition if one IP floods the system
+ Capacity mode: **On-demand**


2. After creation, go to **Additional settings** → enable **Point-in-time recovery (PITR)** and confirm **Encryption** uses an AWS managed key.


#### Create the Lambda function

3. Open the [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/functions) → **Create function**.
+ Function name: ```netmon-infra-preprocess-logs```
+ Runtime: **Python 3.12**


4. In **Configuration → Environment variables**, add:
+ `SYSTEM_PAUSED` = `true` (safety switch — always keep this `true` unless actively testing)

![env vars](/images/5-Workshop/5.6-AI-Agent/env-vars.png)


6. Write the function logic: parse each Flow Log record from the S3 object, aggregate byte/packet counts per `sourceIp` and 5-minute `timeBucket`, and write the result via `dynamodb:PutItem`.

{{% notice tip %}}
Skip processing entirely and return immediately if `SYSTEM_PAUSED == "true"` — this single check prevents accidental runs from affecting your data during unrelated S3 operations (e.g. re-uploading files).
{{% /notice %}}

7. Go back to your S3 bucket's **Event notification** (configured in section 5.4.4) and finish selecting this Lambda as the destination.


8. Test: wait for a new Flow Log file to arrive (or trigger manually via **Test** tab with a sample S3 event), then confirm a new item appears in ```netmon-app-traffic-stats```.

![ddb item](/images/5-Workshop/5.6-AI-Agent/ddb-item.png)