---
title: "Worklog Week 8"
date: 2026-06-22
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Objectives for Week 8:

* Initialize the Amazon SNS service to send instant incident email notifications to the management team.
* Develop Lambda functions for automated incident response: `sendAlert` (send notifications) and `blockIP` (block IP on Network ACL).
* Enforce all IAM Policy permissions according to the Principle of Least Privilege.


### Tasks to Implementation This Week:

| Day | Task | Start Date | Completion Date | Reference Resource |
| :---: | :--- | :---: | :---: | :--- |
| **Mon** | - Create Amazon SNS Topic `NetworkAlertTopic` for urgent alert broadcasting.<br>- Subscribe Admin Email to receive notifications from SNS Topic and confirm the automatic activation link. | 22/06/2026 | 22/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **Tue** | - Develop Lambda Action Group `sendAlert` in Python using the `boto3` library.<br>- Format alert email content: Attacking IP address, detection timestamp, and severity level. | 23/06/2026 | 23/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **Wed** | - Develop Lambda Action Group `blockIP` invoking EC2 Client's `create_network_acl_entry` API.<br>- Configure insertion of DENY rule with top priority on Network ACL to instantly sever malicious IP connections. | 24/06/2026 | 24/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **Thu** | - Audit all IAM Roles across Lambda, Bedrock Agent, S3, and DynamoDB services.<br>- Remove overly broad access rights, replacing them with granular Inline Policies adhering to Least Privilege. | 25/06/2026 | 25/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **Fri** | - Conduct standalone testing of the `blockIP` function using simulated IP addresses on the Lambda Console.<br>- Verify Network ACL table to confirm automated DENY rule creation and check admin inbox for successful SNS email delivery. | 26/06/2026 | 26/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |

### Achievements in Week 8:

* Completed 2 core automated incident response tools: `sendAlert` and `blockIP`.
* Successfully established real-time emergency notification channels via Amazon SNS.
* Standardized and hardened IAM Role authorization infrastructure, ensuring absolute security for the Serverless ecosystem.