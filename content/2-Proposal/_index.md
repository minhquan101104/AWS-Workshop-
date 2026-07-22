---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# AI Agent-based Network Security Monitoring System on AWS
## An Autonomous Serverless Solution for Real-Time Network Threat Detection and Response

### 1. Executive Summary
The Network Security Monitoring Platform is an autonomous threat detection and response system built entirely on AWS Serverless services. It captures network traffic via VPC Flow Logs, uses Amazon Bedrock Agent to reason over the data, and automatically classifies severity, deduplicates alerts, escalates repeated attacks, and executes defensive actions (email alerts, IP blocking via Network ACL). The system reduces manual SOC (Security Operations Center) workload and shortens detection-to-response time from hours to seconds.

### 2. Problem Statement
### What's the Problem?
Traditional network monitoring relies on human analysts to review logs, correlate events, and manually decide on a response. This does not scale, is prone to alert fatigue, and results in delayed reaction to active threats such as port scanning or brute-force attempts.

### The Solution
The platform uses VPC Flow Logs to capture ACCEPT/REJECT traffic, Amazon S3 as the raw log data lake, AWS Lambda for event-driven preprocessing, and Amazon DynamoDB to store traffic statistics and audit trails. An Amazon Bedrock Agent (Amazon Nova Pro) autonomously orchestrates four Action Group tools to analyze traffic, detect anomalies (with whitelist, deduplication, and escalation logic), send alerts via Amazon SNS, and block malicious IPs through Network ACL. Amazon Bedrock Guardrails protects the Agent against prompt-injection attempts.

### Benefits and Return on Investment
The solution removes the need for constant manual log review, cuts detection-to-alert time to near real-time, and creates a reusable reference architecture for AI-driven security automation. Being fully serverless, the platform incurs cost only when actively processing events, keeping monthly operating cost low while remaining scalable if traffic volume grows.

### 3. Solution Architecture
The platform runs in a dedicated VPC (`netmon-infra-vpc`, 10.0.0.0/16) containing a Public Subnet with a single EC2 instance acting as the monitored target. All data processing components (Lambda, DynamoDB, Bedrock Agent, SNS, SQS) are intentionally **not VPC-attached**, since they only interact with managed AWS services via the AWS Public API and IAM — avoiding unnecessary NAT Gateway cost and cold-start latency.

![NetMon Architecture Overview](/images/2-Proposal/netmon_architecture.png)

### AWS Services Used
- **Amazon VPC / EC2**: Hosts the monitored target instance inside a Public Subnet with restricted Security Group rules.
- **VPC Flow Logs**: Captures ACCEPT + REJECT traffic at 1-minute aggregation.
- **Amazon S3**: Stores raw Flow Logs as the ingestion data lake (SSE-KMS encrypted).
- **AWS Lambda**: 5 functions — `preprocess-logs`, `getNetworkMetrics`, `checkTrafficAnomaly`, `sendAlert`, `blockIP`.
- **Amazon DynamoDB**: `traffic-stats` (network metrics) and `agent-decisions` (audit trail) tables, KMS-encrypted with Point-in-Time Recovery enabled.
- **Amazon Bedrock Agent**: Central AI orchestrator (Amazon Nova Pro model) reasoning over 4 Action Group tools.
- **Amazon Bedrock Guardrails**: Blocks prompt-injection and system-prompt exploitation attempts.
- **Amazon SNS**: Delivers email alerts to the security team.
- **Amazon SQS**: Dead Letter Queue protecting the asynchronous S3-triggered preprocessing Lambda.
- **Amazon CloudWatch**: Dashboard (8 widgets) for logs, metrics, and operational visibility.
- **AWS IAM**: Least-privilege roles scoped per Lambda function.
- **AWS Shield Standard**: Baseline DDoS protection (automatic, no configuration required).

### Component Design
- **Target Instance**: EC2 in the Public Subnet intentionally exposed to capture real attack traffic for detection testing.
- **Data Ingestion**: VPC Flow Logs → S3 → S3 Event Notification triggers `preprocess-logs` Lambda.
- **Data Storage**: DynamoDB stores per-IP traffic statistics and every Agent decision as an immutable audit trail.
- **AI Reasoning**: Bedrock Agent autonomously calls `getNetworkMetrics` and `checkTrafficAnomaly`, then decides whether to call `sendAlert` and/or `blockIP` based on severity.
- **Response Layer**: `sendAlert` publishes to SNS; `blockIP` writes a Network ACL entry when `REAL_BLOCK_ENABLED=true` (defaults to safe logged-only mode).
- **Observability**: CloudWatch Dashboard visualizes anomaly trends, severity classification, alert history, and IP-blocking history.

### 4. Technical Implementation
**Implementation Phases**
- Phase 1 — Network & Ingestion: Design VPC, deploy target EC2, configure VPC Flow Logs and S3 data lake.
- Phase 2 — Data Pipeline: Build `preprocess-logs` Lambda, design DynamoDB schema, validate end-to-end ingestion.
- Phase 3 — AI Agent: Research Bedrock Agent, design and implement all 4 Action Group Lambdas, connect to the Agent.
- Phase 4 — Hardening & Reporting: Apply AWS Well-Architected review, enforce least-privilege IAM, add DLQ, enable KMS encryption and PITR, finalize documentation.

**Technical Requirements**
- Practical knowledge of VPC networking, IAM least-privilege policy design, event-driven Lambda architecture, DynamoDB single-table design, and Amazon Bedrock Agent/Action Group configuration.
- Testing tools: `nmap` for port-scan simulation, manual SSH brute-force simulation, and traffic-spike testing to validate detection logic.

### 5. Timeline & Milestones
**Project Timeline** (~11-12 weeks, 05/05/2026 – 20/07/2026)
- Weeks 1-3: Onboarding, self-selected topic proposal, initial network design (VPC, EC2, Security Group).
- Weeks 4-5: VPC Flow Logs, S3 data lake, ingestion pipeline (Lambda + DynamoDB).
- Weeks 6-8: Bedrock Agent research and implementation of all 4 Action Groups.
- Weeks 9-10: Guardrails, CloudWatch Dashboard, realistic attack scenario testing.
- Weeks 11-12: AWS Well-Architected review, security hardening, final report.

### 6. Budget Estimation
The platform is fully serverless and event-driven, so cost scales with actual traffic volume rather than fixed infrastructure. Primary cost drivers are:
- **Amazon Bedrock Agent invocations** (charged per token, largest variable cost).
- **AWS Lambda** invocations and duration (within Free Tier for typical lab-scale traffic).
- **Amazon DynamoDB** on-demand read/write capacity.
- **Amazon S3** storage for raw Flow Logs (with lifecycle rules recommended for cost control).
- **EC2** t3.micro (Free Tier eligible for the first 12 months).

An exact AWS Pricing Calculator estimate depends on final traffic volume and Bedrock invocation frequency during testing; a detailed cost breakdown will be attached as a separate estimation file once testing volume is finalized.

### 7. Risk Assessment
#### Risk Matrix
- Bedrock Agent quota/rate limits: Medium impact, medium probability (new accounts have lower default quotas).
- False positives/negatives in anomaly detection: Medium impact, medium probability.
- Accidental real IP blocking during testing: High impact, low probability (mitigated by `REAL_BLOCK_ENABLED=false` default).
- Single point of failure (Single-AZ): Medium impact, low probability during a short-term lab project.
- No WAF in front of the target instance: intentional trade-off (see Section 9), compensated by Shield Standard and restrictive Security Group/NACL rules.

#### Mitigation Strategies
- Quota: Fallback model selection, request throttling awareness.
- Detection accuracy: Whitelist + deduplication + escalation logic to reduce noise.
- Blocking safety: Safe-by-default logged-only mode; real blocking requires explicit opt-in.
- Reliability: Dead Letter Queue on the asynchronous ingestion Lambda to prevent silent event loss.

#### Contingency Plans
- `SYSTEM_PAUSED` kill-switch to halt the entire pipeline instantly if unexpected behavior occurs.
- Manual review of DynamoDB audit trail if automated response needs to be verified or reverted.

### 8. Security Considerations
- **No WAF in front of the target instance**: an intentional design decision, since the target functions as a honeypot to capture real attack traffic — filtering it through WAF would defeat that purpose. Compensating controls: AWS Shield Standard (automatic baseline DDoS protection), Security Group egress deny-by-default, and no IAM Instance Profile attached to the target instance to limit blast radius if compromised.
- **Encryption at rest**: S3 (SSE-KMS) and both DynamoDB tables (AWS managed KMS key) are encrypted, with Point-in-Time Recovery enabled on DynamoDB.
- **Least-privilege IAM**: all 5 Lambda functions run under scoped, customer-managed policies with no `*FullAccess` managed policies attached.

### 9. Known Limitations & Future Direction

The following are architectural trade-offs made deliberately due to internship time constraints, disclosed here for transparency rather than left undiscovered:

- **No AWS WAF in front of the target instance**: intentional — filtering traffic through WAF would require an ALB and would reduce visibility to HTTP/HTTPS only, defeating the goal of capturing raw port-scan traffic. Compensating controls: AWS Shield Standard (automatic), Security Group egress deny-by-default, no IAM Instance Profile on the target instance.
- **Dead Letter Queue coverage**: only the S3-triggered `preprocess-logs` Lambda uses true asynchronous invocation, where DLQ semantics apply correctly. The four Action Group Lambdas are invoked synchronously by the Bedrock Agent; errors there are handled through the Agent's own retry/orchestration logic rather than DLQ.
- **Synchronous Agent invocation**: `preprocess-logs` currently calls the Bedrock Agent synchronously (`[sync-blocking]`). Decoupling this via Amazon EventBridge + SQS would better follow serverless best practices and improve resilience under load.
- **DynamoDB query pattern**: current Lambdas use `Scan()` rather than `Query()` with a Global Secondary Index. This is acceptable at current data volume but would need to be optimized before scaling to production traffic levels.
- **Single Availability Zone**: acceptable for a time-boxed internship project; a production deployment would require Multi-AZ EC2/Auto Scaling and DynamoDB Global Tables for higher availability.
- **No Infrastructure as Code**: the entire environment was provisioned manually through the AWS Console for learning purposes. Migrating to AWS CDK or Terraform is a natural next step for reproducibility and safer iteration.

**Planned next steps**: add CloudWatch Alarms for proactive (rather than dashboard-only) monitoring, integrate AWS X-Ray for distributed tracing across the multi-Lambda pipeline, and evaluate Amazon SageMaker for a machine-learning-based anomaly model to reduce reliance on fixed thresholds.

### 10. Expected Outcomes
#### Technical Improvements
Near real-time autonomous detection and response replacing manual log review.
A reusable reference architecture for AI Agent-driven security automation on AWS.
#### Long-term Value
A working demonstration of applying generative AI (Amazon Bedrock Agent) to a real operational security use case, extensible to GuardDuty integration, Multi-AZ, and Infrastructure as Code in future iterations.