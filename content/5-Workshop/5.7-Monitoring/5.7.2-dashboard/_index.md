---
title : "Build the CloudWatch Dashboard"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.7.2 </b> "
---

A dashboard gives the security team one place to observe system health and detection activity in real time.

1. Open the [CloudWatch console](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#dashboards) → **Dashboards** → **Create dashboard**.
+ Name: ```NetMon-Dashboard```

2. Add widgets covering:
+ **Lambda metrics**: invocations, errors, duration for all 5 functions
+ **DynamoDB metrics**: read/write capacity for both tables
+ **SNS metrics**: number of messages published
+ **Anomaly trend**: count of detections over time
+ **Severity breakdown**: MEDIUM vs HIGH classification
+ **Alert history**: recent alerts sent
+ **IP blocking history**: recent block/log-only actions

![dashboard widgets](/images/5-Workshop/5.7-Monitoring/dashboard-widgets.jpg)

3. For log-based widgets (anomaly trend, severity, history), use **Logs Insights** queries against each Lambda's log group and pin the results as a widget.

{{% notice note %}}
If a Logs Insights widget shows "No results" right after a test, it's often just the selected time range being too wide, causing lag. Narrow it to the last 5-30 minutes when checking a recent test.
{{% /notice %}}

4. Save the dashboard and confirm all widgets populate with real data after running a test scan.

![dashboard final](/images/5-Workshop/5.7-Monitoring/dashboard-final.jpg)

5. (Optional, documented as future direction) Add **CloudWatch Alarms** on Lambda error rate and DLQ message count, wired to the SNS topic — this shifts monitoring from passive (someone has to open the dashboard) to active (the system notifies you automatically).