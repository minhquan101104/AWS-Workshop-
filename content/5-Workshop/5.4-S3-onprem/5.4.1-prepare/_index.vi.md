---
title : "Chuẩn bị S3 bucket"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Trước khi bật VPC Flow Logs, bạn cần 1 bucket đích để nhận log.

1. Mở [Amazon S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1)
2. Bấm **Create bucket**.
+ Bucket name: ```netmon-infra-flowlogs-8425``` (tên bucket phải duy nhất toàn cầu — thêm hậu tố riêng nếu bị trùng)
+ Region: **Asia Pacific (Singapore) ap-southeast-1**
+ Block Public Access settings: giữ nguyên cả 4 ô đều **tick** (bucket không bao giờ được để public)
3. Cuộn xuống mục **Default encryption** → chọn **Server-side encryption with AWS Key Management Service keys (SSE-KMS)** → **AWS KMS key**: `Use AWS managed key (aws/s3)` → bật **Bucket Key**.
4. Bấm **Create bucket**. Xác nhận nó xuất hiện trong danh sách bucket.

{{% notice tip %}}
Bật mã hoá ngay từ bước này nghĩa là mọi object Flow Log ghi vào sau đó tự động được bảo vệ — không cần làm thêm gì về sau.
{{% /notice %}}