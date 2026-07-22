---
title : "Action Group 3: sendAlert"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.6.5 </b> "
---

Tool này thông báo cho đội bảo mật và ghi lại audit record cho quyết định của Agent.

#### Tạo SNS Topic

1. Mở [Amazon SNS console](https://ap-southeast-1.console.aws.amazon.com/sns/v3/home?region=ap-southeast-1#/topics) → **Create topic**.
+ Type: **Standard**
+ Name: ```netmon-app-alerts```


2. Tạo subscription: Protocol **Email**, nhập email đội bảo mật. Xác nhận subscription qua link trong email được gửi tới.


#### Xây Lambda

3. Tạo Lambda ```netmon-tool-sendAlert``` (Python 3.12), nhận tham số `attackType`, `sourceIp`, `alertLevel`, `details` từ Agent.
+ Quyền IAM: `sns:Publish` trên topic, `dynamodb:PutItem` trên ```netmon-app-agent-decisions``` (ghi audit record)


4. Hàm publish message định dạng sẵn vào SNS, sau đó ghi 1 record vào ```netmon-app-agent-decisions``` (partition key `eventId`) gồm: timestamp, sourceIp, mức độ, hành động đã thực hiện — đây chính là audit trail của bạn.

#### Đăng ký Action Group

5. Vào Agent → **Action groups** → **Add**.
+ Action group name: ```sendAlert```
+ Function name: ```sendAlert```, tham số: `attackType`, `sourceIp`, `alertLevel`, `details` (đều string, required)


6. **Save and exit** → **Prepare**. Kiểm thử và xác nhận nhận được email cảnh báo thật.

![email received](/images/5-Workshop/5.6-AI-Agent/email-received.png)