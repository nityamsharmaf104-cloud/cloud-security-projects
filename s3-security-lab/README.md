# S3 Bucket Security Misconfigurations Lab

Second project in my cloud security series. This one is about S3 buckets —
specifically how easy it is to accidentally expose sensitive data to the entire
internet and how to fix it.

## Why This Matters

Some of the biggest cloud data breaches happened because of misconfigured S3 buckets.
Not hacking, not malware — just a wrong setting. I wanted to see exactly how that
happens and what it looks like from both sides.

## What I Did

Created an S3 bucket, disabled public access controls, added a permissive bucket
policy, uploaded a file called sensitive-data.txt — then opened an incognito window
and accessed the file with no login, no credentials, nothing. Just the URL.
Then fixed it and verified the fix worked.

## Misconfiguration Details

| Setting | Insecure State | Secure State |
|---------|---------------|--------------|
| Block Public Access | Disabled | Enabled |
| Bucket Policy | Principal: * (everyone) | No public policy |
| Object Visibility | Publicly readable via URL | Access Denied |

## Attack Scenario

A publicly accessible S3 bucket with sensitive data can be discovered by:
- Automated scanning tools (GrayhatWarfare, bucket finder tools)
- Google dorking — `site:s3.amazonaws.com`
- Brute forcing common bucket names

Once discovered, any file inside is downloadable with no authentication.

## Remediation Steps

1. Enable **Block all public access** on the bucket
2. Remove any bucket policy with `"Principal": "*"`
3. Audit existing buckets using AWS Config or Prowler
4. Set S3 bucket public access block at the **account level** to prevent future misconfigurations

## Tools Used
- AWS S3
- AWS IAM (bucket policies)
- AWS CloudTrail (logging all access attempts)

## What I Learned

The scary part is how simple this misconfiguration is to make. Two settings,
one policy, and your data is exposed. What stuck with me is that the fix is
equally simple — but only if someone is actually checking. That's the job.
