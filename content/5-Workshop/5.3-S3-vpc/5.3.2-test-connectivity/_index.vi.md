---
title : "Kiểm tra kết nối"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

1. Từ máy local, ping hoặc curl vào Public IP của instance mục tiêu để xác nhận có kết nối được:
```
curl http://<target-public-ip>
```

2. (Tuỳ chọn, dùng để test phát hiện bất thường sau này) Chạy thử `nmap` cơ bản vào instance mục tiêu để mô phỏng traffic dò quét:
```
nmap -sV <target-public-ip>
```

![test connectivity](/images/5-Workshop/5.3-S3-vpc/test-connectivity.png)

3. Xác nhận traffic đã tới được instance và tạo ra hoạt động mà bạn sẽ quan sát lại sau này qua **VPC Flow Logs** (cấu hình ở mục tiếp theo).

{{% notice tip %}}
Ghi nhớ lần test này — bạn sẽ dùng lại chính lần quét này để xác nhận pipeline phát hiện bất thường có bắt được đúng nó không.
{{% /notice %}}