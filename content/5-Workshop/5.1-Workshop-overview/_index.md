---
title : "Introduction"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### AI Agent-based Network Security Monitoring
+ Traditional network monitoring requires security analysts to manually review logs, correlate events across multiple sources, and decide on a response — a process that does not scale and is prone to delayed reaction against active threats.
+ **Amazon Bedrock Agent** enables a generative-AI model to autonomously reason over network telemetry, call specialized tools (Action Groups) to investigate further, and decide on an appropriate response — without a human in the loop for routine detection and triage.

#### Workshop overview
In this workshop, you will build a serverless, AI-driven network security monitoring system on AWS from the ground up.

+ **Data Collection Layer**: A VPC with a target EC2 instance generates real network traffic, captured by **VPC Flow Logs** and stored in an **Amazon S3** data lake.
+ **Data Processing Layer**: An **AWS Lambda** function triggered by S3 Events parses the raw logs and stores per-IP traffic statistics in **Amazon DynamoDB**.
+ **AI Agent Layer**: An **Amazon Bedrock Agent** autonomously calls four Action Group tools — read network metrics, detect anomalies (with whitelist, deduplication, and severity escalation), send alerts via **Amazon SNS**, and block malicious IPs via **Network ACL**.
+ **Security Layer**: **Amazon Bedrock Guardrails** protects the Agent from prompt-injection attempts, and every Lambda runs under a least-privilege **IAM** role.

By the end of this workshop, you will have a working end-to-end pipeline that can detect a simulated port scan or brute-force attack, autonomously classify its severity, and respond — all without manual intervention.

![NetMon Architecture Overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)