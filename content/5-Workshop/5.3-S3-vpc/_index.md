---
title : "Networking Layer"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Building the Networking Layer

In this section, you will build the foundational network for the monitoring system: a dedicated VPC, a Public Subnet, and a target EC2 instance that will be intentionally exposed to capture real network traffic. This traffic is what the rest of the pipeline (VPC Flow Logs, Lambda, Bedrock Agent) will analyze in later sections.

![overview](/images/5-Workshop/5.3-S3-vpc/diagram2.png)

#### Content

- [Create VPC and target EC2 instance](5.3.1-create-vpc-ec2/)
- [Test connectivity to the target instance](5.3.2-test-connectivity/)