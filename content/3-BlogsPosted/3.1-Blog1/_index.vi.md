---
title: "Blog 1"
date: 2026-05-19
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# CIRT INSIGHTS: NGĂN CHẶN VIỆC XÓA TÀI KHOẢN TRÁI PHÉP KHỎI AWS ORGANIZATIONS

## Giới thiệu

AWS Customer Incident Response Team (AWS CIRT) đã chia sẻ một phương thức tấn công mới mà các đối tượng xấu thường sử dụng sau khi chiếm quyền kiểm soát tài khoản AWS. Thay vì khai thác lỗ hổng của dịch vụ AWS, kẻ tấn công tìm cách đưa tài khoản ra khỏi AWS Organizations nhằm vô hiệu hóa các cơ chế quản trị và bảo mật tập trung.

Bài viết này trình bày cách thức tấn công, những rủi ro có thể xảy ra và các biện pháp phòng chống được AWS khuyến nghị.

## Phương thức tấn công

Sau khi có đủ quyền, đặc biệt là quyền organizations:LeaveOrganization, kẻ tấn công có thể đưa tài khoản thành viên ra khỏi AWS Organizations.

Khi đó tài khoản sẽ không còn được bảo vệ bởi:

* Service Control Policies (SCP)
* CloudTrail của tổ chức
* Amazon GuardDuty quản lý tập trung
* Consolidated Billing
* Hệ thống giám sát bảo mật của tổ chức

Điều này giúp kẻ tấn công dễ dàng thực hiện các hành vi nguy hiểm mà khó bị phát hiện hơn. 

## Tại sao đây là mối nguy hiểm?

Đây không phải là lỗ hổng của AWS mà là việc lợi dụng các quyền IAM được cấp quá rộng hoặc thông tin xác thực bị đánh cắp.

Hậu quả có thể bao gồm:

* Mất các lớp bảo vệ tập trung.
* Giảm khả năng giám sát của đội ngũ bảo mật.
* Vượt qua các chính sách quản trị của tổ chức.
* Tăng nguy cơ phá hoại tài nguyên.
* Khó khăn trong quá trình điều tra sự cố. 

## Khuyến nghị của AWS

AWS đưa ra một số biện pháp bảo vệ quan trọng:

### Chặn quyền LeaveOrganization

Tạo Service Control Policy (SCP) để từ chối quyền organizations:LeaveOrganization đối với các tài khoản thành viên.

### Áp dụng nguyên tắc Least Privilege

Chỉ cấp các quyền IAM thực sự cần thiết và thường xuyên rà soát các IAM Role cũng như IAM Policy.

### Bảo vệ Root Account

* Bật xác thực đa yếu tố (MFA).
* Xóa các Root Access Key không sử dụng.
* Áp dụng cơ chế quản lý Root tập trung nếu có thể.

### Theo dõi CloudTrail

Thường xuyên giám sát các sự kiện:

* organizations:LeaveOrganization
* organizations:AcceptHandshake
* organizations:RemoveAccountFromOrganization

Nếu phát hiện các sự kiện này ngoài kế hoạch quản trị, cần điều tra ngay lập tức. 

## Kiến thức đạt được

Sau khi đọc bài viết này, tôi học được:

* Vai trò quan trọng của AWS Organizations trong việc quản lý nhiều tài khoản AWS.
* Cách kẻ tấn công lợi dụng quyền IAM để vượt qua các cơ chế bảo mật.
* Tầm quan trọng của Service Control Policy (SCP).
* Nguyên tắc Least Privilege trong quản lý IAM.
* Vai trò của AWS CloudTrail trong việc phát hiện các hành vi bất thường.

## Đánh giá cá nhân

Bài viết giúp tôi hiểu rõ hơn về các kỹ thuật tấn công trong môi trường điện toán đám mây và cách AWS khuyến nghị bảo vệ hệ thống nhiều tài khoản. Những kiến thức này rất hữu ích khi triển khai các hệ thống có yêu cầu bảo mật cao, đặc biệt là các dự án sử dụng AWS Organizations và nhiều tài khoản thành viên.

## Tài liệu tham khảo

AWS Security Blog:

https://aws.amazon.com/vi/blogs/security/cirt-insights-how-to-help-prevent-unauthorized-account-removals-from-aws-organizations/