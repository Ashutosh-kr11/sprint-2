<img src="https://github.com/user-attachments/assets/df5d77e2-2cae-4a07-a98d-533f4c757bba" alt="Image" width="100%" />

# SonarQube for SCA in React CI Checks POC: Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-14     | 1.0         | 2025-07-14          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [Static Code Analysis for React CI Checks Documentation](#static-code-analysis-for-react-ci-checks-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)
  * [Step 1: Setup SonarQube Server](#step-1-setup-sonarqube-server)
  * [Step 2: Configure SonarQube Project](#step-2-configure-sonarqube-project)
  * [Step 3: Setup React Project for Analysis](#step-3-setup-react-project-for-analysis)
  * [Step 4: Run Your First Analysis](#step-4-run-your-first-analysis)
  * [Step 5: View Results](#step-5-view-results)
* [Troubleshooting Common Issues](#troubleshooting-common-issues)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This Proof of Concept (POC) demonstrates how SonarQube can be integrated into React applications for comprehensive static code analysis. SonarQube helps identify potential security vulnerabilities, code quality issues, performance problems, accessibility violations, and TypeScript/JavaScript errors in your React codebase—which you can fix almost immediately. The following steps will guide you through setting up SonarQube, configuring React-specific analysis, and handling any detected issues.

This POC covers:
- JavaScript code quality analysis
- React-specific patterns and best practices
- Security vulnerability detection (XSS, unsafe patterns)
- Bundle size and performance optimization
- Accessibility (a11y) compliance checking
- Integration with modern React build tools

Follow this link for "Sonarqube" Doc:

> [Sonarqube Doc](https://github.com/Snaatak-Cloudops-Crew/documentation/blob/scrum-59-deepak/Sonarqube/README.md)

---

## **Pre-requisites**

Before starting, ensure you have:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| **Node.js** | 16 or higher | `node --version` |
| **npm** | 8+ | `npm --version` |
| **React Application** | 16.8+ | Check `package.json` |
| **Docker** | Latest | `docker --version` |
| **Git** | Latest | `git --version` |

---

## **Static Code Analysis for React CI Checks Documentation**

Follow this link for "Static code analysis for java CI checks" Doc:

> [Static code analysis for React.js CI checks Doc](https://github.com/Ashutosh-kr11/sprint-2/blob/main/React%20CI%20Checks/Static%20code%20analysis%20Doc.md)

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
   - **Project key:** `ot-go-webapp`
   - **Display name:** `OT Go WebApp - React Frontend`
4. Click **"Set Up"**
   <img width="1918" height="987" alt="Image" src="https://github.com/user-attachments/assets/5bf28be7-6d38-4ac4-b877-e291d9dc395a" />
  
5. Choose **"Locally"**
   <img width="1918" height="988" alt="Image" src="https://github.com/user-attachments/assets/b5c1c39a-c6d6-4186-b00f-d51cf0ba11bb" />
   
7. Generate a token and save it
   <img width="1918" height="993" alt="Image" src="https://github.com/user-attachments/assets/e2dd294c-0f83-44b0-81a0-47856e80844c" />
   <img width="1918" height="986" alt="Image" src="https://github.com/user-attachments/assets/70472efc-9895-4fc8-ae4b-9e48df110b8c" />
   <img width="1280" height="932" alt="Image" src="https://github.com/user-attachments/assets/2af10f01-1102-4feb-92ee-e511799360a7" />
   
---

###  Step 3: Setup React Project for Analysis

#### 1. Install SonarQube Scanner for JavaScript

```bash
# Install globally
npm install -g sonarqube-scanner

# OR install as dev dependency (recommended)
npm install --save-dev sonarqube-scanner
```
  <img width="1918" height="541" alt="Image" src="https://github.com/user-attachments/assets/de2a63ba-dd3e-4a45-b91b-b9fc25b54fe9" />

#### 2. Create SonarQube Configuration File

Create `sonar-project.properties` in your React project root:

```properties
# Project identification
sonar.projectKey=ot-go-webapp
sonar.projectName=OT Go WebApp - React Frontend
sonar.projectVersion=0.0.0

# Source code location
sonar.sources=src
sonar.tests=src
sonar.test.inclusions=**/*.test.js,**/*.test.jsx,**/*.spec.js,**/*.spec.jsx

# Exclude files
sonar.exclusions=**/node_modules/**,**/*.test.js,**/*.test.jsx,**/coverage/**,**/build/**,**/dist/**,**/*.spec.js,**/*.spec.jsx,**/static/**

# Language settings
sonar.javascript.lcov.reportPaths=coverage/lcov.info

# SonarQube server
sonar.host.url=http://localhost:9000
sonar.login=your-generated-token

```

<img width="1918" height="120" alt="Image" src="https://github.com/user-attachments/assets/5aca2ed1-8387-4377-b0f9-e300cba4fcc9" />

#### 3. Configure ESLint for React (if not already configured)

Create or update `.eslintrc.json`:

```json
{
  "extends": [
    "react-app"
  ],
  "plugins": [
    "react",
    "react-hooks",
    "jsx-a11y"
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/anchor-is-valid": "error",
    "no-unused-vars": "warn",
    "no-console": "warn"
  },
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  }
}
```
<img width="1918" height="140" alt="Image" src="https://github.com/user-attachments/assets/7eeb4766-c1a5-4020-a5f3-b62d4ab9dc2a" />

#### 4. Add npm Scripts

Update your `package.json`:

```json
{
  "scripts": {
    "start": "react-scripts start",
    "watch": "react-scripts start --watch",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build",
    "test:coverage": "react-scripts test --env=jsdom --coverage --watchAll=false",
    "sonar": "sonar-scanner",
    "analyze": "npm run test:coverage && npm run sonar"
  }
}
```

#### Add dev dependencies:
```bash
npm install --save-dev sonarqube-scanner eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y @babel/eslint-parser @babel/preset-react
```
<img width="1918" height="653" alt="Image" src="https://github.com/user-attachments/assets/a24a2afb-37a8-4156-893e-652e434e0f8e" />

---

###  Step 4: Run Your First Analysis

#### Method 1: Using npm Script (Recommended)
```bash
# Run complete analysis with coverage
npm run analyze
```
  <img width="1918" height="437" alt="Image" src="https://github.com/user-attachments/assets/7da6df6d-6a8a-4e85-aaf0-404944eb0abf" />


#### Method 2: Step-by-Step Analysis
```bash
# 1. Generate test coverage
npm run test:coverage

# 2. Run ESLint
npm run lint

# 3. Run SonarQube analysis
npm run sonar
```
  <img width="1918" height="848" alt="Image" src="https://github.com/user-attachments/assets/192fed31-45d7-4867-a479-302aeda00200" />


#### Method 3: Direct SonarQube Scanner
```bash
# Using installed scanner
sonar-scanner \
  -Dsonar.projectKey=ot-go-webapp \
  -Dsonar.sources=src \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-generated-token
```
  

---
### Step 5: View Results

#### 1. Open SonarQube Dashboard
- Go to: http://localhost:9000/dashboard?id=ot-go-webapp&selectedTutorial=local
- What You'll See
- **Quality Overview Dashboard**
  <img width="1281" height="933" alt="Image" src="https://github.com/user-attachments/assets/ebea27f9-b3a9-442c-b833-8f4038fddade" />
- **Code Issues Priority**
  <img width="1297" height="932" alt="Image" src="https://github.com/user-attachments/assets/a9d6237a-2b86-48a5-976a-ea2c16aba965" />
  
---

## Troubleshooting Common Issues


| Issue                   | Quick Fix                                                           |
| ----------------------- | ------------------------------------------------------------------- |
| Connection refused      | Start container: `docker start sonarqube-react` and check port 9000 |
| Authentication failed   | Generate new token in SonarQube and update `sonar.login`            |
| No coverage info        | Run `npm run test:coverage` and set correct `lcov.info` path        |
| TS files not analyzed   | Install TS ESLint plugins and check `tsconfig.json` path            |
| React rules not applied | Install React ESLint plugins and update `.eslintrc.json`            |
| Analysis too slow       | Exclude unused files and increase Node memory                       |
| Permission denied       | Fix file/Docker permissions with `chmod` or proper user             |

---

## **Conclusion**

By following this POC, you have implemented an efficient process to identify and address potential problems early in the React development cycle, improving application performance, security, and user experience while reducing technical debt. SonarQube provides a reliable and continuous way to enforce coding standards and deliver cleaner, more accessible React applications with minimal overhead.

The integration of ESLint, Prettier, TypeScript, and dependency scanning with SonarQube's centralized reporting creates a comprehensive analysis pipeline suitable for modern React development teams.


---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**

| **Links**                                                    | **Overview**                                                                  |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| [SonarQube JavaScript/TypeScript](https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/languages/javascript-typescript/) | Official documentation for JavaScript/TypeScript analysis in SonarQube |
| [React ESLint Plugin](https://github.com/jsx-eslint/eslint-plugin-react) | React-specific ESLint rules and best practices |
| [ESLint React Hooks Plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) | ESLint rules for React Hooks usage patterns |
| [SonarQube Rules Explorer](https://rules.sonarsource.com/javascript) | Comprehensive list of JavaScript/TypeScript rules in SonarQube |
| [React Testing Best Practices](https://reactjs.org/docs/testing.html) | Official React testing documentation |
