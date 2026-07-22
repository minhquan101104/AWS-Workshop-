---
title: "Worklog Tuần 8"
date: 2026-06-22
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Khởi tạo dịch vụ Amazon SNS để gửi email thông báo sự cố tức thì cho đội ngũ quản trị.
* Lập trình các hàm Lambda tự động ứng phó sự cố: `sendAlert` (bắn thông báo) và `blockIP` (cấm IP trên Network ACL).
* Siết chặt toàn bộ chính sách phân quyền IAM Policy theo nguyên tắc cấp quyền tối thiểu (Least Privilege).


### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| **2** | - Khởi tạo Amazon SNS Topic `NetworkAlertTopic` phục vụ gửi cảnh báo khẩn cấp.<br>- Đăng ký Email Admin nhận thông báo từ SNS Topic và xác nhận link kích hoạt tự động. | 22/06/2026 | 22/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **3** | - Lập trình Lambda Action Group `sendAlert` bằng Python sử dụng thư viện `boto3`.<br>- Cấu hình định dạng nội dung email cảnh báo: Địa chỉ IP tấn công, thời điểm phát hiện và mức độ nguy hiểm. | 23/06/2026 | 23/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **4** | - Lập trình Lambda Action Group `blockIP` gọi API `create_network_acl_entry` của EC2 Client.<br>- Cấu hình chèn rule DENY với số thứ tự ưu tiên cao nhất trên Network ACL để ngắt kết nối IP độc hại tức thì. | 24/06/2026 | 24/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **5** | - Rà soát toàn bộ IAM Roles của các dịch vụ Lambda, Bedrock Agent, S3 và DynamoDB.<br>- Loại bỏ các quyền truy cập rộng, thay thế bằng Inline Policy chi tiết tuân thủ nguyên tắc Least Privilege. | 25/06/2026 | 25/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |
| **6** | - Thực hành kiểm thử độc lập hàm `blockIP` với địa chỉ IP giả lập trên giao diện Lambda Console.<br>- Soi bảng Network ACL xác nhận rule DENY đã tự động tạo và kiểm tra hòm thư nhận email từ SNS thành công. | 26/06/2026 | 26/06/2026 | https://cloudjourney.awsstudygroup.com/vi/ |

### Kết quả đạt được tuần 8:

* Hoàn thiện 2 công cụ phản ứng sự cố tự động cốt lõi là `sendAlert` và `blockIP`.
* Thiết lập thành công kênh thông báo khẩn cấp Amazon SNS phản hồi theo thời gian thực.
* Chuẩn hóa và siết chặt hạ tầng phân quyền IAM Role, đảm bảo an toàn tuyệt đối cho toàn bộ hệ thống Serverless.
