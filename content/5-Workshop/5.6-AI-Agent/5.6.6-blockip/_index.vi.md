---
title : "Action Group 4: blockIP"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6.6 </b> "
---

Đây là tool nhạy cảm nhất — có thể thay đổi kiểm soát truy cập mạng. Mặc định chạy ở **chế độ an toàn, chỉ ghi log**.

#### Xây Lambda

1. Tạo Lambda ```netmon-tool-blockIP``` (Python 3.12).
2. Thêm environment variables:
+ `REAL_BLOCK_ENABLED` = `false` (**mặc định — không đổi trừ khi hiểu rõ tác động**)
+ `NETWORK_ACL_ID` = ID NACL của bạn, ví dụ ```acl-02a618d1f2b2c5ea2```


3. Quyền IAM: `ec2:CreateNetworkAclEntry` giới hạn đúng ARN của NACL đó, `dynamodb:PutItem` trên bảng audit.
4. Logic:
```
neu REAL_BLOCK_ENABLED == "true":
    tao 1 DENY entry trong Network ACL cho sourceIp
    action = "blocked"
nguoc lai:
    action = "logged_only"
ghi audit record voi hanh dong da thuc hien
```


{{% notice warning %}}
Giữ `REAL_BLOCK_ENABLED=false` trong lúc phát triển và demo. Bật thành `true` sẽ thực sự thay đổi NACL và có thể chặn nhầm traffic hợp lệ — kể cả IP test của chính bạn nếu logic không được giới hạn cẩn thận.
{{% /notice %}}

#### Đăng ký Action Group

5. Vào Agent → **Action groups** → **Add**.
+ Action group name: ```blockIPActions```
+ Function name: ```blockIP```, tham số `sourceIp` (string, required)


6. **Save and exit** → **Prepare**. Kiểm thử và xác nhận audit record hiện `action: "logged_only"` khi flag đang `false`.

![blockip test](/images/5-Workshop/5.6-AI-Agent/blockip-test.png)

#### Danh sách 4 Action Group hiện đủ trong Agent
![blockip test](/images/5-Workshop/5.6-AI-Agent/all-actiongroups.png)