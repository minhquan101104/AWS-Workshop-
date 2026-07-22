---
title : "Lớp AI Agent"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Xây dựng Lớp AI Agent

Đây là phần cốt lõi của toàn hệ thống: 1 **Amazon Bedrock Agent** tự suy luận trên dữ liệu mạng và điều phối 4 Action Group tool để phát hiện và phản ứng với mối đe dọa — không cần con người can thiệp.

![overview](/images/5-Workshop/5.6-AI-Agent/diagram4.png)

#### Nội dung

- [Xây Lambda xử lý dữ liệu (preprocess-logs)](5.6.1-preprocess-lambda/)
- [Tạo Bedrock Agent](5.6.2-create-agent/)
- [Xây Action Group 1: getNetworkMetrics](5.6.3-getmetrics/)
- [Xây Action Group 2: checkTrafficAnomaly](5.6.4-checkanomaly/)
- [Xây Action Group 3: sendAlert](5.6.5-sendalert/)
- [Xây Action Group 4: blockIP](5.6.6-blockip/)
- [Kiểm thử toàn bộ luồng phát hiện tự động](5.6.7-test-agent/)