---
title : "Test Connectivity"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

1. From your local machine, ping or curl the target instance's Public IP to confirm it is reachable:
```
curl http://<target-public-ip>
```

2. (Optional, for later anomaly-detection testing) Run a basic `nmap` scan against the target to simulate reconnaissance traffic:
```
nmap -sV <target-public-ip>
```

![test connectivity](/images/5-Workshop/5.3-S3-vpc/test-connectivity.png)

3. Confirm the traffic reaches the instance and generates activity you can later observe in **VPC Flow Logs** (configured in the next section).

{{% notice tip %}}
Keep note of this test — you will use it again later to verify the anomaly-detection pipeline picks up the same scan.
{{% /notice %}}