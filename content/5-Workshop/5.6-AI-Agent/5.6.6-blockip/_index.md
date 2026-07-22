---
title : "Action Group 4: blockIP"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.6.6 </b> "
---

This is the most sensitive tool — it can modify network access control. It defaults to a **safe, logged-only mode**.

#### Build the Lambda

1. Create Lambda ```netmon-tool-blockIP``` (Python 3.12).
2. Add environment variables:
+ `REAL_BLOCK_ENABLED` = `false` (**default — do not change unless you fully understand the impact**)
+ `NETWORK_ACL_ID` = your NACL ID, e.g. ```acl-02a618d1f2b2c5ea2```


3. IAM permissions: `ec2:CreateNetworkAclEntry` scoped to the specific NACL ARN, `dynamodb:PutItem` on the audit table.
4. Logic:
```
if REAL_BLOCK_ENABLED == "true":
    create a DENY entry in the Network ACL for sourceIp
    action = "blocked"
else:
    action = "logged_only"
write audit record with the action taken
```


{{% notice warning %}}
Keep `REAL_BLOCK_ENABLED=false` during development and demos. Flipping it to `true` will actually modify your NACL and can lock out legitimate traffic — including your own testing IP if the logic isn't scoped carefully.
{{% /notice %}}

#### Register the Action Group

5. In your Agent → **Action groups** → **Add**.
+ Action group name: ```blockIPActions```
+ Function name: ```blockIP```, parameter `sourceIp` (string, required)


6. **Save and exit** → **Prepare**. Test and confirm the audit record shows `action: "logged_only"` while the flag is `false`.

![blockip test](/images/5-Workshop/5.6-AI-Agent/blockip-test.png)

#### List of the Four Action Groups Configured in the Agent
![blockip test](/images/5-Workshop/5.6-AI-Agent/all-actiongroups.png)