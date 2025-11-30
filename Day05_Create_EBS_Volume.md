# 💾 Day 05: Create EBS Volume

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team needs additional storage capacity for their upcoming database migration. They require a decoupled storage unit (EBS Volume) that can be attached to instances later as needed.

**Task Requirements:**
1.  **Resource:** EBS Volume.
2.  **Name Tag:** `devops-volume`.
3.  **Type:** `gp3` (General Purpose SSD).
4.  **Size:** `2 GiB`.

## 💻 Infrastructure Details
| Resource | Name Tag | Type | Size | Zone |
| :--- | :--- | :--- | :--- | :--- |
| **EBS Volume** | `devops-volume` | `gp3` | `2 GiB` | `us-east-1a` |

---

## 🛠️ Step-by-Step Implementation

### Method: AWS Console (GUI)

**1. Navigate to EC2:**
* Go to **EC2 Dashboard** -> **Elastic Block Store** -> **Volumes**.
* Click **Create volume**.

**2. Configure Settings:**
* **Volume type:** `General Purpose SSD (gp3)`
* **Size (GiB):** `2`
* **Availability Zone:** `us-east-1a` (or any available zone in the region).

**3. Add Tags:**
To fulfill the name requirement, we must tag the resource during creation.
* **Key:** `Name`
* **Value:** `devops-volume`

**4. Finish:**
* Click **Create volume**.

---

### ⌨️ CLI Reference (Infrastructure as Code)
*For reference, this is how you would perform the same task using the AWS CLI:*

```bash
aws ec2 create-volume \
    --volume-type gp3 \
    --size 2 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=devops-volume}]'