---
title : "VPC Flow Logs & S3 Data Lake"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Xây dựng Lớp Thu thập Dữ liệu

Trong mục này, bạn sẽ cấu hình **VPC Flow Logs** để ghi lại toàn bộ traffic mạng (ACCEPT + REJECT) đi vào EC2 mục tiêu, và thiết lập **Amazon S3** làm data lake lưu log thô. Đây là điểm khởi đầu của toàn bộ pipeline phát hiện — mọi Lambda và Bedrock Agent phía sau đều phụ thuộc vào dữ liệu được thu thập ở đây.

![overview](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

#### Nội dung

- [Chuẩn bị S3 bucket](5.4.1-prepare/)
- [Bật VPC Flow Logs](5.4.2-create-interface-enpoint/)
- [Xác nhận Flow Logs đã ghi vào S3](5.4.3-test-endpoint/)
- [Cấu hình S3 Event Notification](5.4.4-dns-simulation/)