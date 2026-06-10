# AWS IAM Least Privilege Lab
A hands on cloud security project demonstrating IAM best practices,
role based access control, and audit logging on AWS. I Built this as my first hands-on cloud security project 
during my Year 1 summer break to get myself familiarized with the working and console of AWS.

## Project Overview
I set up three different AWS users — an auditor, a developer, and an intern 
each with different levels of access. Then I tested whether each one could actually do things they shouldn't be able to.

## Architecture

|     User     |     Group      | Policy |          Access Level                    |
|--------------|----------------|--------|------------------------------------------|
| Auditor-user |Auditors       | ReadOnlyAccess | View all resources, no modifications |
| Developer-user | Developers | AmazonS3FullAccess, AmazonEC2FullAccess | S3 and EC2 only |
| Intern | Interns | IAMUserChangePassword | Password change only |

## Security Controls Implemented
- MFA enabled on root account and admin user
- Root account not used for any operational tasks
- IAM admin user created for all daily work
- Users assigned to groups, not policies directly
- CloudTrail enabled across all regions for audit logging
- Zero-spend billing alarm configured

## Access Validation Results

### Auditor-user
- ✅ Can view S3 buckets (ListBuckets allowed)
- ❌ Cannot create S3 bucket (s3:CreateBucket denied)
- ❌ Cannot create IAM users (iam:CreateUser denied)

### Developer-user
- ✅ Can create S3 buckets (AmazonS3FullAccess)
- ❌ Cannot view IAM users (iam:ListUsers denied)
- ❌ No access to identity management

### Intern
- ✅ Can log in and change password
- ❌ No access to any AWS services

## Tools Used
- AWS IAM
- AWS CloudTrail
- AWS S3
- AWS Free Tier

## What I learned
Before this I understood IAM conceptually. After building and breaking it myself, 
I understand why attaching policies to groups instead of users actually matters at scale.
If permissions were attached to individual users, removing or changing access for 50 people would mean 50 manual changes.
With groups, it's one change.

