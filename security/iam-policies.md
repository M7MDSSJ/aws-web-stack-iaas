# IAM Policies

This document details the IAM policies used to secure AWS resources for this project.

---

## 1. EC2 Instance Role Policy

Allows Tomcat EC2 instances to pull application artifacts from S3.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name/path/to/artifacts/*"
      ]
    }
  ]
}
