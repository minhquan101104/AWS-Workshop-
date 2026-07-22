---
title: "Event 2"
date: 2026-07-15
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: “CLOUD ARCHITECT Grand Final – AWS Tech Talk: SLA Monitoring, CLF-C02 Certification Strategy & AI Security Agent Integration”

### 1. Event Overview

* **Topic:** AWS Cloud Architecture, Security Automation & Certification Roadmap.
* **Program:** Specialized Seminar Series under the **First Cloud AI Journey** project.
* **Speakers List:**
  * **Nguyen Huynh Son** - Infrastructure Support Engineer at Endava / Ex-SPS, AWS Student Builder Group HUFLIT.
  * **Ngo Le Tan Huy** - Speaker & AWS Cloud Practitioner Specialist.
  * **Nguyen Tuan Thinh** - DevOps/DevSecOps/Cloud Engineer at Styl Solutions, First Cloud AI Journey.

---

### 2. Core Technical Content & Key Takeaways

#### Topic 1: SLA and Monitoring – From SLA to Monitoring What Really Matters (Speaker: Nguyen Huynh Son)
* **SLA Concept & Role:** Formal service level commitment between provider and customer. Supports risk management, performance measurement, and clear accountability.
* **The Gap "Healthy Infrastructure ≠ Happy User Experience":** A "green" infrastructure status (CPU 18%, ALB HealthCheck OK) does not guarantee user actions are successful (e.g., database connection loss causing login failures).
* **Monitoring Pyramid Model:** Requires multi-tiered observability spanning *Cloud Provider → Infrastructure → Application → Business Metrics → Customer Experience*.
* **Risk Loop Workflow:** *Identify Risk → Monitor Signals → Respond (SNS, SOP) → Improve*.

#### Topic 2: Inside The Exam – AWS Cloud Practitioner (CLF-C02) (Speaker: Ngo Le Tan Huy)
* **Exam Structure:** 65 multiple-choice questions in 90 minutes (+30 minutes for non-native English speakers), passing score 700/1000.
* **Breakdown across 4 Domains:**
  1. *Cloud Concepts (24%)*: 6 Cloud Benefits, AWS WAF, AWS CAF.
  2. *Security & Compliance (30%)*: Shared Responsibility Model, IAM (Least Privilege), Security Groups vs NACLs.
  3. *Cloud Technology & Services (34%)*: EC2, S3, EBS, EFS, RDS, DynamoDB, VPC, Route 53.
  4. *Billing, Pricing & Support (12%)*: EC2 pricing models, Cost Explorer, Support Plans.
* **Preparation & Exam Strategies:** Elimination technique, Keyword Thinking mapping, and hands-on practice using AWS Free Tier.

#### Topic 3: Securing Your Web Apps With AWS Security Agent (Speaker: Nguyen Tuan Thinh)
* **Solving Security Bottlenecks:** Manual penetration testing takes weeks and carries high costs ($5k - $20k per engagement).
* **Power of Frontier Agent (Amazon Bedrock):** Autonomous planning and comprehensive security execution: *Design Review (Architecture) → Code Review (Git PRs) → Automated Pentesting (Real-world attacks)*.
* **Real-world Cost & Limitations:** Delivers significant cost savings compared to traditional pentesting; however, the Agent can be blocked by MFA/Biometrics and is not yet fully optimized for complex business logic flaws.

---

### 3. Key Takeaways & Practical Project Application

* **Building True Observability:** Integrate Custom Metrics (such as *Login Failure Rate*) combined with CloudWatch Alarms and SNS Topics rather than relying purely on CPU/RAM metrics.
* **Applying AI Agents to Security:** Clearly recognized the potential of Generative AI (Amazon Bedrock) in automating vulnerability assessments and incident response.
* **Certification Roadmap:** Gained a structured methodology to systemize AWS Cloud knowledge in preparation for earning the AWS Certified Cloud Practitioner certification.

#### Event Gallery

{{< figure src="/images/event2.jpg" title="AWS Seminar Presentation 1" >}}
{{< figure src="/images/event2.1.jpg" title="AWS Seminar Presentation 2" >}}

> **Summary:** The seminar provided rich practical knowledge, closely connecting infrastructure operations (Monitoring/SLA), structured knowledge validation (Certification), and cutting-edge security trends with Generative AI.