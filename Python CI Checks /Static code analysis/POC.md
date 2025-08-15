<img src="https://github.com/user-attachments/assets/df5d77e2-2cae-4a07-a98d-533f4c757bba" alt="Image" width="100%" />

# SonarQube for Python Static Code Analysis POC: Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-14     | 1.0         | 2025-07-14          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [Static Code Analysis for Python CI Checks Documentation](#static-code-analysis-for-python-ci-checks-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)
  * [Step 1: Setup SonarQube Server](#step-1-setup-sonarqube-server)
  * [Step 2: Configure SonarQube Project](#step-2-configure-sonarqube-project)
  * [Step 3: Setup Python Projects for Analysis](#step-3-setup-python-projects-for-analysis)
  * [Step 4: Run Your First Analysis](#step-4-run-your-first-analysis)
  * [Step 5: View Results](#step-5-view-results)
* [Troubleshooting Common Issues](#troubleshooting-common-issues)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This Proof of Concept (POC) demonstrates how SonarQube can be integrated into Python applications for comprehensive static code analysis. SonarQube helps identify potential security vulnerabilities, code quality issues, PEP 8 violations, complexity problems, and maintainability issues in your Python codebase—which you can fix almost immediately. 

This POC covers analysis of two OT-MICROSERVICES Python applications:
- **attendance-api**: Flask-based REST API for attendance management
- **notification-worker**: Python service for email notifications

Follow this link for "Sonarqube" Doc:

> [Sonarqube Doc](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-59-deepak/Sonarqube/README.md)

---

## **Pre-requisites**

Before starting, ensure you have:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| **Python** | 3.8 or higher | `python3 --version` |
| **Java JDK** | 17 or higher | `java -version` |
| **Docker** | Latest | `docker --version` |
| **Git** | Latest | `git --version` |
| **SonarScanner** | 5.0+ | `sonar-scanner --version` |

---

## **Static Code Analysis for Python CI Checks Documentation**

Follow this link for "Static code analysis for Python CI checks" Doc:

> [Static code analysis for Python CI checks Doc](https://github.com/Ashutosh-kr11/sprint-2/blob/main/React%20CI%20Checks/Static%20code%20analysis%20Doc.md)

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

**Create attendance-api Project:**
1. Click **"Create Project"**
2. Choose **"Manually"**

   <img width="1918" height="985" alt="Image" src="https://github.com/user-attachments/assets/eb323f6f-84f7-46c9-9b19-4579e98df7ba" />
   
3. Fill in:
   - **Project key:** `ot-attendance-api`
   - **Display name:** `OT Attendance API`
4. Click **"Set Up"**
   <img width="1918" height="985" alt="Image" src="https://github.com/user-attachments/assets/b6339b50-99ed-467d-aa95-edc097bd41cf" />
  
5. Choose **"Locally"**
   <img width="1918" height="987" alt="Image" src="https://github.com/user-attachments/assets/d36f9e6f-554d-45ed-895b-a845ea5e305f" />
   
6. Generate a token and save it
   <img width="1918" height="986" alt="Image" src="https://github.com/user-attachments/assets/fffe7ac5-8251-405c-a5db-a8219f1bf139" />
   <img width="1918" height="990" alt="Image" src="https://github.com/user-attachments/assets/1c73734d-d8e3-430e-b38d-8108fc660b30" />

**Create notification-worker Project:**
1. Repeat the same process with:
   - **Project key:** `ot-notification-worker`
   - **Display name:** `OT Notification Worker`
   <img width="1918" height="987" alt="Image" src="https://github.com/user-attachments/assets/5f97c680-546d-4612-bbb7-242081dc1e25" />
     
2. Generate a separate token for this project
    <img width="1918" height="987" alt="Image" src="https://github.com/user-attachments/assets/4f3f0a10-8202-49ef-9dbb-5a8ec2b18615" />

---

###  Step 3: Setup Python Projects for Analysis

#### Install SonarQube Scanner for Python

```bash
# Install globally
npm install -g sonarqube-scanner

# OR install as dev dependency (recommended)
npm install --save-dev sonarqube-scanner
```
  <img width="1918" height="541" alt="Image" src="https://github.com/user-attachments/assets/de2a63ba-dd3e-4a45-b91b-b9fc25b54fe9" />

#### 1. Clone the Target Applications

```bash
# Clone attendance-api
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
cd attendance-api

# Clone notification-worker
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
cd notification-worker
```
<img width="1918" height="130" alt="Image" src="https://github.com/user-attachments/assets/8d7e3d2d-6c97-417e-829c-4fa355de4fd1" />

#### 2. Install Static Analysis Tools

```bash
# Install required tools for both projects
pip install pylint bandit black flake8
```

<img width="1918" height="935" alt="Image" src="https://github.com/user-attachments/assets/a2b439b1-cacf-4575-942d-d6e1f5f79965" />

<img width="1918" height="876" alt="Image" src="https://github.com/user-attachments/assets/a032ab85-7a28-485b-887a-417629120375" />

#### 3. Setup attendance-api Project

```bash
cd attendance-api

# Create sonar-project.properties
cat > sonar-project.properties << EOF
# Project identification
sonar.projectKey=ot-attendance-api
sonar.projectName=OT Attendance API
sonar.projectVersion=1.0

# Source code paths
sonar.sources=app,router,client,models,utils
sonar.tests=tests

# File patterns
sonar.inclusions=**/*.py
sonar.exclusions=tests/**,**/__pycache__/**,**/venv/**,**/.pytest_cache/**,migrations/**

# Python-specific settings
sonar.python.pylint.reportPaths=pylint-report.txt
sonar.python.bandit.reportPaths=bandit-report.json
sonar.python.flake8.reportPath=flake8-report.txt

# Additional settings
sonar.sourceEncoding=UTF-8
sonar.python.version=3.8,3.9,3.10,3.11

# SonarQube server
sonar.host.url=http://localhost:9000
EOF
```


#### 4. Setup notification-worker Project

```bash
cd notification-worker

# Create sonar-project.properties
cat > sonar-project.properties << EOF
# Project identification
sonar.projectKey=ot-notification-worker
sonar.projectName=OT Notification Worker
sonar.projectVersion=1.0

# Source code paths
sonar.sources=.

# File patterns
sonar.inclusions=**/*.py
sonar.exclusions=**/__pycache__/**,**/venv/**,**/.pytest_cache/**,**/.git/**

# Python-specific settings
sonar.python.pylint.reportPaths=pylint-report.txt
sonar.python.bandit.reportPaths=bandit-report.json
sonar.python.flake8.reportPath=flake8-report.txt

# Additional settings
sonar.sourceEncoding=UTF-8
sonar.python.version=3.8,3.9,3.10,3.11

# SonarQube server
sonar.host.url=http://localhost:9000
EOF
```

#### 5. Create Analysis Scripts

**For attendance-api:**
```bash
cat > analyze.sh << EOF
#!/bin/bash
echo "=== Running Static Analysis for Attendance API ==="

# Check if SONAR_TOKEN is set
if [ -z "\$SONAR_TOKEN" ]; then
    echo "Error: SONAR_TOKEN environment variable not set"
    echo "Please run: export SONAR_TOKEN=your-token-here"
    exit 1
fi

# Run Pylint
echo "Running Pylint..."
pylint app router client models utils --output-format=parseable --reports=no > pylint-report.txt || true

# Run Bandit security scan
echo "Running Bandit security scan..."
bandit -r app router client models utils -f json -o bandit-report.json || true

# Run Flake8
echo "Running Flake8 style check..."
flake8 app router client models utils --output-file=flake8-report.txt --format='%(path)s:%(row)d:%(col)d: %(code)s %(text)s' || true

# Run Black check (dry run)
echo "Running Black formatting check..."
black --check --diff app router client models utils || true

# Run SonarQube analysis
echo "Running SonarQube analysis..."
sonar-scanner -Dsonar.login=\$SONAR_TOKEN

echo "Analysis complete! Check results at http://localhost:9000"
EOF

chmod +x analyze.sh
```

**For notification-worker:**
```bash
cat > analyze.sh << EOF
#!/bin/bash
echo "=== Running Static Analysis for Notification Worker ==="

# Check if SONAR_TOKEN is set
if [ -z "\$SONAR_TOKEN" ]; then
    echo "Error: SONAR_TOKEN environment variable not set"
    echo "Please run: export SONAR_TOKEN=your-token-here"
    exit 1
fi

# Run Pylint
echo "Running Pylint..."
pylint *.py --output-format=parseable --reports=no > pylint-report.txt || true

# Run Bandit security scan
echo "Running Bandit security scan..."
bandit -r . -f json -o bandit-report.json --exclude tests || true

# Run Flake8
echo "Running Flake8 style check..."
flake8 . --output-file=flake8-report.txt --format='%(path)s:%(row)d:%(col)d: %(code)s %(text)s' || true

# Run Black check (dry run)
echo "Running Black formatting check..."
black --check --diff . || true

# Run SonarQube analysis with login parameter (compatible with older versions)
echo "Running SonarQube analysis..."
sonar-scanner -Dsonar.login=\$SONAR_TOKEN

echo "Analysis complete! Check results at http://localhost:9000"
EOF

chmod +x analyze.sh
```

---

###  Step 4: Run Your First Analysis

#### Method 1: Using Analysis Scripts (Recommended)

**For attendance-api:**
```bash
cd attendance-api

export SONAR_TOKEN=your-token

./analyze.sh
```
  <img width="1918" height="937" alt="Image" src="https://github.com/user-attachments/assets/c83d9c5a-6f1a-4a4d-b270-504c34b87ef8" />
  
**For notification-worker:**
```bash
cd notification-worker

export SONAR_TOKEN=your-token

./analyze.sh
```
<img width="1918" height="932" alt="Image" src="https://github.com/user-attachments/assets/166c0c2a-5745-47ef-bbc0-e01ca5f8e643" />

#### Method 2: Manual Step-by-Step Analysis

**For attendance-api:**
```bash
cd attendance-api

# Generate reports
pylint app router client models utils --output-format=parseable --reports=no > pylint-report.txt || true
bandit -r app router client models utils -f json -o bandit-report.json || true
flake8 app router client models utils --output-file=flake8-report.txt || true

# Run SonarQube analysis
sonar-scanner \
  -Dsonar.projectKey=ot-attendance-api \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-attendance-api-token
```

**For notification-worker:**
```bash
cd notification-worker

# Generate reports
pylint *.py --output-format=parseable --reports=no > pylint-report.txt || true
bandit -r . -f json -o bandit-report.json --exclude tests || true
flake8 . --output-file=flake8-report.txt || true

# Run SonarQube analysis
sonar-scanner \
  -Dsonar.projectKey=ot-notification-worker \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-notification-worker-token
```

---

### Step 5: View Results

#### 1. Open SonarQube Dashboard

**For attendance-api:**
- Go to: http://localhost:9000/dashboard?id=ot-attendance-api

**For notification-worker:**
- Go to: http://localhost:9000/dashboard?id=ot-notification-worker

#### 2. What You'll See

**Quality Overview Dashboard:**
  <img width="985" height="930" alt="Image" src="https://github.com/user-attachments/assets/b8d2e2f2-cc46-4b63-834c-129d08287bc0" />
  <img width="980" height="935" alt="Image" src="https://github.com/user-attachments/assets/7b3d6cbd-a276-4ef8-b7fa-ffd39754e2dd" />
  
**Code Issues by Priority:**
  <img width="987" height="935" alt="Image" src="https://github.com/user-attachments/assets/11d19681-313a-4712-b60d-f6eb172be067" />
  <img width="981" height="933" alt="Image" src="https://github.com/user-attachments/assets/105fb8ee-28c3-41ca-99a7-099edba13a45" />

---

## Troubleshooting Common Issues

| Issue | Solution |
|-------|----------|
| **"Connection refused" to SonarQube** | 1. Check if SonarQube is running: `docker ps \| grep sonarqube`<br>2. If not running: `docker start sonarqube` |
| **"Authentication failed"** | Generate a new token in SonarQube UI: *My Account > Security > Generate Tokens* |
| **"sonar-scanner: command not found"** | Add to PATH: `export PATH="/opt/sonar-scanner/bin:$PATH"` |
| **"No sources found"** | Check `sonar.sources` path in `sonar-project.properties` matches your project structure |
| **"Python plugin not found"** | SonarQube Community Edition includes Python plugin by default |
| **"Import errors in Pylint"** | Set PYTHONPATH: `export PYTHONPATH="${PYTHONPATH}:$(pwd)"` |
| **"Permission denied"** | Fix file permissions: `chmod +x analyze.sh` and check Docker permissions |
| **"OutOfMemoryError"** | Increase scanner memory: `export SONAR_SCANNER_OPTS="-Xmx2048m"` |

---

## **Conclusion**

The setup ensures early detection of code quality issues, security vulnerabilities, and maintainability problems across both Python applications. SonarQube's centralized dashboard provides a unified view of code health, enabling teams to maintain high-quality standards while reducing technical debt.

This static analysis approach helps prevent issues from reaching production and establishes a foundation for continuous code quality improvement in your Python microservices architecture.


---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**

| **Links**                                                    | **Overview**                                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| [SonarQube Python Analysis](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/languages/python/) | Official documentation for Python analysis in SonarQube |
| [SonarPython Rules](https://rules.sonarsource.com/python/) | Comprehensive list of Python-specific rules supported by SonarQube |
| [OT-MICROSERVICES attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) | Source repository for attendance API application |
| [OT-MICROSERVICES notification-worker](https://github.com/OT-MICROSERVICES/notification-worker) | Source repository for notification worker service |
