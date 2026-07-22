---
title : "Set up the Dead Letter Queue"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

Asynchronous Lambda invocations (like S3-triggered `preprocess-logs`) can fail silently after exhausting retries. A Dead Letter Queue catches these failed events so you can investigate instead of losing them.

1. Open the [Amazon SQS console](https://ap-southeast-1.console.aws.amazon.com/sqs/v3/home?region=ap-southeast-1#/queues) → **Create queue**.
+ Type: **Standard**
+ Name: ```netmon-dlq```

![create queue](/images/5-Workshop/5.7-Monitoring/create-dlq.jpg)

2. Attach it to `preprocess-logs`: open the Lambda → **Configuration → Asynchronous invocation** → **Edit** → **On failure** → **SQS queue** → select ```netmon-dlq```.

3. Make sure the Lambda's IAM role has `sqs:SendMessage` permission scoped to this queue's ARN — this is easy to forget and causes the DLQ to silently fail even when correctly configured on the Lambda side.

4. Confirm the destination is set correctly.

![dlq attached](/images/5-Workshop/5.7-Monitoring/dlq-attached.jpg)

{{% notice tip %}}
Check the queue's **Messages available** metric periodically (or set an alarm on it, next section) — a non-zero count means something is failing upstream and needs attention.
{{% /notice %}}