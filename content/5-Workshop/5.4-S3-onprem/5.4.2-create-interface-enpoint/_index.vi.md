---
title : "Bật VPC Flow Logs"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

1. Mở [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#vpcs:) → chọn ```netmon-infra-vpc```.
2. Vào tab **Flow logs** → bấm **Create flow log**.
+ Name: ```netmon-infra-flowlog```
+ Filter: **All** (ghi cả traffic ACCEPT lẫn REJECT — quan trọng để phát hiện các lần quét bị chặn)
+ Maximum aggregation interval: **1 phút** (xuất log nhanh nhất, giúp pipeline phát hiện mối đe dọa sớm hơn)
3. Destination: chọn **Send to an Amazon S3 bucket**.
+ S3 bucket ARN: browse và chọn ```netmon-infra-flowlogs-8425```
+ Log record format: dùng **AWS default format** là đủ cho dự án này
4. Bấm **Create flow log**. Xác nhận trạng thái chuyển thành **Active**.

![flow log active](/images/5-Workshop/5.4-S3-onprem/flowlog-active.png)