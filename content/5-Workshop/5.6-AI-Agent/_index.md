---
title : "AI Agent Layer"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Building the AI Agent Layer

This is the core of the entire system: an **Amazon Bedrock Agent** that autonomously reasons over network data and orchestrates four Action Group tools to detect and respond to threats — without a human in the loop.

![overview](/images/5-Workshop/5.6-AI-Agent/diagram4.png)

#### Content

- [Build the Data Processing Lambda (preprocess-logs)](5.6.1-preprocess-lambda/)
- [Create the Bedrock Agent](5.6.2-create-agent/)
- [Build Action Group 1: getNetworkMetrics](5.6.3-getmetrics/)
- [Build Action Group 2: checkTrafficAnomaly](5.6.4-checkanomaly/)
- [Build Action Group 3: sendAlert](5.6.5-sendalert/)
- [Build Action Group 4: blockIP](5.6.6-blockip/)
- [Test the full autonomous detection flow](5.6.7-test-agent/)