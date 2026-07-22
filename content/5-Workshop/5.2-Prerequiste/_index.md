---
title : "Prerequisite"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Add the following IAM permission policy to your user account to build and clean up this workshop's resources.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "cloudwatch:*",
                "logs:*",
                "ec2:AllocateAddress",
                "ec2:AssociateAddress",
                "ec2:AssociateRouteTable",
                "ec2:AttachInternetGateway",
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateFlowLogs",
                "ec2:CreateInternetGateway",
                "ec2:CreateNetworkAcl",
                "ec2:CreateNetworkAclEntry",
                "ec2:CreateRoute",
                "ec2:CreateRouteTable",
                "ec2:CreateSecurityGroup",
                "ec2:CreateSubnet",
                "ec2:CreateTags",
                "ec2:CreateVpc",
                "ec2:CreateVpcEndpoint",
                "ec2:Describe*",
                "ec2:ModifyInstanceAttribute",
                "ec2:ModifyVpcAttribute",
                "ec2:ReleaseAddress",
                "ec2:ReplaceRoute",
                "ec2:RevokeSecurityGroupEgress",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:RunInstances",
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:TerminateInstances",
                "iam:AttachRolePolicy",
                "iam:CreatePolicy",
                "iam:CreateRole",
                "iam:DeletePolicy",
                "iam:DeleteRole",
                "iam:DeleteRolePolicy",
                "iam:DetachRolePolicy",
                "iam:GetPolicy",
                "iam:GetRole",
                "iam:GetRolePolicy",
                "iam:ListPolicyVersions",
                "iam:ListRoles",
                "iam:PassRole",
                "iam:PutRolePolicy",
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:GetFunction",
                "lambda:InvokeFunction",
                "lambda:UpdateFunctionCode",
                "lambda:UpdateFunctionConfiguration",
                "dynamodb:CreateTable",
                "dynamodb:DeleteTable",
                "dynamodb:DescribeTable",
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:Query",
                "dynamodb:Scan",
                "dynamodb:UpdateContinuousBackups",
                "dynamodb:UpdateTable",
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:DeleteObject",
                "s3:GetBucketPolicy",
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutBucketEncryption",
                "s3:PutBucketNotification",
                "s3:PutBucketPolicy",
                "s3:PutEncryptionConfiguration",
                "s3:PutObject",
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:GetTopicAttributes",
                "sns:Publish",
                "sns:Subscribe",
                "sqs:CreateQueue",
                "sqs:DeleteQueue",
                "sqs:GetQueueAttributes",
                "sqs:SetQueueAttributes",
                "bedrock:CreateAgent",
                "bedrock:CreateAgentActionGroup",
                "bedrock:CreateAgentAlias",
                "bedrock:CreateGuardrail",
                "bedrock:DeleteAgent",
                "bedrock:GetAgent",
                "bedrock:InvokeAgent",
                "bedrock:InvokeModel",
                "bedrock:ListAgents",
                "bedrock:PrepareAgent",
                "bedrock:UpdateAgent",
                "kms:CreateKey",
                "kms:DescribeKey",
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:GenerateDataKey"
            ],
            "Resource": "*"
        }
    ]
}
```

#### Environment setup

In this workshop, we will use the **Asia Pacific (Singapore) region (ap-southeast-1)**.

Unlike many AWS workshops, resources for this system are **not** deployed via a single CloudFormation template. This is an intentional architectural decision — the entire pipeline (VPC, EC2, S3, Lambda, DynamoDB, Bedrock Agent) is built manually step-by-step through the AWS Console across the following sections, so you gain hands-on familiarity with every service involved rather than treating it as a black box.

Before continuing, confirm:
+ You have an AWS account with the IAM permissions above attached.
+ The region selector (top-right of the AWS Console) is set to **Asia Pacific (Singapore)**.

![Region confirmation](/images/5-Workshop/5.2-Prerequisite/region-check.png)

The following sections will guide you through building each layer of the system: networking, data ingestion, the AI Agent, and monitoring — in that order.