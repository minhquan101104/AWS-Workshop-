---
title : "Xây Lambda xử lý dữ liệu"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1 </b> "
---

Lambda này là cầu nối giữa lớp Networking/Data Collection và AI Agent — parse Flow Logs thô, tính toán chỉ số lưu lượng, và lưu vào DynamoDB để Agent truy vấn sau này.

#### Tạo bảng DynamoDB

1. Mở [DynamoDB console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#tables) → **Create table**.
+ Table name: ```netmon-app-traffic-stats```
+ Partition key: ```sourceIp``` (String)
+ Sort key: ```timeBucket``` (String) — gom traffic theo khung 5 phút cho mỗi IP, tránh hot partition khi 1 IP flood hệ thống
+ Capacity mode: **On-demand**


2. Sau khi tạo, vào **Additional settings** → bật **Point-in-time recovery (PITR)** và xác nhận **Encryption** dùng AWS managed key.


#### Tạo Lambda function

3. Mở [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/functions) → **Create function**.
+ Function name: ```netmon-infra-preprocess-logs```
+ Runtime: **Python 3.12**


4. Vào **Configuration → Environment variables**, thêm:
+ `SYSTEM_PAUSED` = `true` (công tắc an toàn — luôn giữ `true` trừ khi đang chủ động test)


5. Vào **Configuration → General configuration**, đặt **Timeout** = `2 phút`.

6. Viết logic hàm: parse từng bản ghi Flow Log từ object S3, tính tổng byte/packet theo `sourceIp` và khung `timeBucket` 5 phút, ghi kết quả bằng `dynamodb:PutItem`.

{{% notice tip %}}
Bỏ qua toàn bộ xử lý và return ngay nếu `SYSTEM_PAUSED == "true"` — chỉ 1 dòng kiểm tra này giúp tránh việc chạy nhầm ảnh hưởng dữ liệu khi có thao tác S3 không liên quan (ví dụ upload lại file).
{{% /notice %}}

7. Quay lại **Event notification** của S3 bucket (đã cấu hình ở mục 5.4.4), hoàn thiện việc chọn Lambda này làm đích.


8. Kiểm thử: chờ 1 file Flow Log mới tới (hoặc trigger thủ công qua tab **Test** với 1 sample S3 event), xác nhận có item mới xuất hiện trong ```netmon-app-traffic-stats```.

![ddb item](/images/5-Workshop/5.6-AI-Agent/ddb-item.png)