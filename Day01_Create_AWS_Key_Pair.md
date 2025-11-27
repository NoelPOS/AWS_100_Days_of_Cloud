# 🔑 Day 01: Create AWS Key Pair (Retry)

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (Access Keys, Secrets) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1` for this lab.

## 📋 Scenario
The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps.

**Task Requirements:**
1.  Create an EC2 Key Pair.
2.  **Name:** `nautilus-kp` (⚠️ Critical: Name varies by attempt).
3.  **Type:** `rsa`.
4.  **Region:** `us-east-1`.

## 💻 Infrastructure Details
| Resource | Name | Type |
| :--- | :--- | :--- |
| **Key Pair** | `nautilus-kp` | `RSA` |
| **Region** | `us-east-1` | - |

---

## 🛠️ Step-by-Step Implementation

### Step 1: Login to AWS Console
Since CLI permissions (IAM) were restricted in previous attempts, we used the AWS Console GUI for reliability.

* **Region Check:** Ensured `us-east-1` (N. Virginia) was selected.

### Step 2: Create the Key Pair (GUI)
1.  Navigate to **EC2** Service.
2.  Locate **Network & Security** -> **Key Pairs** in the left sidebar.
3.  Click **Create key pair**.

**Configuration:**
* **Name:** `nautilus-kp`
* **Type:** `RSA`
* **Format:** `.pem`

4.  Click **Create**.

### Step 3: Verification
* Verified that `nautilus-kp` appeared in the Key Pair list within the AWS Console.
* The `.pem` file was successfully downloaded to the local machine.

## 🧠 What We Learned
* **Attention to Detail:** Lab requirements (like resource names) can change every time you restart a lab. Always double-check the `Name` requirement.
* **Alternative Methods:** When the CLI is blocked due to IAM permissions (Permission Denied on `CreateAccessKey`), the AWS Console (GUI) is a valid and often easier fallback for creating simple resources like Key Pairs.