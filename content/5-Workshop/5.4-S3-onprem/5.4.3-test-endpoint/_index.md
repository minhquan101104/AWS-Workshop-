---
title : "Verify Flow Logs delivery"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

Flow Logs can take a few minutes to start delivering. Use the traffic you generated in the earlier connectivity test (curl / nmap) to confirm data is flowing correctly.

1. Open the S3 console → navigate into ```netmon-infra-flowlogs-8425```.
2. Drill down through the auto-generated folder structure (```AWSLogs/<account-id>/vpcflowlogs/ap-southeast-1/<year>/<month>/<day>/```) until you find `.gz` log files.

![s3 folder structure](/images/5-Workshop/5.4-S3-onprem/s3-folders.png)

3. Download and open one file to confirm it contains real traffic records, including entries from the earlier nmap/curl test.
{{% notice tip %}}
If no files appear after 5-10 minutes, double-check the Flow Log status is **Active** and that the correct VPC (not just the subnet) was selected in the previous step.
{{% /notice %}}