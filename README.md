# AWS-Security-Misconfiguration-Lab

## Overview
This lab demonstrates how to identify and exploit common AWS misconfigurations using the AWS CLI and a custom IAM audit script.  
I checked for insecure IAM, EC2, and S3 configurations, and verified how data in a public S3 bucket can be accessed.

---

## Tools Used
- **AWS CLI** – to interact with AWS services from the command line  
- **Python 3** – to run the IAM audit script (`iam_audit.py`)  
- **Custom IAM Audit Script** – to find misconfigurations in IAM, EC2, and S3

---

## Steps Performed

### 1. Run IAM Audit Script
Detected:
- Users without MFA
- Users with `AdministratorAccess` policy attached
- Security groups open to the world (port 22)
- S3 bucket with a public ACL

---

### 2. Create and Upload a File to S3
I created a test file and uploaded it to the vulnerable public S3 bucket with public read access.

Command used:
```bash
echo hello > hello.txt
aws s3 cp hello.txt s3://<BUCKET_NAME>/hello.txt --acl public-read --profile lab
```
### 3. Verify Bucket ACL
We checked the bucket ACL to confirm it grants READ access to the AllUsers group, meaning the file is publicly accessible.

Command used:
```bash
aws s3api get-bucket-acl --bucket <BUCKET_NAME> --profile lab
```
Security Risk
A public S3 bucket allows anyone on the internet to access its contents without authentication.
If sensitive data is stored in such a bucket, it could be downloaded by unauthorized users, leading to potential data breaches.

Key Takeaways
Always enable MFA for IAM users, especially administrators.
Avoid granting public access to S3 buckets unless explicitly needed.
Restrict security group rules to specific IPs, not 0.0.0.0/0.
