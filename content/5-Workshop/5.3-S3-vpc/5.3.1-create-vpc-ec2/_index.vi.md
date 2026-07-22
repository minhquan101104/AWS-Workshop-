---
title : "Tạo VPC và EC2 mục tiêu"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#vpcs:)
2. Ở menu bên trái, chọn **Your VPCs**, bấm **Create VPC**.

{{% notice note %}}
Đảm bảo ô chọn Region (góc trên bên phải) đang để đúng **Asia Pacific (Singapore) (ap-southeast-1)** trước khi tiếp tục.
{{% /notice %}}

3. Trong màn hình Create VPC:
+ Chọn **VPC only**
+ Name tag: ```netmon-infra-vpc```
+ IPv4 CIDR block: ```10.0.0.0/16```

![create vpc](/images/5-Workshop/5.3-S3-vpc/create-vpc.png)

4. Bấm **Create VPC**. Tiếp theo, vào **Subnets** → **Create subnet**.
+ VPC ID: chọn ```netmon-infra-vpc```
+ Subnet name: ```netmon-infra-subnet-public```
+ IPv4 CIDR block: ```10.0.0.0/24```

![create subnet](/images/5-Workshop/5.3-S3-vpc/create-subnet.png)

5. Tạo và gắn **Internet Gateway**: vào **Internet Gateways** → **Create internet gateway** → đặt tên ```netmon-infra-igw``` → sau khi tạo, chọn nó → **Actions** → **Attach to VPC** → chọn ```netmon-infra-vpc```.

![internet gateway](/images/5-Workshop/5.3-S3-vpc/igw.png)

6. Tạo Security Group ```netmon-target-sg``` cho instance mục tiêu:
+ Inbound rule: SSH (22) — source giới hạn về **My IP**
+ Inbound rule: HTTP (80) và 1 dải TCP tuỳ chỉnh — source ```0.0.0.0/0``` (cố ý mở để hứng traffic tấn công thử nghiệm)

![security group](/images/5-Workshop/5.3-S3-vpc/sg.png)

7. Khởi tạo EC2 instance mục tiêu:
+ AMI: Amazon Linux 2023
+ Instance type: ```t3.micro```
+ Network: ```netmon-infra-vpc``` → Subnet: ```netmon-infra-subnet-public``` → Auto-assign public IP: **Enable**
+ Security Group: ```netmon-target-sg```

8. Xác nhận instance ở trạng thái **Running**, ghi nhớ Public IPv4 của nó.

![launch ec2](/images/5-Workshop/5.3-S3-vpc/launch-ec2.png)