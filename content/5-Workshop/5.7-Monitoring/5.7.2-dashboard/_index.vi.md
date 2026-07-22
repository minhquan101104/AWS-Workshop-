---
title : "Xây CloudWatch Dashboard"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.7.2 </b> "
---

Dashboard giúp đội bảo mật có 1 nơi duy nhất để theo dõi tình trạng hệ thống và hoạt động phát hiện theo thời gian thực.

1. Mở [CloudWatch console](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#dashboards) → **Dashboards** → **Create dashboard**.
+ Name: ```NetMon-Dashboard```

2. Thêm các widget:
+ **Lambda metrics**: invocations, errors, duration cho đủ 5 hàm
+ **DynamoDB metrics**: read/write capacity cho 2 bảng
+ **SNS metrics**: số message đã publish
+ **Xu hướng bất thường**: số lần phát hiện theo thời gian
+ **Phân loại mức độ**: MEDIUM vs HIGH
+ **Lịch sử cảnh báo**: các alert gần đây đã gửi
+ **Lịch sử chặn IP**: hành động block/log-only gần đây

![dashboard widgets](/images/5-Workshop/5.7-Monitoring/dashboard-widgets.jpg)

3. Với các widget dựa trên log (xu hướng bất thường, mức độ, lịch sử), dùng query **Logs Insights** trên log group của từng Lambda rồi ghim kết quả thành widget.

{{% notice note %}}
Nếu widget Logs Insights hiện "No results" ngay sau khi test, thường chỉ do time range đang chọn quá rộng gây lag. Thu hẹp về 5-30 phút gần nhất khi kiểm tra 1 lần test vừa chạy.
{{% /notice %}}

4. Lưu dashboard, xác nhận mọi widget có dữ liệu thật sau khi chạy 1 lần quét test.

![dashboard final](/images/5-Workshop/5.7-Monitoring/dashboard-final.jpg)

5. (Tuỳ chọn, ghi nhận là định hướng phát triển) Thêm **CloudWatch Alarms** cho tỷ lệ lỗi Lambda và số message trong DLQ, nối vào SNS topic — chuyển giám sát từ bị động (phải mở dashboard lên xem) sang chủ động (hệ thống tự báo).