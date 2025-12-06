# AWS-IAM-and-EC2-Role-Segmentation-Lab-
[07:06, 12/6/2025] Mr Tolu.🇺🇸: This project demonstrates how to design *segmented access control* in AWS using:

- *IAM users & groups*
- *EC2 instances (per department)*
- *S3 + CloudTrail for logging*
- *Least privilege policies* that prevent cross-department access

The goal:  
> Sales users should only manage *Sales resources, and Marketing users should only manage **Marketing resources* — enforced through IAM policies and resource tags.

---

## 🌐 High-Level Architecture

- *IAM*
  - SalesUser and MarketingUser
  - IAM groups: SalesGroup, MarketingGroup
  - Custom IAM policies for each group

- *EC2*
  - sales-ec2 instance (for Sales department)
  - marketing-ec2 instance (for Marketing department)
  - Tagged by department for policy enforcement

- *S3*
  - rg-sales-bucket (example name) for storing logs / objects

- *CloudTrail*
  - One trail capturing management events across the account
  - Sends logs to S3 for auditing IAM and EC2 actions

---

## 🔐 1. IAM User & Group Setup

### 1.1 Create IAM Users

In the AWS Console:

1. Go to *IAM → Users → Add users*
2. Create two users, for example:
   - SalesUser
   - MarketingUser
3. Select:
   - ✅ *Provide user access to the AWS Management Console*
   - Choose *auto-generated password* or set custom
   - ✅ User must change password on first sign-in (optional)

> 📸 Screenshot example: IAM user list with SalesUser and MarketingUser  
![IAM Users](screenshots/iam-users.png)

---

### 1.2 Create IAM Groups

1. Go to *IAM → User groups → Create group*
2. Create:
   - SalesGroup
   - MarketingGroup
3. For now, you can skip attaching policies (we’ll add custom ones later).

> 📸 Screenshot example: IAM groups list with SalesGroup and MarketingGroup  
![IAM Groups](screenshots/iam-groups.png)

---

### 1.3 Add Users to Groups

1. Open *SalesGroup → Add users*
   - Add SalesUser
2. Open *MarketingGroup → Add users*
   - Add MarketingUser

> 📸 Screenshot example: SalesGroup with SalesUser attached  
![SalesGroup Members](screenshots/sales-group-members.png)

---

## 💻 2. EC2 Instance Setup (Per Department)

### 2.1 Create Sales EC2 Instance

1. Go to *EC2 → Instances → Launch instances*
2. Name: sales-ec2
3. OS: e.g. *Amazon Linux 2* or *Windows Server* (depending on your lab)
4. Instance type: t2.micro or t3.micro (free tier–eligible)
5. Key pair: create or select an existing one
6. Network:
   - Default VPC
   - Subnet: any available
7. Tags (VERY IMPORTANT):
   - Department = Sales
   - Name = sales-ec2

> 📸 Screenshot example: EC2 launch summary showing tags for Sales  
![Sales EC2 Instance](screenshots/sales-ec2-instance.png)

---

### 2.2 Create Marketing EC2 Instance

Repeat the same process:

- Name: marketing-ec2
- Tags:
  - Department = Marketing
  - Name = marketing-ec2

> 📸 Screenshot example: EC2 list with both Sales & Marketing instances  
![Both EC2 Instances](screenshots/ec2-both-instances.png)

You can keep only one instance running at a time to save cost.

---

## 📦 3. S3 Bucket for Logs (Optional but Recommended)

To centralize logs (CloudTrail + app logs):

1. Go to *S3 → Create bucket*
2. Name example: rg-sales-bucket (must be globally unique)
3. Region: same as EC2 if possible
4. Block Public Access: ✅ keep public access blocked
5. Create bucket

> 📸 Screenshot example: S3 bucket list with rg-sales-bucket  
![S3 Bucket](screenshots/s3-rg-sales-bucket.png)

You can later use this bucket as a *CloudTrail destination*.

---

## 🕵️‍♂️ 4. Enable CloudTrail for Auditing

1. Go to *CloudTrail → Trails → Create trail*
2. Trail name: rg-org-trail
3. Storage location:
   - Choose existing bucket: rg-sales-bucket
4. Event type:
   - ✅ Management events (Read + Write)
5. Create trail

> 📸 Screenshot example: CloudTrail trail overview page  
![CloudTrail Trail](screenshots/cloudtrail-trail-overview.png)

Now *every action* (like starting/stopping EC2, changing IAM, etc.) is logged and visible in *Event history*.

> 📸 Screenshot example: Event history showing EC2 & IAM actions  
![CloudTrail Event History](screenshots/cloudtrail-event-history.png)

---

## 🧱 5. IAM Policies for Department Segmentation

Now we enforce *who can manage what*.

### 5.1 Design Principle

- *SalesGroup*:
  - Can Start/Stop/Describe only sales-ec2
  - Cannot affect marketing-ec2
- *MarketingGroup*:
  - Can Start/Stop/Describe only marketing-ec2
  - Cannot affect sales-ec2

We can do this using *IAM policy conditions on tags*.

---

### 5.2 Example Policy for SalesGroup

Create a new policy:

1. Go to *IAM → Policies → Create policy → JSON*
2. Use something like:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowDescribeInstances",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AllowSalesInstanceControl",
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:RebootInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Department": "Sales"
        }
      }
    }
  ]
}
[07:07, 12/6/2025] Mr Tolu.🇺🇸: 3.	Name it: Sales-EC2-Access-Policy

📸 Screenshot example: JSON policy editor for Sales group
![Sales IAM Policy](screenshots/iam-policy-sales.png)

Attach this policy to SalesGroup.

⸻

5.3 Example Policy for MarketingGroup

Similar concept, but for Marketing:
[07:07, 12/6/2025] Mr Tolu.🇺🇸: {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowDescribeInstances",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    },
    {
      "Sid": "AllowMarketingInstanceControl",
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:RebootInstances"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Department": "Marketing"
        }
      }
    }
  ]
}
[07:08, 12/6/2025] Mr Tolu.🇺🇸: Name it: Marketing-EC2-Access-Policy, and attach to MarketingGroup.

📸 Screenshot example: Policy attached to MarketingGroup
![Marketing IAM Policy](screenshots/iam-policy-marketing.png)

⸻

🧪 6. Testing the Segmentation

6.1 Log in as SalesUser
	1.	Log out of root account
	2.	Log in at: IAM sign-in URL with:
	•	Username: SalesUser
	3.	Go to EC2 → Instances

Expected:
	•	✅ SalesUser can see both instances (because Describe is allowed)
	•	✅ Can Start/Stop sales-ec2
	•	❌ Gets AccessDenied when trying to Start/Stop marketing-ec2

📸 Screenshot example: Access Denied on Marketing instance
![SalesUser Access Denied](screenshots/salesuser-access-denied-marketing.png)

⸻

6.2 Log in as MarketingUser

Repeat with MarketingUser.

Expected:
  .	✅ Can manage marketing-ec2
	•	❌ Cannot start/stop sales-ec2

📸 Screenshot example: Marketing user blocked from sales instance
![MarketingUser Access Denied](screenshots/marketinguser-access-denied-sales.png)

⸻

📊 7. Verifying in CloudTrail

In CloudTrail → Event history, filter by:
	•	Event source: ec2.amazonaws.com
	•	User name: SalesUser or MarketingUser

You should see:
	•	Successful actions on the allowed instance
	•	AccessDenied events for blocked actions

📸 Screenshot example: CloudTrail showing both success and denied events
![CloudTrail Access Logs](screenshots/cloudtrail-access-denied.png)

This proves your IAM + EC2 + tagging strategy is working as designed.

⸻

✅ Summary

In this lab you:
	•	Created dedicated IAM users & groups for Sales and Marketing
	•	Deployed separate EC2 instances per department
	•	Applied resource tags to drive access control
	•	Built least-privilege IAM policies allowing each group to manage only their own resources
	•	Enabled CloudTrail and optionally S3 logging for auditing all actionarketing…
