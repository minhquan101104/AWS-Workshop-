---
title: "Worklog Tuần 2"
date: 2026-05-11
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---



### Mục tiêu tuần 2:

* Tìm hiểu kiến trúc mạng ảo Amazon VPC, các phân vùng Subnet, Route Table và Internet Gateway.
* Phân biệt cơ chế bảo mật của Security Group và Network ACL; thực hành cấu hình máy chủ EC2 và kho lưu trữ S3.
* Khảo sát các bài toán thực tế do đơn vị gợi ý và định hướng đề tài thực tập tốt nghiệp.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| **2** | - Nghiên cứu kiến trúc mạng ảo Amazon VPC, dải IP CIDR, Public và Private Subnets.<br>- Tìm hiểu cách định tuyến mạng bằng Route Table, Internet Gateway (IGW) và NAT Gateway.<br>- Phân biệt cơ chế bảo mật tường lửa của Security Group (Stateful) và Network ACL (Stateless). | 11/05/2026 | 11/05/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **3** | - Tìm hiểu chuyên sâu dịch vụ máy chủ ảo Amazon EC2 và các nhóm Instance Types.<br>- Khảo sát dịch vụ lưu trữ đối tượng Amazon S3, lưu trữ khối Amazon EBS.<br>- Học cách tạo và quản lý Key Pair để kết nối SSH an toàn từ máy cá nhân vào EC2. | 12/05/2026 | 12/05/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **4** | - **Thực hành:**<br>&emsp; + Khởi tạo một Amazon VPC hoàn chỉnh kèm Public/Private Subnet.<br>&emsp; + Dựng máy chủ EC2 Ubuntu, gắn Security Group mở cổng 22, 80 và SSH kiểm thử.<br>&emsp; + Tạo Amazon S3 Bucket, cấu hình mã hóa mặc định và tải tệp kiểm thử. | 13/05/2026 | 13/05/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **5** | - Khảo sát các định hướng bài toán thực tế do AWS Việt Nam gợi ý cho chương trình FCAJ.<br>- Nghiên cứu xu hướng ứng dụng Trí tuệ nhân tạo (AI/ML) và Generative AI trong hạ tầng đám mây.<br>- Họp nhóm thảo luận lựa chọn bài toán giám sát an ninh mạng. | 14/05/2026 | 14/05/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **6** | - Báo cáo định hướng ý tưởng đề tài bước đầu với Mentor Nguyễn Gia Hưng.<br>- Phân tích thực trạng bùng nổ cảnh báo giả (Alert Fatigue) trong vận hành mạng.<br>- Đề xuất hướng ứng dụng AI Agent để tự động hóa quá trình rà soát log và cô lập sự cố. | 15/05/2026 | 15/05/2026 | https://cloudjourney.awsstudygroup.com/vi/ |

### Kết quả đạt được tuần 2:

* Nắm vững tư duy thiết kế mô hình mạng ảo cô lập Amazon VPC và cơ chế phân vùng bảo mật Subnet.
* Thành thạo kỹ năng khởi tạo máy chủ EC2, gắn ổ cứng EBS, cấu hình Security Group và kết nối SSH an toàn.
* Hiểu cách tạo kho lưu trữ Amazon S3, quản lý quyền truy cập Bucket Policy và mã hóa dữ liệu.
* Phân biệt rõ cơ chế chặn/mở traffic của Security Group (Stateful) và Network ACL (Stateless).
* Nhận diện được bài toán Alert Fatigue trong giám sát hạ tầng và định hình được đề xuất giải pháp AI Agent với Mentor.

