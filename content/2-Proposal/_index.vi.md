---
title: "Đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# Hệ thống Giám sát Mạng bằng AI Agent trên AWS
## Giải pháp Serverless tự động phát hiện và phản ứng với mối đe dọa mạng theo thời gian thực

### 1. Tóm tắt tổng quan
Nền tảng Giám sát Mạng là hệ thống tự động phát hiện và phản ứng với mối đe dọa, xây dựng hoàn toàn trên AWS Serverless. Hệ thống thu thập lưu lượng mạng qua VPC Flow Logs, dùng Amazon Bedrock Agent để suy luận trên dữ liệu, tự động phân loại mức độ, chống trùng lặp cảnh báo, leo thang khi tấn công lặp lại, và thực thi hành động phòng thủ (gửi email cảnh báo, chặn IP qua Network ACL). Hệ thống giảm tải công việc thủ công cho đội SOC và rút ngắn thời gian từ phát hiện đến phản ứng từ hàng giờ xuống còn vài giây.

### 2. Vấn đề cần giải quyết
### Vấn đề là gì?
Giám sát mạng truyền thống phụ thuộc vào con người rà soát log, liên kết sự kiện, và tự quyết định phản ứng thủ công. Cách làm này không mở rộng được, dễ gây "alert fatigue" (quá tải cảnh báo), và phản ứng chậm với các mối đe dọa đang diễn ra như port scan hay brute-force.

### Giải pháp
Nền tảng dùng VPC Flow Logs để ghi lại traffic ACCEPT/REJECT, Amazon S3 làm data lake lưu log thô, AWS Lambda xử lý theo sự kiện, và Amazon DynamoDB lưu thống kê lưu lượng và audit trail. Một Amazon Bedrock Agent (model Amazon Nova Pro) tự điều phối 4 Action Group tool để phân tích traffic, phát hiện bất thường (có whitelist, chống trùng lặp, leo thang mức độ), gửi cảnh báo qua Amazon SNS, và chặn IP độc hại qua Network ACL. Amazon Bedrock Guardrails bảo vệ Agent khỏi các cuộc tấn công prompt injection.

### Lợi ích và hiệu quả đầu tư
Giải pháp loại bỏ nhu cầu rà soát log thủ công liên tục, rút ngắn thời gian từ phát hiện đến cảnh báo gần như thời gian thực, và tạo ra một kiến trúc tham chiếu có thể tái sử dụng cho tự động hoá bảo mật bằng AI. Vì hoàn toàn serverless, nền tảng chỉ phát sinh chi phí khi thực sự xử lý sự kiện, giữ chi phí vận hành hàng tháng thấp trong khi vẫn có thể mở rộng nếu lưu lượng tăng.

### 3. Kiến trúc giải pháp
Nền tảng chạy trong 1 VPC riêng (`netmon-infra-vpc`, 10.0.0.0/16) chứa Public Subnet với 1 EC2 instance đóng vai trò mục tiêu giám sát. Toàn bộ thành phần xử lý dữ liệu (Lambda, DynamoDB, Bedrock Agent, SNS, SQS) **có chủ đích không VPC-attach**, vì chỉ tương tác với managed service của AWS qua Public API và IAM — tránh chi phí NAT Gateway và độ trễ cold-start không cần thiết.

![Kiến trúc tổng quan NetMon](/images/2-Proposal/netmon_architecture.png)

### Các dịch vụ AWS sử dụng
- **Amazon VPC / EC2**: Chứa instance mục tiêu giám sát trong Public Subnet, Security Group giới hạn chặt.
- **VPC Flow Logs**: Ghi lại traffic ACCEPT + REJECT, aggregation mỗi 1 phút.
- **Amazon S3**: Lưu Flow Logs thô làm data lake thu thập (mã hoá SSE-KMS).
- **AWS Lambda**: 5 hàm — `preprocess-logs`, `getNetworkMetrics`, `checkTrafficAnomaly`, `sendAlert`, `blockIP`.
- **Amazon DynamoDB**: Bảng `traffic-stats` (chỉ số mạng) và `agent-decisions` (audit trail), mã hoá KMS, bật Point-in-Time Recovery.
- **Amazon Bedrock Agent**: Bộ điều phối AI trung tâm (model Amazon Nova Pro) suy luận trên 4 Action Group tool.
- **Amazon Bedrock Guardrails**: Chặn prompt injection và khai thác system prompt.
- **Amazon SNS**: Gửi email cảnh báo tới đội bảo mật.
- **Amazon SQS**: Dead Letter Queue bảo vệ Lambda tiền xử lý bất đồng bộ (trigger từ S3).
- **Amazon CloudWatch**: Dashboard (8 widget) theo dõi log, metric, và tình trạng vận hành.
- **AWS IAM**: Role least-privilege riêng cho từng Lambda.
- **AWS Shield Standard**: Bảo vệ DDoS cơ bản (tự động, không cần cấu hình).

### Thiết kế thành phần
- **Instance mục tiêu**: EC2 trong Public Subnet, có chủ đích để lộ nhằm thu hút traffic tấn công thật phục vụ kiểm thử phát hiện.
- **Thu thập dữ liệu**: VPC Flow Logs → S3 → S3 Event Notification kích hoạt Lambda `preprocess-logs`.
- **Lưu trữ dữ liệu**: DynamoDB lưu thống kê lưu lượng theo IP và toàn bộ quyết định của Agent làm audit trail bất biến.
- **Suy luận AI**: Bedrock Agent tự gọi `getNetworkMetrics` và `checkTrafficAnomaly`, sau đó tự quyết định có gọi `sendAlert` và/hoặc `blockIP` dựa trên mức độ nghiêm trọng.
- **Lớp phản ứng**: `sendAlert` publish vào SNS; `blockIP` ghi entry Network ACL khi `REAL_BLOCK_ENABLED=true` (mặc định ở chế độ an toàn chỉ ghi log).
- **Giám sát**: CloudWatch Dashboard trực quan hoá xu hướng bất thường, phân loại mức độ, lịch sử cảnh báo, lịch sử chặn IP.

### 4. Triển khai kỹ thuật
**Các giai đoạn triển khai**
- Giai đoạn 1 — Mạng & Thu thập: Thiết kế VPC, triển khai EC2 mục tiêu, cấu hình VPC Flow Logs và S3 data lake.
- Giai đoạn 2 — Pipeline dữ liệu: Xây Lambda `preprocess-logs`, thiết kế schema DynamoDB, xác nhận pipeline end-to-end.
- Giai đoạn 3 — AI Agent: Nghiên cứu Bedrock Agent, thiết kế và triển khai đủ 4 Action Group Lambda, kết nối vào Agent.
- Giai đoạn 4 — Hoàn thiện & Báo cáo: Áp dụng đánh giá AWS Well-Architected, siết IAM least-privilege, thêm DLQ, bật mã hoá KMS và PITR, hoàn thiện tài liệu.

**Yêu cầu kỹ thuật**
- Kiến thức thực hành về VPC networking, thiết kế IAM least-privilege, kiến trúc Lambda hướng sự kiện, thiết kế bảng DynamoDB, và cấu hình Amazon Bedrock Agent/Action Group.
- Công cụ kiểm thử: `nmap` mô phỏng port scan, mô phỏng SSH brute-force thủ công, và kiểm thử traffic spike để xác nhận logic phát hiện.

### 5. Lộ trình & Cột mốc
**Timeline dự án** (~11-12 tuần, 05/05/2026 – 20/07/2026)
- Tuần 1-3: Làm quen, tự đề xuất đề tài, thiết kế mạng ban đầu (VPC, EC2, Security Group).
- Tuần 4-5: VPC Flow Logs, S3 data lake, pipeline thu thập (Lambda + DynamoDB).
- Tuần 6-8: Nghiên cứu và triển khai Bedrock Agent với đủ 4 Action Group.
- Tuần 9-10: Guardrails, CloudWatch Dashboard, kiểm thử kịch bản tấn công thực tế.
- Tuần 11-12: Đánh giá AWS Well-Architected, siết bảo mật, báo cáo cuối cùng.

### 6. Ước tính ngân sách
Nền tảng hoàn toàn serverless và hướng sự kiện, nên chi phí tỷ lệ theo lưu lượng thực tế thay vì hạ tầng cố định. Các yếu tố chi phí chính:
- **Lời gọi Amazon Bedrock Agent** (tính theo token, chi phí biến động lớn nhất).
- **AWS Lambda**: số lần gọi và thời gian chạy (nằm trong Free Tier với lưu lượng quy mô lab điển hình).
- **Amazon DynamoDB**: capacity đọc/ghi on-demand.
- **Amazon S3**: lưu trữ Flow Logs thô (khuyến nghị lifecycle rule để kiểm soát chi phí).
- **EC2** t3.micro (đủ điều kiện Free Tier 12 tháng đầu).

Con số ước tính chính xác từ AWS Pricing Calculator phụ thuộc vào lưu lượng thực tế và tần suất gọi Bedrock trong quá trình kiểm thử; bảng chi tiết chi phí sẽ được đính kèm riêng khi khối lượng kiểm thử được chốt.

### 7. Đánh giá rủi ro
#### Ma trận rủi ro
- Giới hạn quota/rate limit của Bedrock Agent: Tác động trung bình, xác suất trung bình (tài khoản mới có quota mặc định thấp hơn).
- False positive/negative trong phát hiện bất thường: Tác động trung bình, xác suất trung bình.
- Vô tình chặn nhầm IP thật khi kiểm thử: Tác động cao, xác suất thấp (đã giảm thiểu bằng mặc định `REAL_BLOCK_ENABLED=false`).
- Single point of failure (Single-AZ): Tác động trung bình, xác suất thấp trong phạm vi dự án ngắn hạn.
- Không có WAF trước instance mục tiêu: đánh đổi có chủ đích (xem Mục 9), bù đắp bằng Shield Standard và rule Security Group/NACL giới hạn chặt.

#### Chiến lược giảm thiểu
- Quota: Chọn model dự phòng, nhận thức về throttling.
- Độ chính xác phát hiện: Logic whitelist + chống trùng lặp + leo thang mức độ để giảm nhiễu.
- An toàn khi chặn: Mặc định an toàn chỉ ghi log; muốn chặn thật phải chủ động bật.
- Độ tin cậy: Dead Letter Queue cho Lambda thu thập bất đồng bộ để tránh mất sự kiện âm thầm.

#### Kế hoạch dự phòng
- Công tắc `SYSTEM_PAUSED` để dừng ngay toàn bộ pipeline nếu phát sinh hành vi bất thường.
- Rà soát thủ công audit trail trong DynamoDB nếu cần xác minh hoặc hoàn tác phản ứng tự động.

### 8. Cân nhắc bảo mật
- **Không có WAF trước instance mục tiêu**: quyết định thiết kế có chủ đích, vì instance mục tiêu đóng vai trò honeypot thu thập traffic tấn công thật — lọc traffic qua WAF sẽ làm mất mục đích đó. Compensating control: AWS Shield Standard (bảo vệ DDoS cơ bản tự động), Security Group egress deny-by-default, và không gắn IAM Instance Profile cho instance mục tiêu để giới hạn phạm vi ảnh hưởng nếu bị chiếm.
- **Mã hoá at-rest**: S3 (SSE-KMS) và cả 2 bảng DynamoDB (AWS managed KMS key) đều được mã hoá, đã bật Point-in-Time Recovery cho DynamoDB.
- **IAM least-privilege**: cả 5 Lambda function chạy với customer-managed policy giới hạn chặt, không còn managed policy `*FullAccess` nào gắn kèm.

### 9. Giới hạn kiến trúc & Định hướng phát triển

Dưới đây là các đánh đổi kiến trúc có chủ đích do ràng buộc thời gian thực tập, được công khai minh bạch thay vì để người đọc tự phát hiện:

- **Không có AWS WAF trước instance mục tiêu**: có chủ đích — lọc traffic qua WAF cần thêm ALB và sẽ chỉ còn thấy được traffic HTTP/HTTPS, làm mất mục tiêu thu thập traffic port-scan thô. Compensating control: AWS Shield Standard (tự động), Security Group egress deny-by-default, không gắn IAM Instance Profile cho instance mục tiêu.
- **Phạm vi Dead Letter Queue**: chỉ Lambda `preprocess-logs` (trigger từ S3) dùng invocation bất đồng bộ thật, nơi cơ chế DLQ áp dụng đúng bản chất. 4 Lambda Action Group được Bedrock Agent gọi đồng bộ; lỗi ở đó được xử lý qua cơ chế retry/orchestration của chính Agent, không qua DLQ.
- **Gọi Agent đồng bộ**: `preprocess-logs` hiện gọi Bedrock Agent theo kiểu đồng bộ (`[sync-blocking]`). Tách rời bằng Amazon EventBridge + SQS sẽ đúng chuẩn serverless hơn và tăng khả năng chịu tải.
- **Kiểu truy vấn DynamoDB**: các Lambda hiện dùng `Scan()` thay vì `Query()` kết hợp Global Secondary Index. Chấp nhận được ở khối lượng dữ liệu hiện tại, nhưng cần tối ưu trước khi mở rộng lên quy mô production.
- **Single Availability Zone**: chấp nhận được cho dự án thực tập có giới hạn thời gian; triển khai production cần Multi-AZ EC2/Auto Scaling và DynamoDB Global Tables để tăng độ sẵn sàng.
- **Chưa có Infrastructure as Code**: toàn bộ môi trường được dựng thủ công qua AWS Console vì mục đích học tập. Chuyển sang AWS CDK hoặc Terraform là bước tiếp theo tự nhiên để tái lập và lặp an toàn hơn.

**Định hướng tiếp theo**: thêm CloudWatch Alarms để giám sát chủ động (thay vì chỉ qua dashboard), tích hợp AWS X-Ray cho distributed tracing xuyên suốt pipeline nhiều Lambda, và nghiên cứu Amazon SageMaker cho mô hình phát hiện bất thường bằng machine learning, giảm phụ thuộc ngưỡng cố định.

### 10. Kết quả kỳ vọng
#### Cải tiến kỹ thuật
Phát hiện và phản ứng tự động gần thời gian thực, thay thế việc rà soát log thủ công.
Kiến trúc tham chiếu có thể tái sử dụng cho tự động hoá bảo mật bằng AI Agent trên AWS.
#### Giá trị lâu dài
Một minh chứng thực tế về việc ứng dụng generative AI (Amazon Bedrock Agent) vào bài toán bảo mật vận hành thật, có thể mở rộng sang tích hợp GuardDuty, Multi-AZ, và Infrastructure as Code trong các phiên bản sau.