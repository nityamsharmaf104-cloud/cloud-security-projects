# Prowler Security Audit Lab

Fourth project in my cloud security series. This one is different from 
the others — instead of manually checking individual services, I used 
Prowler to automatically scan my entire AWS account for security issues. 
301 checks, 2 minutes, real findings.

## What is Prowler

Prowler is an open source security tool that runs hundreds of checks 
against your AWS account based on industry frameworks like CIS, NIST, 
and AWS Foundational Security Best Practices. It's used by real security 
teams to audit cloud environments at scale.

## Setup

- Python 3.13.3
- AWS CLI v2.35.2 configured with least-privilege IAM credentials
- Prowler 3.11.3 installed via pip
- Ran against ap-south-1 (Mumbai) region only
- Access keys generated for the scan and deleted immediately after

## Scan Results

| Metric | Count |
|--------|-------|
| Total checks | 301 |
| Passed | 70 |
| Failed | 71 |
| Total resources scanned | 33 |

## Key Findings

### Finding 1 — S3 Account Level Public Access Block not configured (High)
Block Public Access was not enabled at the account level — only at 
individual bucket level. This means any new bucket created in future 
could be made public unless the account-level block is enforced.

**Remediation:** Enable S3 Block Public Access at account level via 
S3 Console → Block Public Access settings for this account → 
Block all public access → Save.

---

### Finding 2 — MFA not enabled on IAM users (High)
Auditor-user, developer-user, and Intern all have console passwords 
but no MFA enabled. If any of these credentials were compromised, 
an attacker would have direct console access with no second factor.

**Remediation:** Enable MFA for each IAM user via IAM → Users → 
Security credentials → Assign MFA device.

---

### Finding 3 — EC2 Default Security Group allows all traffic (High)
The default VPC security group has rules that allow unrestricted 
inbound and outbound traffic. Any EC2 instance launched into this 
security group would be exposed.

**Remediation:** Remove all inbound and outbound rules from the 
default security group. Never use the default security group for 
actual workloads.

---

### Finding 4 — CloudWatch alarms not configured (Medium — 15 findings)
No CloudWatch alarms are set up for key security events like root 
account usage, IAM policy changes, or unauthorized API calls.

**Remediation:** Create CloudWatch metric filters and alarms for 
CIS Benchmark recommended events.

## What Passed

- CloudTrail multi-region logging enabled ✅
- Root account MFA enabled ✅
- Nityam-admin MFA enabled ✅
- IAM Access Analyzer enabled ✅
- No public S3 buckets detected ✅

## What I Learned

Running Prowler for the first time is humbling. I had spent three 
projects manually securing specific things — IAM policies, S3 buckets, 
access analysis — and thought the account was in decent shape. Prowler 
found 71 failures in under 2 minutes. That's the point. Manual checks 
don't scale and human attention misses things. Automated scanning isn't 
a replacement for understanding security — but it's how you make sure 
nothing slips through at scale.

The MFA finding was the most interesting one personally. I had enabled 
MFA on root and Nityam-admin but completely forgot about the three 
lab users. Prowler caught it immediately. In a real environment those 
three accounts could have been actual employees with no second factor 
on their credentials.

## Tools Used
- Prowler 3.11.3
- AWS CLI v2.35.2
- Python 3.13.3
- AWS IAM
- AWS S3
- AWS CloudTrail
