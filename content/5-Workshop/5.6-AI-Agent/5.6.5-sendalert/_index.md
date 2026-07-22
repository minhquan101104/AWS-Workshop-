---
title : "Action Group 3: sendAlert"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.6.5 </b> "
---

This tool notifies the security team and writes an audit record of the Agent's decision.

#### Create the SNS Topic

1. Open the [Amazon SNS console](https://ap-southeast-1.console.aws.amazon.com/sns/v3/home?region=ap-southeast-1#/topics) → **Create topic**.
+ Type: **Standard**
+ Name: ```netmon-app-alerts```


2. Create a subscription: Protocol **Email**, enter the security team's email address. Confirm the subscription via the email link sent to that address.


#### Build the Lambda

3. Create Lambda ```netmon-tool-sendAlert``` (Python 3.12), accepting parameters `attackType`, `sourceIp`, `alertLevel`, `details` from the Agent.
+ IAM permissions: `sns:Publish` on the topic, `dynamodb:PutItem` on ```netmon-app-agent-decisions``` (writing the audit record)


4. The function publishes a formatted message to SNS, then writes a record to ```netmon-app-agent-decisions``` (partition key `eventId`) capturing: timestamp, sourceIp, severity, action taken — this becomes your audit trail.

#### Register the Action Group

5. In your Agent → **Action groups** → **Add**.
+ Action group name: ```sendAlert```
+ Function name: ```sendAlert```, parameters: `attackType`, `sourceIp`, `alertLevel`, `details` (all string, required)


6. **Save and exit** → **Prepare**. Test and confirm you receive a real email alert.

![email received](/images/5-Workshop/5.6-AI-Agent/email-received.png)