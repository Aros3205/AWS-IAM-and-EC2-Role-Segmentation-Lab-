# AWS IAM & EC2 Role Segmentation Project

## 📌 Overview
This project demonstrates how Identity & Access Management (IAM) principles are applied within AWS to enforce role-based access control (RBAC) between cloud resources. I designed two IAM users and two EC2 instances representing separate departments (Sales and Marketing) and applied security policies to segment privileges, preventing unauthorized resource access across departments.

This foundational project strengthens AWS security concepts that are essential for Cybersecurity Analysts—such as least privilege, IAM policy enforcement, cloud resource access auditing, and privilege segregation.

---

## 🔥 Project Objectives

- Configure secure IAM users
- Launch and configure EC2 instances
- Create an S3 bucket for each department
- Assign permissions based on job role
- Prevent unauthorized cross-access
- Apply least-privilege principles
- Audit activity using CloudTrail

---

## 🏗 Architecture Summary

| AWS Service | Purpose |
|------------|---------|
| IAM | Identity, access, security |
| EC2 | Compute resource (department systems) |
| S3 | Storage for departmental files |
| CloudTrail | Logging and auditing |
| IAM Policies | Access control enforcement |

---

## 🚀 Step-by-Step Implementation

### **1️⃣ Create IAM Users**
- Go to **IAM → Users → Create user**
- Create:
  - `Sales_User`
  - `Marketing_User`

🔹 Users created without admin privileges  
🔹 Will receive access only through custom IAM policies  

> 📌 Screenshot Placeholder  

![EC2 Role Segmentation – Instance Overview](https://raw.githubusercontent.com/Aros3205/AWS-IAM-and-EC2-Role-Segmentation-Lab-/main/screenshot/Screenshot%202025-12-05%20084741.png)

>![Screenshot](https://raw.githubusercontent.com/Aros3205/AWS-IAM-and-EC2-Role-Segmentation-Lab-/main/screenshot/Screenshot%202025-12-05%20084003.png)

![Screenshot](https://raw.githubusercontent.com/Aros3205/AWS-IAM-and-EC2-Role-Segmentation-Lab-/main/screenshot/Screenshot%202025-12-05%20084102.png)

### **2️⃣ Launch EC2 Instances**
- Navigate to **EC2**
- Create two instances:
  - `Sales-Instance`
  - `Marketing-Instance`

![Screenshot](https://github.com/Aros3205/AWS-IAM-and-EC2-Role-Segmentation-Lab-/raw/main/screenshot/Screenshot%202025-12-05%20095606.png)

![AWS IAM & EC2 – Example Screenshot](screenshot/Screenshot%202025-12-05%20095134.png)

![Screenshot](https://raw.githubusercontent.com/Aros3205/AWS-IAM-and-EC2-Role-Segmentation-Lab-/main/screenshot/Screenshot%202025-12-05%20094504.png)


### **3️⃣ Create Department S3 Buckets**
Create buckets:
- `rg-sales-bucket`
- `rg-marketing-bucket`

These will later be permission restricted.

> 📌 Screenshot Placeholder  
> `![Create S3 Bucket](screenshots/s3bucket.png)`

---

### **4️⃣ Build IAM Security Policies**
Assign:
- Sales user → access only sales resources
- Marketing user → access only marketing resources

Example rules:
- allow listing own bucket
- deny access to other department bucket
- deny modify
- allow read/write only inside own folder

> 📌 Screenshot Placeholder  
> `![IAM Policy](screenshots/iam-policy.png)`

---

### **5️⃣  Create CloudTrail **
> 📌 Screenshot Placeholder  
> `![CloudTrail](screenshots/cloudtrail.png)`

---

## 🛡 What I Learned

✔ IAM security  
✔ EC2 access control  
✔ Role segmentation  
✔ AWS resource isolation  
✔ CloudTrail auditing  
✔ Principle of least privilege  
✔ User-to-instance segmentation  

This directly strengthens cybersecurity skills such as:
- security governance
- access control
- identity management
- cloud hardening
- insider threat prevention

---

## 🧠 Why This Matters in Cybersecurity

This project demonstrates how security teams restrict access inside cloud environments using IAM controls. Understanding IAM is essential for preventing unauthorized access, insider threats, privilege escalation, and data exposure inside enterprise cloud infrastructures.

---

## 🔮 Next Phase (Coming Soon)

- MFA configuration
- SCP policies (Organizations)
- IAM Groups for department roles
- Restricting console access
- Restricting SSH access
- CloudWatch monitoring
- GuardDuty

---


## 🔗 Connect
More cybersecurity labs are continuously being added. Follow this repository and my LinkedIn for updates.

