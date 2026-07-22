---
title: "Event 2"
date: 2026-07-15
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “Trận Chung Kết CLOUD ARCHITECT - AWS Tech Talk: SLA Monitoring, Chiến Lược Chinh Phục Chứng Chỉ CLF-C02 & Ứng Dụng AI Security Agent”

### 1. Tổng Quan Sự Kiện

* **Chủ đề:** AWS Cloud Architecture, Security Automation & Certification Roadmap.
* **Chương trình:** Chuỗi hội thảo chuyên đề thuộc dự án **First Cloud AI Journey**.
* **Danh sách Diễn giả:**
  * **Nguyễn Huỳnh Sơn** - Infrastructure Support Engineer at Endava / Ex-SPS, AWS Student Builder Group HUFLIT.
  * **Ngô Lê Tấn Huy** - Speaker & AWS Cloud Practitioner Specialist.
  * **Nguyễn Tuấn Thịnh** - DevOps/DevSecOps/Cloud Engineer at Styl Solutions, First Cloud AI Journey.

---

### 2. Nội Dung Trọng Tâm & Bài Học Chuyên Môn

#### Chủ đề 1: SLA and Monitoring – From SLA to Monitoring What Really Matters (Diễn giả: Nguyễn Huỳnh Sơn)
* **Khái niệm & Vai trò của SLA:** Cam kết mức độ dịch vụ giữa nhà cung cấp và khách hàng. Hỗ trợ quản trị rủi ro, đo lường hiệu năng và làm rõ trách nhiệm.
* **Khoảng cách "Healthy Infrastructure ≠ Happy User Experience":** Hạ tầng báo "xanh" (CPU 18%, ALB HealthCheck OK) chưa chắc người dùng đã thao tác thành công (ví dụ: mất kết nối CSDL làm lỗi đăng nhập).
* **Mô hình Monitoring Pyramid:** Cần theo dõi đa tầng từ *Cloud Provider → Infrastructure → Application → Business Metrics → Customer Experience*.
* **Quy trình Risk Loop:** *Identify Risk → Monitor Signals → Respond (SNS, SOP) → Improve*.

#### Chủ đề 2: Inside The Exam – AWS Cloud Practitioner (CLF-C02) (Diễn giả: Ngô Lê Tấn Huy)
* **Cấu trúc bài thi:** 65 câu hỏi trắc nghiệm trong 90 phút (thêm 30 phút cho người không bản ngữ), điểm đạt từ 700/1000.
* **Phân bổ 4 Domains:**
  1. *Cloud Concepts (24%)*: 6 lợi ích của Cloud, AWS WAF, AWS CAF.
  2. *Security & Compliance (30%)*: Shared Responsibility Model, IAM (Least Privilege), Security Groups vs NACLs.
  3. *Cloud Technology & Services (34%)*: EC2, S3, EBS, EFS, RDS, DynamoDB, VPC, Route 53.
  4. *Billing, Pricing & Support (12%)*: Các mô hình giá EC2, Cost Explorer, Support Plans.
* **Chiến lược ôn tập & Làm bài:** Phương pháp loại trừ ("Elimination technique"), gắn từ khóa giải quyết bài toán ("Keyword Thinking") và thực hành trên AWS Free Tier.

#### Chủ đề 3: Securing Your Web Apps With AWS Security Agent (Diễn giả: Nguyễn Tuấn Thịnh)
* **Giải quyết điểm nghẽn an ninh (Security Bottleneck):** Pentest thủ công tốn nhiều tuần và chi phí cao ($5k - $20k/lần).
* **Sức mạnh của Frontier Agent (Amazon Bedrock):** Tự động lập kế hoạch và thực thi bảo mật toàn diện: *Design Review (kiến trúc) → Code Review (Git PRs) → Automated Pentesting (tấn công thực tế)*.
* **Thực tế chi phí & Hạn chế:** Tiết kiệm chi phí đáng kể so với pentest truyền thống, tuy nhiên Agent có thể bị chặn bởi MFA/Biometrics và chưa tối ưu hoàn toàn cho các lỗi logic nghiệp vụ phức tạp.

---

### 3. Đúc Kết & Ứng Dụng Thực Tế Vào Dự Án

* **Xây dựng Monitoring đúng nghĩa:** Tích hợp các chỉ số Custom Metrics (như *Login Failure Rate*) kết hợp CloudWatch Alarms và SNS Topic thay vì chỉ theo dõi CPU/RAM đơn thuần.
* **Áp dụng AI Agent vào Security:** Thấy rõ tiềm năng của Generative AI (Amazon Bedrock) trong việc tự động hóa đánh giá an ninh mạng và phản ứng sự cố.
* **Lộ trình chứng chỉ:** Nắm rõ phương pháp hệ thống hóa kiến thức Cloud chuẩn AWS để sẵn sàng chinh phục chứng chỉ AWS Certified Cloud Practitioner.

#### Hình ảnh thực tế tại sự kiện

![AWS Seminar Presentation 1](/images/event2.jpg)
![AWS Seminar Presentation 2](/images/event2.1.jpg)

> **Tổng kết:** Buổi Seminar mang lại lượng kiến thức thực chiến phong phú, kết nối chặt chẽ giữa tư duy vận hành hệ thống (Monitoring/SLA), chuẩn hóa kiến thức (Certification) và xu hướng bảo mật tiên tiến với Generative AI.