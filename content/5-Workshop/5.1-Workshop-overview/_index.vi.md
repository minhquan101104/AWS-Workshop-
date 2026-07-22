---
title : "Giới thiệu"
date : 2024-01-01 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giám sát mạng bằng AI Agent
+ Giám sát mạng truyền thống đòi hỏi chuyên viên bảo mật phải thủ công rà soát log, liên kết sự kiện từ nhiều nguồn, và tự quyết định phản ứng — cách làm này không mở rộng được và dễ phản ứng chậm với các mối đe dọa đang diễn ra.
+ **Amazon Bedrock Agent** cho phép một mô hình generative AI tự suy luận trên dữ liệu mạng, gọi các tool chuyên biệt (Action Group) để điều tra sâu hơn, và tự quyết định phản ứng phù hợp — không cần con người can thiệp cho các trường hợp phát hiện và phân loại thông thường.

#### Tổng quan về workshop
Trong workshop này, bạn sẽ tự xây dựng một hệ thống giám sát mạng bằng AI, hoàn toàn serverless, từ đầu đến cuối trên AWS.

+ **Lớp thu thập dữ liệu**: 1 VPC với EC2 instance mục tiêu sinh ra traffic mạng thật, được **VPC Flow Logs** ghi lại và lưu vào data lake **Amazon S3**.
+ **Lớp xử lý dữ liệu**: 1 hàm **AWS Lambda** được kích hoạt bởi S3 Event sẽ parse log thô và lưu thống kê lưu lượng theo từng IP vào **Amazon DynamoDB**.
+ **Lớp AI Agent**: 1 **Amazon Bedrock Agent** tự động gọi 4 Action Group tool — đọc chỉ số mạng, phát hiện bất thường (có whitelist, chống trùng lặp, leo thang mức độ), gửi cảnh báo qua **Amazon SNS**, và chặn IP độc hại qua **Network ACL**.
+ **Lớp bảo mật**: **Amazon Bedrock Guardrails** bảo vệ Agent khỏi các cuộc tấn công prompt injection, và mỗi Lambda chạy với 1 IAM role theo nguyên tắc least-privilege.

Kết thúc workshop, bạn sẽ có 1 pipeline end-to-end hoạt động thật, có thể phát hiện port scan hoặc brute-force giả lập, tự động phân loại mức độ nghiêm trọng, và phản ứng — hoàn toàn không cần can thiệp thủ công.

![Kiến trúc tổng quan NetMon](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)