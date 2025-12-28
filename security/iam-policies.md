# IAM Policies

This document details the IAM policies used to secure AWS resources for this project.

---

## 1. EC2 Instance Role Policy

Allows Tomcat EC2 instances to pull application artifacts from S3.

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

---

## 2. CLI User Policy

Allows uploading artifacts and minimal management permissions.

    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
            "s3:PutObject",
            "s3:GetObject",
            "s3:ListBucket"
          ],
          "Resource": [
            "arn:aws:s3:::your-bucket-name",
            "arn:aws:s3:::your-bucket-name/*"
          ]
        },
        {
          "Effect": "Allow",
          "Action": [
            "autoscaling:Describe*",
            "autoscaling:UpdateAutoScalingGroup",
            "ec2:DescribeInstances"
          ],
          "Resource": "*"
        }
      ]
    }

---

## Notes

- Replace `"your-bucket-name"` and paths with your actual S3 bucket and folder  
- Follow the principle of least privilege  
- Rotate credentials regularly  
- Adjust policies as needed for additional AWS operations  

---

## Summary

These policies ensure secure, limited access for deployment automation and instance operations.
