# IAM Access Analyzer Lab

Third project in my cloud security series. This one is about automated 
detection instead of manually checking every resource, I used AWS IAM 
Access Analyzer to automatically find externally accessible resources in my account.

## Why This Matters

In a real company you might have hundreds of S3 buckets, IAM roles, and 
other resources. Manually checking each one for misconfigurations isn't 
realistic. Access Analyzer does it automatically and alerts you the moment 
something is exposed that shouldn't be.

## What I Did

Created an Access Analyzer, then intentionally applied a public bucket 
policy to an S3 bucket. The analyzer detected it automatically as an 
active finding. Fixed the misconfiguration and verified the finding resolved.

## What is IAM Access Analyzer

IAM Access Analyzer continuously monitors your AWS environment and
automatically generates findings when a resource can be accessed from
outside your account. It analyzes resource based policies on S3 buckets,
IAM roles, KMS keys, and more.

## Finding Details

| Field | Value |
|-------|-------|
| Resource | security-lab-nityam-2026 (S3 Bucket) |
| Access type | Public access |
| Principal | All Principals (*) |
| Shared through | Bucket policy |
| Access level | Read |

## What the Finding Means

Any person on the internet could read objects in this bucket.
The analyzer detected this automatically without manual checking
this is how security teams monitor hundreds of buckets at scale.

## Remediation

1. Removed the public bucket policy (`Principal: *`)
2. Re-enabled Block Public Access on the bucket
3. Verified finding resolved — 0 active findings confirmed

## Tools Used
- AWS IAM Access Analyzer
- AWS S3
- AWS IAM (bucket policies)
- AWS CloudTrail

## What I Learned

Manual checking doesn't scale. A security engineer at a company with 
200 S3 buckets can't open each one and verify the permissions daily. 
Access Analyzer does that continuously in the background and only 
surfaces what actually needs attention. That's the difference between 
reactive and proactive security.
