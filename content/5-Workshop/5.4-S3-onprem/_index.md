---
title : "VPC Flow Logs & S3 Data Lake"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Building the Data Collection Layer

In this section, you will configure **VPC Flow Logs** to capture all network traffic (ACCEPT + REJECT) hitting the target EC2 instance, and set up **Amazon S3** as the raw log data lake. This is the entry point of the entire detection pipeline — every downstream Lambda and the Bedrock Agent ultimately depend on data captured here.

![overview](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

#### Content

- [Prepare the S3 bucket](5.4.1-prepare/)
- [Enable VPC Flow Logs](5.4.2-create-interface-enpoint/)
- [Verify Flow Logs delivery](5.4.3-test-endpoint/)
- [Configure S3 Event Notification](5.4.4-dns-simulation/)