---
title : "Thiết lập Dead Letter Queue"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

Các lời gọi Lambda bất đồng bộ (như `preprocess-logs` được trigger từ S3) có thể lỗi âm thầm sau khi hết retry. Dead Letter Queue bắt lại các sự kiện lỗi này để bạn điều tra thay vì mất luôn.

1. Mở [Amazon SQS console](https://ap-southeast-1.console.aws.amazon.com/sqs/v3/home?region=ap-southeast-1#/queues) → **Create queue**.
+ Type: **Standard**
+ Name: ```netmon-dlq```

![create queue](/images/5-Workshop/5.7-Monitoring/create-dlq.jpg)

2. Gắn vào `preprocess-logs`: mở Lambda → **Configuration → Asynchronous invocation** → **Edit** → **On failure** → **SQS queue** → chọn ```netmon-dlq```.

3. Đảm bảo IAM role của Lambda có quyền `sqs:SendMessage` giới hạn đúng ARN queue này — bước này rất dễ quên, khiến DLQ âm thầm không hoạt động dù đã cấu hình đúng phía Lambda.

4. Xác nhận destination đã set đúng.

![dlq attached](/images/5-Workshop/5.7-Monitoring/dlq-attached.jpg)

{{% notice tip %}}
Theo dõi định kỳ chỉ số **Messages available** của queue (hoặc đặt alarm, mục sau) — số khác 0 nghĩa là có lỗi đang xảy ra ở đâu đó cần xử lý.
{{% /notice %}}