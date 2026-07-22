---
title : "Prepare the S3 bucket"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Before enabling VPC Flow Logs, you need a destination bucket to receive them.

1. Open the [Amazon S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1)
2. Click **Create bucket**.
+ Bucket name: ```netmon-infra-flowlogs-8425``` (bucket names must be globally unique — append your own suffix if needed)
+ Region: **Asia Pacific (Singapore) ap-southeast-1**
+ Block Public Access settings: leave all 4 boxes **checked** (bucket must never be public)
3. Scroll down to **Default encryption** → choose **Server-side encryption with AWS Key Management Service keys (SSE-KMS)** → **AWS KMS key**: `Use AWS managed key (aws/s3)` → enable **Bucket Key**.
4. Click **Create bucket**. Confirm it appears in your bucket list.
{{% notice tip %}}
Enabling encryption at this step means every Flow Log object written later is automatically protected — no extra work needed afterward.
{{% /notice %}}