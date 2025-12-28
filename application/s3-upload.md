# S3 Artifact Upload Instructions

This document explains how to upload the built Java application artifact to an Amazon S3 bucket for deployment.

---

## Prerequisites

- AWS CLI installed and configured with appropriate IAM user credentials  
- S3 bucket created to store application artifacts  
- Artifact `.war` file built locally (see `maven-build.md`)

---

## Upload Command

Run the following command from your local machine, replacing placeholders with your values:

    aws s3 cp target/your-app-name.war s3://your-bucket-name/path/to/artifacts/

Example:

    aws s3 cp target/myapp.war s3://myapp-artifacts/production/

---

## IAM Permissions

The IAM user used for uploading must have permissions similar to:

- `s3:PutObject` on the target bucket  
- `s3:GetObject` (optional, for verification)

Use the principle of least privilege when assigning permissions.

---

## Notes

- Keep artifact naming consistent to avoid confusion  
- Consider versioning artifacts in S3 (e.g., using timestamps or git commit hashes)  
- Ensure the EC2 instances’ IAM role has permission to `GetObject` from this bucket

---

## Next Step

After uploading, the artifact will be pulled by the Tomcat instances during launch as described in user data scripts.
