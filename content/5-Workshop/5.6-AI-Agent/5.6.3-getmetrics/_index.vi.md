---
title : "Action Group 1: getNetworkMetrics"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.6.3 </b> "
---

Đây là "đôi mắt" của Agent — 1 tool chỉ đọc, lấy thống kê lưu lượng của 1 IP nguồn từ DynamoDB.

#### Xây Lambda

1. Tạo Lambda mới ```getNetworkMetrics``` (Python 3.12).
2. Gắn IAM role least-privilege chỉ có `dynamodb:GetItem` / `dynamodb:Query` trên ```netmon-app-traffic-stats```.
3. Hàm phải nhận event từ Agent và trả về đúng format Bedrock yêu cầu:

```json
{
    "messageVersion": "1.0",
    "response": {
        "actionGroup": "networkActions",
        "function": "getNetworkMetrics",
        "functionResponse": {
            "responseBody": {
                "TEXT": {
                    "body": "{\"sourceIp\": \"x.x.x.x\", \"requestCount\": 42, \"lastSeen\": \"...\"}"
                }
            }
        }
    }
}
```

{{% notice tip %}}
Sai định dạng response là lỗi tích hợp hay gặp nhất — Agent sẽ âm thầm không parse được kết quả tool nếu `functionResponse.responseBody.TEXT.body` không đúng khớp cấu trúc này.
{{% /notice %}}


#### Đăng ký làm Action Group

4. Mở Agent → **Action groups** → **Add**.
+ Action group name: ```networkActions```
+ Action group type: **Define with function details**
+ Function name: ```getNetworkMetrics```, mô tả rõ ràng và 1 tham số `sourceIp` (string, required)
+ Chọn đúng Lambda vừa tạo


5. Bấm **Save and exit**, sau đó **Prepare** Agent để áp dụng thay đổi.

6. Kiểm thử trong khung chat Agent Builder: hỏi thử *"Lịch sử traffic của IP 203.0.113.5 thế nào?"* và xác nhận Agent gọi đúng tool, trả về dữ liệu.

![test in builder](/images/5-Workshop/5.6-AI-Agent/getmetrics-test.png)