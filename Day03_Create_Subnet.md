# 🕸️ Day 03: Create a Subnet

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team is segmenting the network to prepare for the migration. Dividing a large network (VPC) into smaller networks (Subnets) allows for better organization, security, and traffic management.

**Task Requirements:**
1.  **Resource:** VPC Subnet.
2.  **Name:** `nautilus-subnet`.
3.  **VPC:** Default VPC.
4.  **Region:** `us-east-1`.

## 💻 Infrastructure Details
| Resource | Name | VPC | CIDR Block |
| :--- | :--- | :--- | :--- |
| **Subnet** | `nautilus-subnet` | `Default` | `172.31.250.0/24` (or available) |

---

## 🛠️ Step-by-Step Implementation

### Method: AWS Console (GUI)

**1. Navigate to VPC:**
* Go to **VPC Dashboard** -> **Virtual Private Cloud** -> **Subnets**.
* Click **Create subnet**.

**2. Configure Details:**
* **VPC ID:** Select the existing Default VPC.
* **Subnet Name:** `nautilus-subnet`.
* **Availability Zone:** `No Preference` (or `us-east-1a`).

**3. Define CIDR Block:**
We need an IP range that does not conflict with existing subnets in the default VPC.
* **IPv4 CIDR:** `172.31.250.0/24`
* *Logic:* The default VPC usually spans `172.31.0.0/16`. Choosing a high number like `250` in the third octet avoids overlapping with the default subnets (which are usually `0`, `16`, `32`, etc.).

**4. Finish:**
* Click **Create subnet**.

---

### ⌨️ CLI Reference (Infrastructure as Code)
*For reference, this is how you would perform the same task using the AWS CLI:*

```bash
# 1. Get the Default VPC ID
VPC_ID=$(aws ec2 describe-vpcs --filter "Name=isDefault,Values=true" --query "Vpcs[0].VpcId" --output text)

# 2. Create the Subnet
aws ec2 create-subnet \
    --vpc-id $VPC_ID \
    --cidr-block 172.31.250.0/24 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=nautilus-subnet}]'