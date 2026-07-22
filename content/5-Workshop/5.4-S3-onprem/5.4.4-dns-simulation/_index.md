---
title : "Configure S3 Event Notification"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

The final step of the Data Collection Layer is wiring S3 to automatically trigger processing whenever a new Flow Log file arrives — this is what connects this layer to the Lambda-based Data Processing Layer covered next.

1. Open the S3 console → select ```netmon-infra-flowlogs-8425``` → go to the **Properties** tab.
2. Scroll to **Event notifications** → click **Create event notification**.
+ Event name: ```netmon-flowlog-trigger```
+ Event types: **All object create events** (```s3:ObjectCreated:*```)
+ Destination: **Lambda function** (you will create and select the `preprocess-logs` function in the next section — you can leave this step and return to complete it once the Lambda exists)
{{% notice note %}}
It's normal to pause here — S3 Event Notification requires the target Lambda to already exist. Continue to the next section to build it, then return here to finish wiring the trigger.
{{% /notice %}}