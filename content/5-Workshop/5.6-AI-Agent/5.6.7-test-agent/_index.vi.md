---
title : "Kiểm thử toàn bộ luồng tự động"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.6.7 </b> "
---

Sau khi đã nối đủ 4 Action Group, giờ là lúc xác nhận toàn bộ chuỗi end-to-end bằng traffic tấn công thật.

1. Từ máy local, quét port vào instance mục tiêu:
```
nmap -sV <target-public-ip>
```

2. Theo dõi luồng: VPC Flow Logs → S3 → `preprocess-logs` → DynamoDB → (nếu bị đánh dấu) gọi Agent → `getNetworkMetrics` → `checkTrafficAnomaly` → (tuỳ mức độ) `sendAlert` và/hoặc `blockIP`.

3. Mở **CloudWatch Logs** của từng Lambda theo đúng thứ tự để trace lại quá trình thực thi, xác nhận từng bước chạy đúng.

![cloudwatch trace](/images/5-Workshop/5.6-AI-Agent/cloudwatch-trace.png)

4. Xác nhận nhận được email cảnh báo thật từ SNS.

![email alert](/images/5-Workshop/5.6-AI-Agent/final-email-alert.png)

5. Kiểm tra ```netmon-app-agent-decisions``` trong DynamoDB — phải thấy 1 audit record mới với đầy đủ dấu vết quyết định (mức độ, hành động đã thực hiện, timestamp).

![audit record](/images/5-Workshop/5.6-AI-Agent/audit-record.png)

6. Lặp lại đúng lần quét đó thêm 1 lần nữa trong vòng 60 phút, xác nhận logic chống trùng lặp chặn đúng cảnh báo trùng (hoặc leo thang mức độ, tuỳ thiết kế của bạn).

{{% notice tip %}}
Nếu quét xong không thấy gì xảy ra, kiểm tra `SYSTEM_PAUSED` của `preprocess-logs` trước tiên — đây là nguyên nhân phổ biến nhất khiến pipeline có vẻ "đứng im" lúc test.
{{% /notice %}}

Chúc mừng — bạn đã có 1 pipeline phát hiện và phản ứng mối đe dọa mạng bằng AI hoạt động tự động, end-to-end trên AWS.