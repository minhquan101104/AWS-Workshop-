---
title : "Cấu hình S3 Event Notification"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Bước cuối cùng của Lớp Thu thập Dữ liệu là nối S3 để tự động kích hoạt xử lý mỗi khi có file Flow Log mới — đây chính là cầu nối sang Lớp Xử lý Dữ liệu (Lambda) ở mục tiếp theo.

1. Mở S3 console → chọn ```netmon-infra-flowlogs-8425``` → vào tab **Properties**.
2. Cuộn tới **Event notifications** → bấm **Create event notification**.
+ Event name: ```netmon-flowlog-trigger```
+ Event types: **All object create events** (```s3:ObjectCreated:*```)
+ Destination: **Lambda function** (bạn sẽ tạo và chọn hàm `preprocess-logs` ở mục tiếp theo — có thể tạm dừng bước này, quay lại hoàn thiện sau khi Lambda đã tồn tại)
{{% notice note %}}
Việc tạm dừng ở đây là bình thường — S3 Event Notification cần Lambda đích đã tồn tại từ trước. Tiếp tục sang mục kế tiếp để xây Lambda đó, rồi quay lại đây hoàn thiện kết nối trigger.
{{% /notice %}}