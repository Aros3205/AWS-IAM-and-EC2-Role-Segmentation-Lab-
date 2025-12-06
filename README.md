# AWS IAM & EC2 Role Segmentation – Project

## 🎯 Objective
This project focuses on building foundational AWS security skills by configuring IAM, launching EC2 instances, creating S3 buckets, and preparing the environment for role-based access segmentation. The ultimate goal is to enforce departmental separation between AWS users such as **Sales** and **Marketing**, ensuring least-privilege access based on business roles.

---

## 🧰 AWS Services Involved
- **IAM** (Users, Groups, Policies)
- **EC2**
- **S3**
- **CloudTrail**
- **AWS Console Security Settings**

---

## 🏗️ Architecture Overview
The project simulates two departments inside an organization:

| Department | AWS IAM User | EC2 Instance | S3 Resource |
|-----------|--------------|--------------|-------------|
| Sales     | sales-user   | sales-instance | sales S3 bucket |
| Marketing | marketing-user | (To be added later) | marketing S3 bucket (later) |

This setup prepares us for eventual access segmentation where each department will only access its related compute and storage resources.

---

# 🚀 Step-by-Step Implementation

---

## **Step 1 — Create IAM Users**
### Actions
- Open IAM console
- Create IAM user (example: `sales-user`)
- Assign login access
- Save login credentials

🔐 Purpose  
Users authenticate separately, ensuring accountability and identity tracking.

---

## **Step 2 — Create IAM Groups**
### Actions
- Navigate to IAM Groups
- Create groups for each department
  - Sales
  - Marketing (later)

🧭 Why?
Groups simplify permission assignments and future enforcement of departmental separation.

---

## **Step 3 — Launch EC2 Instance**
### Actions
- Go to EC2 console
- Launch a new instance
- Choose Amazon Linux or Windows (as preferred)
- Name example: **sales-instance**
- Configure key pair
- Launch instance

💡 NOTE  
At this stage, only **one instance (Sales)** is required. Marketing EC2 instance will be created in the next phase when enforcing cross-department restrictions.

---

## **Step 4 — Create S3 Bucket**
### Actions
- Go to S3 Console
- Create bucket
- Bucket name example:
  - `rg-sales-bucket`

📦 Purpose  
Each department will later store data separately. Segregated buckets allow granular policy enforcement.

---

## Step 5 — Create CloudTrail (Created Only)
### Status
CloudTrail has been created but not fully configured for logging policies.

🔜 Will be completed in next project.

---

## Step 6 — IAM Role Segmentation (Pending)
### Future actions
- Restrict Sales user from accessing Marketing instance
- Restrict Marketing user from accessing Sales instance
- Enforce least privilege per department

🔥 Will be implemented in Project 3.

---

# 🛠️ What Has Been Completed So Far
✔ IAM users  
✔ IAM group (Sales)  
✔ EC2 instance (Sales)  
✔ S3 bucket (Sales)  
✔ CloudTrail created (partial)  

---

# 📌 What Will Be Completed Next
➡ CloudTrail Logging Enforcement  
➡ Marketing EC2  
➡ IAM segmentation policies  
➡ Access restriction between departments

---

# 💡 Key Takeaways
- IAM identity-based security is the foundation of AWS access control
- EC2 resources must be isolated per business unit
- S3 bucket separation helps enforce clear data boundaries
- CloudTrail auditing is mandatory for monitoring corporate environments

---

# 🧠 Skills Demonstrated
- IAM identity creation
- Cloud resource provisioning
- AWS security fundamentals
- Preparing role segmentation structures
- Practical cloud governance understanding

---

## 🔜 Next Project
**AWS IAM Access Control and Department Segmentation**

This will finalize:
- Cross-account restrictions
- IAM permission boundaries
- Resource-level conditions

---

# ✔ Status
🟡 In-progress  
(Current phase completed successfully – moving to access segmentation soon)



