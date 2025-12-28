# Maven Build Instructions

This document describes how to build the Java application artifact locally using Maven, to be deployed later on AWS Tomcat instances.

---

## Prerequisites

- Java Development Kit (JDK) 17 installed  
- Apache Maven installed  
- Source code checked out locally  

---

## Build Command

Navigate to the root of your Java project, then run:

    mvn clean package

This command will:

- Clean previous build artifacts  
- Compile source code  
- Run tests (if any)  
- Package the application into a `.war` file inside the `target/` directory

---

## Output Artifact

- Location: `target/your-app-name.war`  
- This `.war` file is the deployable artifact uploaded to Amazon S3  

---

## Notes

- Ensure the `pom.xml` specifies Java 17 compatibility  
- You can skip tests with:

      mvn clean package -DskipTests

- The artifact name should be consistent with deployment scripts  

---

## Next Step

After building, upload the artifact to S3 as described in `application/s3-upload.md`.
