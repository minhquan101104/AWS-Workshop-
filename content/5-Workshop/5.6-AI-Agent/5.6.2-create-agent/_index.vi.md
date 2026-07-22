---
title : "Tạo Bedrock Agent"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2 </b> "
---

Bedrock Agent là bộ não suy luận của hệ thống — tự quyết định gọi tool nào, khi nào, dựa trên dữ liệu nhận được.

1. Mở [Amazon Bedrock console](https://ap-southeast-1.console.aws.amazon.com/bedrock/home?region=ap-southeast-1#/agents) → **Agents** → **Create Agent**.
+ Agent name: ```netmon-security-agent```
+ Description: mô tả ngắn vai trò (tự động phát hiện và phản ứng mối đe dọa mạng)


2. Mục **Agent resource role**, để Bedrock tự tạo service role mới.

3. Mục **Select model**, chọn 1 foundation model.

{{% notice note %}}
Nếu gặp lỗi `429 rate limit` trên tài khoản mới tạo, thường do quota mặc định cho model đó chưa được nâng. Chuyển sang **Amazon Nova Pro** là giải pháp thực tế trong lúc chờ xin tăng quota cho model khác.
{{% /notice %}}


4. Ở **Instructions for the Agent**, viết system prompt rõ ràng mô tả vai trò, ví dụ:
> *"Bạn là AI Agent phân tích bảo mật mạng. Khi nhận dữ liệu traffic mạng, dùng tool để lấy chỉ số, kiểm tra bất thường, và dựa trên mức độ nghiêm trọng — gửi cảnh báo và/hoặc chặn IP độc hại. Luôn kiểm tra trạng thái bất thường trước khi thực hiện bất kỳ hành động nào."*


5. Bấm **Save**, sau đó **Create**. Bạn có 1 Agent chưa có tool nào — các mục tiếp theo sẽ thêm đủ 4 Action Group.

6. Tạo **Agent Alias** (ví dụ ```production```) — đây là endpoint ổn định để Lambda gọi vào, tách biệt khỏi các bản draft bạn đang chỉnh sửa trong lúc phát triển.