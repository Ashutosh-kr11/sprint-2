
<img src="https://github.com/user-attachments/assets/a7fede05-a973-401b-a600-bb82f88eb050" alt="Image" width="100%" />

# License Scanning POC: Complete Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-12     | 1.0         | 2025-07-12          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [License Scanning Documentation](#license-scanning-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)

  * [Create FOSSA Account](#create-fossa-account)
  * [Install FOSSA CLI](#install-fossa-cli)
  * [Get Your API Key](#get-your-api-key)
  * [Basic Project Setup](#basic-project-setup)
  * [Run License Scanning](#run-license-scanning)
  * [Analyze and Resolve Issues](#analyze-and-resolve-issues)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This Proof of Concept (POC) demonstrates how FOSSA can be integrated into your project to automate license scanning and compliance management. FOSSA helps identify and resolve licensing issues related to third-party dependencies, ensuring your project stays compliant with open-source licenses. The following steps will guide you through setting up FOSSA, scanning your codebase, and handling any detected issues.

---

## **Pre-requisites**

Before starting, ensure you have:

* A GitHub account with a repository
* Basic familiarity with command line/terminal
* Git installed on your machine
* Node.js, Python, or other language runtime (depending on your project)

---

## **License Scanning Documentation**

Follow this link for "License Scanning" POC:

> [Licence Scanning Doc](https://github.com/Ashutosh-kr11/sprint-2/blob/main/License%20Scanning%3ADoc.md)

---

## **Step-by-Step Instructions**

### **Create FOSSA Account**

**1. Sign Up for FOSSA**

* Visit [https://fossa.com](https://fossa.com)
* Click **"Get Started"** or **"Sign Up"**
* Choose between:

  * Free Tier: Good for personal/small projects (limited features)
  * Trial: Full features for 14 days
  * Paid Plan: For commercial use

  <img width="1226" height="1049" alt="Image" src="https://github.com/user-attachments/assets/68cdc21a-d064-4bba-b81c-142fa56605bb" />

**2. Complete your profile information**

---

### **Install FOSSA CLI**

**Quick Install (Linux/macOS)**
Run the following command to install the latest version of FOSSA CLI:

```bash
curl -H 'Cache-Control: no-cache' https://raw.githubusercontent.com/fossas/fossa-cli/master/install-latest.sh | bash
```

  <img width="1918" height="850" alt="Image" src="https://github.com/user-attachments/assets/507b645a-1fe1-4c80-ba5b-251a6d9e7407" />

**Verify Installation**

```bash
fossa --version
```

You should see output like:
`fossa-cli version 3.x.x`

---

### **Get Your API Key**

**From FOSSA Dashboard**

* Log into your FOSSA account
* Go to **"Settings" → "Integration Settings"**
* Click **"API"**
* Create a new token by clicking **"Add New Token"**
* Copy the API key (keep it secure!)

  <img width="1208" height="1055" alt="Image" src="https://github.com/user-attachments/assets/391a750b-f5c9-4054-806a-7ea40755475e" />

**Authenticate FOSSA CLI**

```bash
export FOSSA_API_KEY=<your_api_key>
```

  <img width="1918" height="27" alt="Image" src="https://github.com/user-attachments/assets/26c86fee-e10d-473a-9a65-effe1aee5fae" />

---

### **Basic Project Setup**

**1. Clone Your Repository**

```bash
git clone https://github.com/OT-MICROSERVICES/employee-api.git
cd employee-api
```

  <img width="1918" height="82" alt="Image" src="https://github.com/user-attachments/assets/cbb12796-0be9-45f1-8339-60bf69a2021c" />

**2. Initialize FOSSA Configuration**

```bash
fossa init
```

  <img width="1918" height="163" alt="Image" src="https://github.com/user-attachments/assets/4f2627fa-52ce-4f7c-bd6f-f66cdcfee80e" />

**3. Customize Configuration (Optional)**

* Edit `.fossa.yml` to match your project.

---

### **Run License Scanning**

Trigger a manual license scan by running:

```bash
fossa analyze
```

  <img width="1852" height="886" alt="Image" src="https://github.com/user-attachments/assets/ac91a38b-869a-4050-ada4-9b7455efd077" />

---

### **Analyze and Resolve Issues**

**1. Review Scan Results on the FOSSA Dashboard**

* After the scan completes, check the results in the FOSSA dashboard.
* Any detected issues, such as conflicting or missing licenses, will be clearly highlighted.
* Go through the detailed report to understand each flagged item and take necessary corrective actions.
* FOSSA may also provide recommendations, such as updating license types or including the required attributions, to help you resolve issues effectively.

  <img width="1208" height="1573" alt="Image" src="https://github.com/user-attachments/assets/efa3cd1c-0a36-4def-903d-232fe55f0ee9" />

**2. Verify Compliance Using `fossa test`**

```bash
fossa test
```

* If the test passes, the output will indicate:
  `0 issues found`

  <img width="1918" height="932" alt="Image" src="https://github.com/user-attachments/assets/9e17ed8b-d9e2-4625-b5dc-d4abf59689f3" />

---

## **Conclusion**

Integrating FOSSA helps ensure your project remains compliant with license requirements by automatically scanning dependencies for potential issues. By following this POC, you have implemented a streamlined process to identify and address license conflicts effectively, maintaining legal compliance with minimal effort. FOSSA offers a dependable way to manage open-source dependencies and their licenses.

---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**

| **Links**                                               | **Overview**                                                   |
| ------------------------------------------------------- | -------------------------------------------------------------- |
| [FOSSA Docs](https://fossa.com/docs/)                   | Official documentation for using FOSSA for license compliance. |
| [Open Source Licenses](https://opensource.org/licenses) | General guidelines and information on open-source licenses.    |
| [FOSSA GitHub](https://github.com/fossas/fossa-cli)     | FOSSA's open-source CLI tool repository.                       |

---
