# 💻 Day 06: Launch EC2 Instance

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team is ready to provision their first compute resource. They need a standard virtual machine to host their internal tools.

**Task Requirements:**
1.  **Instance Name:** `datacenter-ec2`.
2.  **AMI:** `Amazon Linux`.
3.  **Type:** `t2.micro`.
4.  **Key Pair:** Create new named `datacenter-kp`.
5.  **Security Group:** Attach the `default` security group.

## 💻 Infrastructure Details
| Resource | Name | Type | Key Pair |
| :--- | :--- | :--- | :--- |
| **EC2 Instance** | `datacenter-ec2` | `t2.micro` | `datacenter-kp` |

---

## 🛠️ Implementation Method 1: AWS Console (GUI)
*Best for beginners or visual verification.*

**1. Launch Wizard:**
* Navigate to **EC2** -> **Instances** -> **Launch instances**.

**2. Configuration:**
* **Name:** `datacenter-ec2`.
* **OS:** Amazon Linux (Default AMI).
* **Instance Type:** `t2.micro`.

**3. Key Pair Creation:**
* Within the wizard, click "Create new key pair".
* **Name:** `datacenter-kp` (RSA / .pem).
* *Note:* This automatically selects the new key for the instance.

**4. Network Settings (Security Group):**
* Change setting from "Create security group" to **Select existing security group**.
* Select the `default` VPC security group from the list.

**5. Execution:**
* Click **Launch instance**.

---

## 🛠️ Implementation Method 2: AWS CLI
*Best for automation. Copy and paste this entire block into your terminal to perform the Task AND Verification instantly.*

```bash
# --- Step 1: Create Key Pair ---
aws ec2 create-key-pair \
    --key-name datacenter-kp \
    --key-type rsa \
    --query "KeyMaterial" \
    --region us-east-1 \
    --output text > datacenter-kp.pem

chmod 400 datacenter-kp.pem

# --- Step 2: Get Dynamic IDs (SG & AMI) ---
SG_ID=$(aws ec2 describe-security-groups \
    --filters Name=group-name,Values=default \
    --region us-east-1 \
    --query "SecurityGroups[0].GroupId" \
    --output text)

AMI_ID=$(aws ec2 describe-images \
    --owners amazon \
    --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
    --region us-east-1 \
    --query "reverse(sort_by(Images, &CreationDate))[0].ImageId" \
    --output text)

echo "Configuration: AMI=$AMI_ID | SG=$SG_ID"

# --- Step 3: Launch Instance ---
aws ec2 run-instances \
    --image-id $AMI_ID \
    --instance-type t2.micro \
    --key-name datacenter-kp \
    --security-group-ids $SG_ID \
    --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=datacenter-ec2}]" \
    --region us-east-1

# --- Step 4: Verification ---
echo "Verifying Instance State..."
sleep 2 # Short pause to allow API to register
aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=datacenter-ec2" \
    --region us-east-1 \
    --query "Reservations[*].Instances[*].{ID:InstanceId,State:State.Name,PublicIP:PublicIpAddress}"