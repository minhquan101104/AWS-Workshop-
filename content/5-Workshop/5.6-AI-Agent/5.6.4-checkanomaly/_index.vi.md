---
title : "Action Group 2: checkTrafficAnomaly"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.6.4 </b> "
---

Đây là Lambda phức tạp nhất hệ thống — quyết định traffic có thực sự là mối đe dọa không, với 3 lớp logic: lọc whitelist, chống trùng lặp cảnh báo, và leo thang mức độ.

#### Xây logic Lambda

1. Tạo Lambda ```checkTrafficAnomaly``` (Python 3.12), quyền least-privilege `Scan`/`Query` trên cả ```netmon-app-traffic-stats``` và ```netmon-app-agent-decisions```.

2. Triển khai logic phát hiện theo đúng thứ tự:
+ **Kiểm tra whitelist**: bỏ qua hoàn toàn các IP đã biết an toàn (ví dụ IP nội bộ team) — trả về `whitelistedSkipped: true`
+ **Ngưỡng bất thường**: đánh dấu nếu số lượng request/pattern vượt ngưỡng đã định trong khung thời gian
+ **Chống trùng lặp (60 phút)**: kiểm tra ```netmon-app-agent-decisions``` xem có cảnh báo gần đây cho cùng IP trong 60 phút qua không — nếu có, trả về `skippedDuplicates: true` thay vì cảnh báo lại
+ **Leo thang mức độ**: nếu là lần phát hiện thứ 2+ cho cùng IP, nâng mức độ lên `HIGH` và đặt `recommendBlock: true`
+ **Phát hiện quét phân tán**: tuỳ chọn kiểm tra nhiều IP nguồn cùng tấn công mục tiêu trong thời gian ngắn (dấu hiệu quét phối hợp)

3. Trả về JSON tóm tắt gọn (không trả dữ liệu thô) để giữ token usage của Agent thấp:
```json
{"anomalyDetected": true, "severity": "HIGH", "recommendBlock": true, "whitelistedSkipped": false, "skippedDuplicates": false}
```

#### Đăng ký Action Group

4. Vào Agent → **Action groups** → **Add**.
+ Action group name: ```anomalyActions```
+ Function name: ```checkTrafficAnomaly```, tham số `sourceIp` (string, required)

5. **Save and exit** → **Prepare**.

6. Kiểm thử với kịch bản tấn công lặp lại: xác nhận khi cùng 1 IP nguồn bị đánh dấu nhiều lần trong 60 phút, trường `details` ghi rõ chính xác số lần lặp lại và tự động đề xuất chặn khi vượt ngưỡng — thể hiện đúng cả logic đếm tần suất lẫn leo thang mức độ hoạt động chính xác.

![escalation evidence](/images/5-Workshop/5.6-AI-Agent/checkanomaly-test.png)