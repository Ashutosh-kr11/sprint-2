<img src="https://github.com/user-attachments/assets/df5d77e2-2cae-4a07-a98d-533f4c757bba" alt="Image" width="100%" />

# SonarQube for Static code analysis POC: Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-13     | 1.0         | 2025-07-13          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [Static Code Analysis for Java CI Checks Documentation](#static-code-analysis-for-java-ci-checks-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)
  * [Step 1: Setup SonarQube Server](#step-1-setup-sonarqube-server)
  * [Step 2: Configure SonarQube Project](#step-2-configure-sonarqube-project)
  * [Step 3: Run Your First Analysis](#step-3-run-your-first-analysis)
  * [Step 4: View Results](#step-4-view-results)
* [Troubleshooting Common Issues](#troubleshooting-common-issues)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This Proof of Concept (POC) demonstrates how Sonarqube can be integrated into your project for static code analysis. Sonarqube helps identify potential security issues, bugs, and performance issues in your code—which you can fix almost immediately. The following steps will guide you through setting up Sonarqube, scanning your codebase, and handling any detected issues.

Follow this link for "Sonarqube" Doc:

> [Sonarqube Doc](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-59-deepak/Sonarqube/README.md)

---

## **Pre-requisites**

Before starting, ensure you have:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| **Java JDK** | 11 or higher | `java -version` |
| **Maven** | 3.6+ | `mvn -version` |
| **Docker** | Latest | `docker --version` |

---

## **Static code analysis for java CI checks Documentation**

Follow this link for "Static code analysis for java CI checks" Doc:

> [Static code analysis for java CI checks Doc](https://github.com/Ashutosh-kr11/sprint-2/blob/main/Java%20CI%20checks/Static%20code%20analysis%20/Doc.md)

---

## **Step-by-Step Instructions**

###  Step 1: Setup SonarQube Server 

#### Option A: Direct Installation

  - **Download SonarQube Community Edition**
    ```bash
    wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.1.69595.zip
    ```
    <img width="1917" height="411" alt="Image" src="https://github.com/user-attachments/assets/c404f86c-c425-4823-b820-ecbdb769b6a3" />
  
  - **Extract**
    ```bash
    unzip sonarqube-9.9.1.69595.zip
    ```
    <img width="1918" height="900" alt="Image" src="https://github.com/user-attachments/assets/efc963e7-5306-4893-bbec-ae71d28f64d7" />
  
  - **Start**
    ```bash
    cd sonarqube-9.9.1.69595/bin/linux-x86-64/
    ./sonar.sh start
    ```
    <img width="1918" height="292" alt="Image" src="https://github.com/user-attachments/assets/330af4d1-0b43-42ae-9c1c-e60ea7ab3d84" />
  
#### Option B: Using Docker (Alternative)

```bash
# 1. Create a directory for SonarQube data
mkdir sonarqube-data

# 2. Run SonarQube in Docker
docker run -d \
  --name sonarqube \
  -p 9000:9000 \
  -v $(pwd)/sonarqube-data:/opt/sonarqube/data \
  sonarqube:community
```

####  Verify Installation
1. Open browser: http://localhost:9000
2. Login with default credentials:
   - **Username:** admin
   - **Password:** admin

   <img width="1918" height="987" alt="Image" src="https://github.com/user-attachments/assets/631f371c-a324-4913-8550-5fc9ab263994" />

3. You'll be prompted to change password

   <img width="1918" height="990" alt="Image" src="https://github.com/user-attachments/assets/60d666ea-401d-4448-938b-d4e062141fc7" />

---

###  Step 2: Configure SonarQube Project

#### 1. Create Project in SonarQube Web Interface
1. Click **"Create Project"**
2. Choose **"Manually"**

   <img width="1918" height="985" alt="Image" src="https://github.com/user-attachments/assets/eb323f6f-84f7-46c9-9b19-4579e98df7ba" />
   
3. Fill in:
   - **Project key:** `Salary-API`
   - **Display name:** `Salary-API`
4. Click **"Set Up"**
   <img width="1918" height="990" alt="Image" src="https://github.com/user-attachments/assets/0e9f236e-e99a-4a49-a9bf-06e5fe34028e" />
  
5. Choose **"Locally"**
   <img width="1918" height="986" alt="Image" src="https://github.com/user-attachments/assets/39ae5954-5870-47b8-8ba4-cd5d79c2b59b" />
   
7. Generate a token and save it
   <img width="1918" height="983" alt="Image" src="https://github.com/user-attachments/assets/6a68bc86-89df-4716-a038-6be1e8ad4bb6" />
   <img width="1918" height="988" alt="Image" src="https://github.com/user-attachments/assets/4eef95d4-1ac1-41e3-a4c8-51e850859bfb" />

---

###  Step 3: Run Your First Analysis

### Method 1: Using Maven (Recommended)
```bash
mvn clean install -DskipTests
mvn sonar:sonar \
  -DskipTests \
  -Dsonar.projectKey=Salary-API \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-generated-token
```
  
  <img width="1918" height="847" alt="Image" src="https://github.com/user-attachments/assets/f0883af8-dff3-431f-acaf-3bba40852d98" />
  
### Method 2: Using SonarQube Scanner (Alternative)
```bash
# If you have SonarQube Scanner installed
sonar-scanner
```

---

### Step 4: View Results

#### 1. Open SonarQube Dashboard
- Go to: http://localhost:9000/dashboard?id=Salary-API
- What You'll See
- **Quality Overview Dashboard**
  <img width="1756" height="986" alt="Image" src="https://github.com/user-attachments/assets/ecec2f67-f630-4630-9f73-555134fccc4b" />
- **Code Issues Priority**
  <img width="1918" height="988" alt="Image" src="https://github.com/user-attachments/assets/fdfebcb1-4390-4f17-ba31-e29e67ae7954" />

---

## Troubleshooting Common Issues

| Issue                                 | Solution                                                                                                       |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **"Connection refused" to SonarQube** | 1. Check if SonarQube is running: `docker ps \| grep sonarqube`<br>2. If not running: `docker start sonarqube` |
| **"Authentication failed"**           | Generate a new token in SonarQube UI: *My Account > Security > Generate Tokens*                                |
| **"No coverage information"**         | Ensure JaCoCo plugin is configured and tests run: `mvn clean test jacoco:report sonar:sonar`                   |
| **"OutOfMemoryError"**                | Increase Maven memory: `export MAVEN_OPTS="-Xmx2048m -XX:MaxPermSize=512m"`                                    |

---

## **Conclusion**

Integrating SonarQube into your development workflow ensures that your project maintains high code quality by automatically scanning for bugs, vulnerabilities, code smells, and maintainability issues. By following this POC, you have implemented an efficient process to identify and address potential problems early in the development cycle, improving software reliability and reducing technical debt. SonarQube provides a dependable and continuous way to enforce coding standards and deliver cleaner, more maintainable code with minimal overhead.

---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**

| **Links**                                                    | **Overview**                                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| [SonarQube Docs](https://docs.sonarsource.com/)              | Official documentation for installing, configuring, and using SonarQube.      |
| [SonarQube Rules](https://rules.sonarsource.com/)            | Comprehensive list of code quality and security rules supported by SonarQube. |
| [SonarQube GitHub](https://github.com/SonarSource/sonarqube) | SonarQube’s open-source repository and development resources.                 |

