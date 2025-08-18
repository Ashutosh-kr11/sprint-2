<img src="https://github.com/user-attachments/assets/e42b1957-ba11-4325-97f4-169cddf662e3" alt="Image" width="100%" />

# Python Dependency Scanning with `pip-audit` POC: Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-16     | 1.0         | 2025-07-16          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [Dependency Scanning for Python CI Checks Documentation](#dependency-scanning-for-python-ci-checks-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)
  * [Pre-requisites Setup](#pre-requisites-setup)
  * [Project Setup and Scanning](#project-setup-and-scanning)
  * [Advanced Scanning Options](#advanced-scanning-options)
* [Troubleshooting Common Issues](#troubleshooting-common-issues)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This guide explains how to perform **dependency vulnerability scanning** in Python projects using `pip-audit`. It helps identify known vulnerabilities in third-party dependencies so developers can proactively secure their applications.

This POC covers analysis of two OT-MICROSERVICES Python applications:
- **attendance-api**: Flask-based REST API for attendance management
- **notification-worker**: Python service for email notifications


---

## **Pre-requisites**

Before starting, ensure you have:

| Dependency       | Minimum Version |
| ---------------- | --------------- |
| Python           | 3.8+            |
| pip              | 20.0+           |
| pip-audit        | Latest          |
| Git              | Latest          |

---

## **Dependency Scanning for Python CI Checks Documentation**

Follow this link for "Dependency Scanning for Python CI checks" Doc:

> [Dependency Scanning for Python CI checks Doc](https://github.com/Ashutosh-kr11/sprint-2/blob/main/React%20CI%20Checks/Static%20code%20analysis%20Doc.md)

---

## **Step-by-Step Instructions**

### Pre-requisites Setup

  #### 1. **System Preparation**
  Update and upgrade your Ubuntu system packages:
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
  <img width="1918" height="916" alt="Image" src="https://github.com/user-attachments/assets/8b1a6681-ed21-4113-9453-958e24d09d21" />

  
  #### 2. **Install Required System Packages**
  Install Python and essential tools:
  ```bash
  sudo apt install python3 python3-pip python3.12-venv -y
  ```
  <img width="1918" height="332" alt="Image" src="https://github.com/user-attachments/assets/57ad11b6-e087-4993-b4b0-6b8d588e0e4a" />

  
  #### 3. **Install pip-audit Tool**
  Install pip-audit globally for dependency scanning:
  ```bash
  sudo pip install pip-audit --break-system-packages
  ```

  <img width="1918" height="898" alt="Image" src="https://github.com/user-attachments/assets/c1b802aa-14df-47e1-be10-4412864afdfc" />
  
  > **Note:** The `--break-system-packages` flag is used for system-wide installation on newer Python versions.

### Project Setup and Scanning

  #### 4. **Clone Target Repositories**
  Clone the Python projects you want to scan:
  ```bash
  # Example repositories
  git clone https://github.com/OT-MICROSERVICES/notification-worker.git
  git clone https://github.com/OT-MICROSERVICES/attendance-api.git
  ```

  <img width="1915" height="132" alt="Image" src="https://github.com/user-attachments/assets/9fae8312-34c9-4749-9624-ca85c64184fd" />
  
  #### 5. **Scan First Project (Attendance API)**
  
  **a) Navigate to project directory:**
  ```bash
  cd attendance-api
  ```
  
  **b) Create and activate virtual environment:**
  ```bash
  python3 -m venv venv_attendance
  source venv_attendance/bin/activate
  ```
  
  **c) Generate or verify requirements.txt:**
  ```bash
  # If requirements.txt doesn't exist, install dependencies and generate it
  pip install -r requirements.txt  # Install existing dependencies
  pip freeze > requirements.txt    # Generate/update requirements.txt
  ```
  <img width="1918" height="355" alt="Image" src="https://github.com/user-attachments/assets/ea8ceb9b-b0ee-4526-a0ef-0303c26d64b0" />
  
  **d) Run dependency audit:**
  ```bash
   # Audit using exported requirements.txt
   pip-audit -r requirements.txt
   
   # Alternative: Audit Poetry environment directly
   poetry run pip-audit
  ```
  <img width="1918" height="902" alt="Image" src="https://github.com/user-attachments/assets/95ea81bf-d9c5-4077-9092-017232a8988e" />

  **e) Deactivate virtual environment:**
  ```bash
  deactivate
  ```

   > if Poetry is used for Dependency management
      Generate requirements.txt from Poetry for pip-audit:   
   ```bash
       # Export Poetry dependencies to requirements.txt
       poetry export -f requirements.txt --output requirements.txt --without-hashes
          
       # Alternative: Export with development dependencies
       poetry export -f requirements.txt --output requirements.txt --without-hashes --with dev
   ```
  
  #### 6. **Scan Second Project (Notification Worker)**
  
  **a) Navigate to project directory:**
  ```bash
  cd ../notification-worker
  ```
  
  **b) Create and activate virtual environment:**
  ```bash
  python3 -m venv venv_notification
  source venv_notification/bin/activate
  ```
  
  **c) Verify requirements.txt and audit:**
  ```bash
  # Check if requirements.txt exists
  ls requirements.txt
  
  # Run audit
  pip-audit -r requirements.txt
  ```
  <img width="1918" height="268" alt="Image" src="https://github.com/user-attachments/assets/0bc37118-8034-45bd-86d6-f5e3758bb204" />
  
  **d) Deactivate virtual environment:**
  ```bash
  deactivate
  ```

### Advanced Scanning Options

### Results Analysis and Remediation

#### 7. **Analyze Scan Results**
- **Review Vulnerabilities**: Examine each identified vulnerability
- **Check Severity Levels**: Prioritize Critical > High > Medium > Low
- **Identify Affected Packages**: Note which dependencies need updates


#### 8. **Verify Fixes**
```bash
# Re-run audit to confirm vulnerabilities are resolved
pip-audit -r requirements.txt

# Check for any remaining issues
pip-audit --desc
```

---

## Troubleshooting Common Issues

| Problem                             | Solution                                                 |
| ----------------------------------- | -------------------------------------------------------- |
| `pip-audit` not found               | `sudo pip install pip-audit --break-system-packages`     |
| Permission denied                   | Use `sudo` or `pip install --user pip-audit`             |
| Virtual environment won’t activate  | `source venv_name/bin/activate` (check the correct path) |
| `requirements.txt` missing          | Generate with: `pip freeze > requirements.txt`           |
| No vulnerabilities found but errors | Check if `requirements.txt` has valid package names      |
| Command not found: `python3`        | Install Python: `sudo apt install python3`               |

---

## **Conclusion**


By integrating `pip-audit` into Python projects, teams can:

* Detect vulnerabilities early
* Strengthen security posture
* Maintain compliance and trust

Regular scans and timely updates are crucial for ongoing security.

---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**


| Source                                                                                  | Description                          |
| --------------------------------------------------------------------------------------- | ------------------------------------ |
| [pip-audit Documentation](https://pypi.org/project/pip-audit/)                          | Official usage guide for pip-audit   |
| [Python Advisory Database](https://github.com/pypa/advisory-database)                   | Known Python package vulnerabilities |
| [Attendance API Repo](https://github.com/OT-MICROSERVICES/attendance-api.git)           | Example project for scanning         |
| [Notification Worker Repo](https://github.com/OT-MICROSERVICES/notification-worker.git) | Example project for scanning         |
