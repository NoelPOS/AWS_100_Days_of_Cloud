# 🌐 Day 04: Allocate Elastic IP

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team requires a static public IP address for one of their public-facing servers. Unlike standard public IPs which change when an instance is stopped, this IP must remain constant to allow for reliable DNS mapping.

**Task Requirements:**
1.  **Resource:** Elastic IP (EIP).
2.  **Name Tag:** `devops-eip`.
3.  **Region:** `us-east-1`.

## 💻 Infrastructure Details
| Resource | Name Tag | Type |
| :--- | :--- | :--- |
| **Elastic IP** | `devops-eip` | `IPv4` |

---

## 🛠️ Step-by-Step Implementation

### Method: AWS Console (GUI)

**1. Navigate to EC2:**
* Go to **EC2 Dashboard** -> **Network & Security** -> **Elastic IPs**.
* Click **Allocate Elastic IP address**.

**2. Configure Settings:**
* **Network Border Group:** `us-east-1` (Default).
* **Public IPv4 address pool:** Amazon's pool of IPv4 addresses.

**3. Add Tags (Critical):**
To fulfill the "Name" requirement, we must tag the resource.
* **Key:** `Name`
* **Value:** `devops-eip`

**4. Finish:**
* Click **Allocate**.

---

### ⌨️ CLI Reference (Infrastructure as Code)
*For reference, this is how you would perform the same task using the AWS CLI:*

```bash
aws ec2 allocate-address \
    --domain vpc \
    --tag-specifications 'ResourceType=elastic-ip,Tags=[{Key=Name,Value=devops-eip}]'