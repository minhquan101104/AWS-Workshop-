---
title : "Create the Bedrock Agent"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

The Bedrock Agent is the reasoning core of the system — it decides which tools to call and when, based on the data it receives.

1. Open the [Amazon Bedrock console](https://ap-southeast-1.console.aws.amazon.com/bedrock/home?region=ap-southeast-1#/agents) → **Agents** → **Create Agent**.
+ Agent name: ```netmon-security-agent```
+ Description: brief summary of its role (autonomous network threat detection and response)


2. Under **Agent resource role**, let Bedrock create a new service role automatically.

3. Under **Select model**, choose a foundation model.

{{% notice note %}}
If you hit a `429 rate limit` error on a newly created account, this usually means the default model quota hasn't been raised yet. Switching to **Amazon Nova Pro** is a practical workaround while requesting a quota increase for other models.
{{% /notice %}}


4. In **Instructions for the Agent**, write a clear system prompt describing its role, e.g.:
> *"You are a network security analyst agent. When given network traffic data, use your tools to retrieve metrics, check for anomalies, and — based on severity — send alerts and/or block malicious IPs. Always check anomaly status before taking any action."*


5. Click **Save**, then **Create**. You now have an Agent with no tools yet — the next sections will add its 4 Action Groups.


6. Create an **Agent Alias** (e.g. ```production```) — this is the stable endpoint your Lambdas will invoke, decoupled from draft versions you edit during development.