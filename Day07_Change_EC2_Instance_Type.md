# 🔄 Day 07: Change EC2 Instance Type

> **⚠️ Dynamic Environment Warning**
> Please note that the specific details in this guide (IPs, Usernames, Passwords) are generated dynamically by the lab environment.
> * **Region:** Always use `us-east-1`.

## 📋 Scenario
The Nautilus DevOps team identified that the `datacenter-ec2` instance is underutilized. To optimize costs, they have decided to downsize the instance type.

**Task Requirements:**
1.  **Target Instance:** `datacenter-ec2`.
2.  **Action:** Change type from `t2.micro` to `t2.nano`.
3.  **Final State:** The instance must be `running`.

## 💻 Infrastructure Details
| Resource | Name | Current Type | Target Type |
| :--- | :--- | :--- | :--- |
| **EC2 Instance** | `datacenter-ec2` | `t2.micro` | `t2.nano` |

---

## 🛠️ Implementation Method 1: AWS Console (GUI)

**1. Locate the Instance:**
* Navigate to **EC2** -> **Instances**.
* Select the instance named `datacenter-ec2`.

**2. Stop the Instance:**
* *Note:* You cannot change the type of a running instance.
* Click **Instance state** (top menu) -> **Stop instance**.
* Click **Stop** to confirm.
* *Wait* until the "Instance state" column shows `Stopped`.

**3. Change Instance Type:**
* With the instance still selected, click **Actions** (top menu).
* Go to **Instance settings** -> **Change instance type**.
* **Instance type:** Select `t2.nano` from the dropdown.
* Click **Apply**.

**4. Start the Instance:**
* Click **Instance state** -> **Start instance**.
* Wait for the state to turn `Running`.

---

## 🛠️ Implementation Method 2: AWS CLI
*Best for automation. This script stops the instance, waits for it to stop, resizes it, starts it back up, and verifies the change.*

```bash
# --- Step 1: Get Instance ID ---
INSTANCE_ID=$(aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=datacenter-ec2" \
    --region us-east-1 \
    --query "Reservations[0].Instances[0].InstanceId" \
    --output text)

echo "Targeting Instance: $INSTANCE_ID"

# --- Step 2: Stop the Instance ---
echo "Stopping instance..."
aws ec2 stop-instances --instance-ids $INSTANCE_ID

# Wait until it is fully stopped (Critical for the next step)
aws ec2 wait instance-stopped --instance-ids $INSTANCE_ID
echo "Instance stopped."

# --- Step 3: Change Instance Type ---
aws ec2 modify-instance-attribute \
    --instance-id $INSTANCE_ID \
    --instance-type "{\"Value\": \"t2.nano\"}"

echo "Instance type updated to t2.nano."

# --- Step 4: Start the Instance ---
echo "Starting instance..."
aws ec2 start-instances --instance-ids $INSTANCE_ID

# Wait until it is running
aws ec2 wait instance-running --instance-ids $INSTANCE_ID

# --- Step 5: Verification ---
echo "Verifying..."
aws ec2 describe-instances \
    --instance-ids $INSTANCE_ID \
    --query "Reservations[0].Instances[0].[InstanceId, InstanceType, State.Name]"