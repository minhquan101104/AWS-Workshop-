---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---
Xin chúc mừng bạn đã hoàn thành workshop này!
Bạn đã tự xây dựng thành công 1 hệ thống giám sát mạng bằng AI từ đầu đến cuối — 1 VPC chứa instance mục tiêu, VPC Flow Logs đổ vào S3 data lake, pipeline xử lý bằng Lambda, 1 Bedrock Agent tự suy luận với 4 Action Group tool, và đầy đủ lớp giám sát CloudWatch.

#### Dọn dẹp

Để tránh phát sinh chi phí, xoá tài nguyên theo đúng thứ tự sau (ngược lại thứ tự tạo, để xoá tài nguyên phụ thuộc trước):

1. **Bedrock Agent**: Mở Bedrock console → **Agents** → chọn ```netmon-security-agent``` → **Delete**.



2. **Các Lambda function**: Mở Lambda console → xoá đủ 5 hàm (`preprocess-logs`, `getNetworkMetrics`, `checkTrafficAnomaly`, `sendAlert`, `blockIP`).



3. **Bảng DynamoDB**: Mở DynamoDB console → xoá `netmon-app-traffic-stats` và `netmon-app-agent-decisions`.

4. **SNS topic & SQS queue**: Xoá `netmon-app-alerts` (SNS) và `netmon-dlq` (SQS).

5. **S3 bucket**: Mở S3 console → chọn `netmon-infra-flowlogs-8425` → **Empty** bucket trước, sau đó **Delete**.



6. **EC2 instance**: Terminate instance mục tiêu `netmon-infra-target`.

7. **VPC**: Sau khi mọi tài nguyên phụ thuộc đã xoá hết, xoá `netmon-infra-vpc` (thao tác này tự động xoá luôn Subnet, gỡ Internet Gateway, Route Table, Security Group gắn với nó).



8. **IAM roles/policies**: Xoá 5 role least-privilege và các customer-managed policy đã gắn.

{{% notice warning %}}
Xoá VPC sẽ báo lỗi nếu vẫn còn tài nguyên bên trong (EC2, ENI, Endpoint) — luôn xoá tài nguyên phụ thuộc trước.
{{% /notice %}}