# Deployment Steps

This document outlines the step-by-step process to deploy the Java web application using the AWS infrastructure.

---

## 1. Build the Application Artifact

- On your local machine, navigate to the project root  
- Run Maven build:

      mvn clean package

- Confirm the `.war` file is generated in the `target/` directory

---

## 2. Upload Artifact to S3

- Use AWS CLI to upload the artifact to your S3 bucket:

      aws s3 cp target/your-app-name.war s3://your-bucket-name/path/to/artifacts/

- Ensure the artifact path is accessible by the EC2 instances’ IAM role

---

## 3. Launch or Update Auto Scaling Group

- If first deployment, launch the ASG with the configured Launch Template  
- For updates, update the Launch Template version to include new user data (if changed) or manually trigger instance refresh in ASG

---

## 4. EC2 Instances Initialization

- New Tomcat instances will run user data scripts on launch  
- The script will pull the latest artifact from S3 and deploy it to Tomcat  
- Health checks by ALB ensure only healthy instances serve traffic

---

## 5. Verify Deployment

- Access the application via the custom domain over HTTPS  
- Confirm the app is responding and stable  
- Use AWS Console or CLI to check instance health and logs if needed

---

## 6. Scaling and Maintenance

- ASG automatically replaces unhealthy instances  
- Adjust ASG desired capacity as needed based on load  
- Update Launch Template versions for application or configuration changes

---

## Notes

- SSH access to instances should be limited and controlled  
- Monitor application and AWS resources for performance and errors  
- Keep user data scripts and IAM roles updated for security

---

## Summary

This flow ensures smooth, repeatable deployments with minimal downtime by leveraging AWS Auto Scaling and S3 artifact storage.
