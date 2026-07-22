---
title : "Xác nhận Flow Logs đã ghi vào S3"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

Flow Logs có thể mất vài phút mới bắt đầu ghi dữ liệu. Dùng lại traffic đã tạo ở bước test kết nối trước đó (curl / nmap) để xác nhận dữ liệu đang chảy đúng.

1. Mở S3 console → vào bucket ```netmon-infra-flowlogs-8425```.
2. Đi sâu vào cấu trúc thư mục tự sinh (```AWSLogs/<account-id>/vpcflowlogs/ap-southeast-1/<year>/<month>/<day>/```) cho tới khi thấy file `.gz`.

![s3 folder structure](/images/5-Workshop/5.4-S3-onprem/s3-folders.png)

3. Tải và mở 1 file để xác nhận có chứa bản ghi traffic thật, bao gồm cả các mục từ lần test nmap/curl trước đó.

{{% notice tip %}}
Nếu sau 5-10 phút vẫn không thấy file nào, kiểm tra lại trạng thái Flow Log đã **Active** chưa, và đảm bảo đã chọn đúng cấp VPC (không phải chỉ subnet) ở bước trước.
{{% /notice %}}