# 🛡️ Day 02: Configure Security Group

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team is continuing the migration. They need to secure the upcoming Application Servers by defining firewall rules. You are tasked with creating a Security Group that allows web traffic and remote management access.

**Task Requirements:**
1.  **Name:** `xfusion-sg`.
2.  **Description:** `Security group for Nautilus App Servers`.
3.  **VPC:** Default VPC.
4.  **Inbound Rules:**
    * Allow `HTTP` (Port 80) from `0.0.0.0/0`.
    * Allow `SSH` (Port 22) from `0.0.0.0/0`.

## 💻 Infrastructure Details
| Resource | Name | Type | Inbound Ports |
| :--- | :--- | :--- | :--- |
| **Security Group** | `xfusion-sg` | `Security Group` | `80`, `22` |
| **Region** | `us-east-1` | - | - |

---

## 🛠️ Step-by-Step Implementation

### Method: AWS Console (GUI)
Since lab permissions for IAM often block CLI key creation, the Console is the most reliable method.

**1. Navigate to EC2:**
* Go to **EC2 Dashboard** -> **Network & Security** -> **Security Groups**.
* Click **Create security group**.

**2. Configure Details:**
* **Name:** `xfusion-sg`
* **Description:** `Security group for Nautilus App Servers`
* **VPC:** Selected default VPC.

**3. Set Inbound Rules:**
We add rules to allow traffic *into* the instances.

| Type | Protocol | Port Range | Source | CIDR |
| :--- | :--- | :--- | :--- | :--- |
| **HTTP** | TCP | 80 | Anywhere | `0.0.0.0/0` |
| **SSH** | TCP | 22 | Anywhere | `0.0.0.0/0` |

**4. Finish:**
* Click **Create security group**.

---

### ⌨️ CLI Reference (Infrastructure as Code)
*For reference, this is how you would perform the same task using the AWS CLI if you had Access Keys:*

```bash
# 1. Create the Security Group
aws ec2 create-security-group \
    --group-name xfusion-sg \
    --description "Security group for Nautilus App Servers" \
    --vpc-id vpc-xxxxxxx

# 2. Authorize Inbound Rules (HTTP & SSH)
aws ec2 authorize-security-group-ingress \
    --group-name xfusion-sg \
    --protocol tcp --port 80 --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
    --group-name xfusion-sg \
    --protocol tcp --port 22 --cidr 0.0.0.0/0