# IAM Roles Configuration

This document explains the IAM roles used in the project, focusing on security and least privilege principles.

---

## EC2 Instance Role (Tomcat)

- Assigned to all Tomcat EC2 instances via the Launch Template
- Permissions:
  - Read-only access to the S3 bucket containing application artifacts
  - Ability to pull artifacts during instance initialization
- No unnecessary permissions granted
- Enables secure, credential-free access to S3

---

## IAM User

- Created for AWS CLI operations (artifact upload, management)
- Assigned policies with limited permissions scoped to required resources
- Access keys managed securely and rotated regularly

---

## Policies

- Follow the principle of least privilege
- Custom policies restrict access to:
  - Specific S3 buckets
  - EC2 and Auto Scaling management (if applicable)
- Use AWS Managed Policies where possible for simplicity

---

## Security Considerations

- No hardcoded credentials in user data or code
- Role permissions reviewed and minimized
- IAM roles linked only to necessary AWS resources

---

## Notes

- IAM roles and policies are versioned and documented
- Roles allow easy future extension (e.g., monitoring, logging)
