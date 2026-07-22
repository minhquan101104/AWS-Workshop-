---
title : "Lớp Mạng (Networking)"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

#### Xây dựng Networking Layer

Trong mục này, bạn sẽ xây dựng nền tảng mạng cho hệ thống giám sát: 1 VPC riêng, 1 Public Subnet, và 1 EC2 instance mục tiêu được để lộ có chủ đích nhằm thu hút traffic mạng thật. Đây chính là nguồn dữ liệu mà phần còn lại của pipeline (VPC Flow Logs, Lambda, Bedrock Agent) sẽ phân tích ở các mục sau.

![overview](/images/5-Workshop/5.3-S3-vpc/diagram2.png)

#### Nội dung

- [Tạo VPC và EC2 instance mục tiêu](5.3.1-create-vpc-ec2/)
- [Kiểm tra kết nối tới instance mục tiêu](5.3.2-test-connectivity/)