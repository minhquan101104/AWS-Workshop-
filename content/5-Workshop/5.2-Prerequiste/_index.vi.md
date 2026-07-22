---
title : "Chuẩn bị"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### Quyền IAM
Gắn policy IAM sau vào user account của bạn để có thể xây dựng và dọn dẹp tài nguyên trong workshop này.

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

#### Thiết lập môi trường

Trong workshop này, chúng ta sẽ dùng region **Asia Pacific (Singapore) (ap-southeast-1)**.

Khác với nhiều workshop AWS thường dùng 1 CloudFormation template để dựng sẵn toàn bộ hạ tầng, hệ thống này **không** dùng CloudFormation. Đây là quyết định kiến trúc có chủ đích — toàn bộ pipeline (VPC, EC2, S3, Lambda, DynamoDB, Bedrock Agent) được xây dựng thủ công từng bước qua AWS Console xuyên suốt các mục dưới đây, để bạn thực sự làm quen với từng dịch vụ thay vì coi nó như một hộp đen.

Trước khi tiếp tục, xác nhận:
+ Bạn có tài khoản AWS đã gắn đủ quyền IAM ở trên.
+ Ô chọn Region (góc trên bên phải Console) đang để đúng **Asia Pacific (Singapore)**.

![Xác nhận Region](/images/5-Workshop/5.2-Prerequisite/region-check.png)

Các mục tiếp theo sẽ hướng dẫn bạn xây dựng từng lớp của hệ thống: mạng, thu thập dữ liệu, AI Agent, và giám sát — theo đúng thứ tự đó.