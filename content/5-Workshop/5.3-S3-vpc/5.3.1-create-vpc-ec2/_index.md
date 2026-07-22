---
title : "Create VPC and Target EC2"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Open the [Amazon VPC console](https://ap-southeast-1.console.aws.amazon.com/vpc/home?region=ap-southeast-1#vpcs:)
2. In the navigation pane, choose **Your VPCs**, then click **Create VPC**.

{{% notice note %}}
Make sure the region selector (top-right) is set to **Asia Pacific (Singapore) (ap-southeast-1)** before continuing.
{{% /notice %}}

3. In the Create VPC console:
+ Choose **VPC only**
+ Name tag: ```netmon-infra-vpc```
+ IPv4 CIDR block: ```10.0.0.0/16```

![create vpc](/images/5-Workshop/5.3-S3-vpc/create-vpc.png)

4. Click **Create VPC**. Next, in the navigation pane, choose **Subnets** → **Create subnet**.
+ VPC ID: select ```netmon-infra-vpc```
+ Subnet name: ```netmon-infra-subnet-public```
+ IPv4 CIDR block: ```10.0.0.0/24```

![create subnet](/images/5-Workshop/5.3-S3-vpc/create-subnet.png)

5. Create and attach an **Internet Gateway**: navigate to **Internet Gateways** → **Create internet gateway** → name it ```netmon-infra-igw``` → after creation, select it → **Actions** → **Attach to VPC** → choose ```netmon-infra-vpc```.

![internet gateway](/images/5-Workshop/5.3-S3-vpc/igw.png)

6. Create a Security Group ```netmon-target-sg``` for the target instance:
+ Inbound rule: SSH (22) — source restricted to **My IP**
+ Inbound rule: HTTP (80) and a custom TCP range — source ```0.0.0.0/0``` (intentionally open to capture test attack traffic)

![security group](/images/5-Workshop/5.3-S3-vpc/sg.png)

7. Launch the target EC2 instance:
+ AMI: Amazon Linux 2023
+ Instance type: ```t3.micro```
+ Network: ```netmon-infra-vpc``` → Subnet: ```netmon-infra-subnet-public``` → Auto-assign public IP: **Enable**
+ Security Group: ```netmon-target-sg```

8. Confirm the instance state is **Running** and note its Public IPv4 address.

![launch ec2](/images/5-Workshop/5.3-S3-vpc/launch-ec2.png)