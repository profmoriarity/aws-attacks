# Public Snapshot Enumeration in AWS

## Overview
Public snapshot enumeration is a security risk that arises when organizations misconfigure their AWS resources, making snapshots public. Attackers can use basic information to locate these public snapshots, potentially exposing sensitive data.

## Prerequisites for Enumeration

1. **Public S3 Bucket**: Requires a publicly accessible S3 bucket allowing listing, which reveals the **AWS account ID**.
   - Necessary permissions:
     - `s3:GetObject` - Permission to download files from the bucket.
     - `s3:ListBucket` - Permission to list the contents of the bucket.

## Step-by-Step Enumeration

1. **Identify Account ID Using s3-account-search Tool**
   - First, install the tool:
     ```bash
     pip3 install s3-account-search
     ```
   - Use `s3-account-search` to locate the account ID:
     ```bash
     s3-account-search arn:aws:iam::123456789012:role/LeakyBucket example-bucket-name
     ```
   - Here:
     - `LeakyBucket` is an example of an exposed role.
     - Replace `example-bucket-name` with the actual bucket name.

2. **Determine Bucket Region Using cURL**
   - After identifying the bucket, use `cURL` to find its region:
     ```bash
     curl -I https://example-bucket-name.s3.amazonaws.com
     ```
   - The region will be included in the response header.
  
    ![image](https://github.com/user-attachments/assets/ee663b95-07e3-42d9-b7bf-70ca91a20a28)


3. **Enumerate Public Snapshots with AWS CLI**
   - Using the obtained **account ID**, list all public snapshots owned by the organization:
     ```bash
     aws ec2 describe-snapshots --filters "Name=owner-id,Values=123456789012" --owner-ids amazon
     ```

     ![image](https://github.com/user-attachments/assets/6f4f9503-1c83-495e-b5a7-6b5f883f2c64)


## Notes
- Replace all placeholders (`123456789012`, `LeakyBucket`, `example-bucket-name`) with your findings.
- This technique highlights the risks associated with public misconfiguration in AWS, emphasizing the need for secure access settings.

---

**End of Document**
