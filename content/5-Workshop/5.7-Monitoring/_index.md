---
title : "Monitoring & Notification"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Building the Monitoring Layer

The final layer gives visibility into the entire pipeline — a dashboard to observe system behavior, and a Dead Letter Queue to catch any failed asynchronous events so nothing is silently lost.

![overview](/images/5-Workshop/5.7-Monitoring/diagram5.png)

#### Content

- [Set up the Dead Letter Queue](5.7.1-dlq/)
- [Build the CloudWatch Dashboard](5.7.2-dashboard/)