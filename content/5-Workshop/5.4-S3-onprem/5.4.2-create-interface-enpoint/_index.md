---
title : "Enable VPC Flow Logs"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

1. Open the [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#vpcs:) → select ```netmon-infra-vpc```.
2. Go to the **Flow logs** tab → click **Create flow log**.
+ Name: ```netmon-infra-flowlog```
+ Filter: **All** (captures both ACCEPT and REJECT traffic — essential for detecting blocked scan attempts)
+ Maximum aggregation interval: **1 minute** (fastest delivery, so the pipeline detects threats sooner)
3. Destination: choose **Send to an Amazon S3 bucket**.
+ S3 bucket ARN: browse and select ```netmon-infra-flowlogs-8425```
+ Log record format: **AWS default format** is sufficient for this project
4. Click **Create flow log**. Confirm its status becomes **Active**.

![flow log active](/images/5-Workshop/5.4-S3-onprem/flowlog-active.png)