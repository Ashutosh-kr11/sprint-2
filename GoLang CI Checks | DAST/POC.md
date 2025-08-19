
<img width="100%" height="336" alt="Image" src="https://github.com/user-attachments/assets/b5d8fc92-4036-4216-9495-e292fcdec2b9" />

# OWASP ZAP for DAST in Golang CI Checks POC: Implementation Guide

---

## Document Metadata

| **Author**     | **Created on** | **Version** | **Last updated on** | **Level** | **Reviewer**    |
| -------------- | -------------- | ----------- | ------------------- | --------- | --------------- |
| Ashutosh Kumar | 2025-08-19     | 1.0         | 2025-07-14          | Internal  | Siddharth Pawar |

---

## **Table of Contents**

* [Introduction](#introduction)
* [Pre-requisites](#pre-requisites)
* [DAST for Golang CI Checks Documentation](#dast-for-golang-ci-checks-documentation)
* [Step-by-Step Instructions](#step-by-step-instructions)

  * [Step 1: Setup OWASP ZAP](#step-1-setup-owasp-zap)
  * [Step 2: Start Golang App](#step-2-start-golang-app)
  * [Step 3: Run DAST Scan](#step-3-run-dast-scan)
  * [Step 4: Review Reports](#step-4-review-reports)
* [Troubleshooting](#troubleshooting)
* [Conclusion](#conclusion)
* [Contact Information](#contact-information)
* [References](#references)

---

## **Introduction**

This Proof of Concept (POC) demonstrates how to integrate **Dynamic Application Security Testing (DAST)** using **OWASP ZAP** for a **Golang application**.

Unlike SAST or SCA, DAST tests the **running Go application** by simulating real-world attacks (SQLi, XSS, CSRF, etc.) without access to the source code.

This POC covers:

* Setup OWASP ZAP scanning
* Scanning Golang REST APIs & web apps
* Vulnerability reporting (HTML/JSON/XML)
* Detection of runtime misconfigurations and vulnerabilities

---

## **Pre-requisites**

| Requirement              | Version | Check Command      |
| ------------------------ | ------- | ------------------ |
| **Go (Golang)**          | 1.18+   | `go version`       |
| **Docker**               | Latest  | `docker --version` |
| **OWASP ZAP CLI/Docker** | 2.13+   | `zaproxy -h`       |
| **Git**                  | Latest  | `git --version`    |

---

## **DAST for Golang CI Checks Documentation**

> [OWASP ZAP Documentation](https://www.zaproxy.org/docs/)

---

## **Step-by-Step Instructions**

### Step 1: Setup OWASP ZAP

**Option A: Install natively on Ubuntu 24.04**

```bash
sudo snap install zaproxy --classic
```
<img width="1913" height="138" alt="Image" src="https://github.com/user-attachments/assets/f0312236-b493-403c-96ff-d707e4b4120d" />

**Option B: Using Docker (Recommended for CI/CD)**

```bash
docker pull owasp/zap2docker-stable
```

---

### Step 2: Start Golang App

Clone OT-MICROSERVICES/employee-api:

```bash
git clone https://github.com/OT-MICROSERVICES/employee-api.git
```
<img width="1918" height="110" alt="Image" src="https://github.com/user-attachments/assets/8fd55b3c-99b2-43c1-8aca-2fcc6126c50b" />

Run it:

```bash
cd employee-api 
make build 
./employee-api
```

<img width="1918" height="620" alt="Image" src="https://github.com/user-attachments/assets/95e7e5d8-d512-445f-acf3-49fb17a473f6" />

---

### Step 3: Run DAST Scan

**Quick ZAP Scan (CLI):**

```bash
zaproxy -cmd -port 9090 -quickurl http://localhost:8080/swagger/index.html -quickout /home/ashutosh-kr/zap_report.html
```
<img width="1918" height="178" alt="Image" src="https://github.com/user-attachments/assets/983f1637-c1c0-4ff7-b6df-0eac29a96ef1" />


**Using Docker (Baseline Scan):**

```bash
docker run -t owasp/zap2docker-stable zap-baseline.py \
    -t http://host.docker.internal:8080 \
    -r zap_report.html
```

---

### Step 4: Review Reports

* `zap_report.html` → Open in browser to review findings
* JSON/XML outputs → integrate into security dashboards

<img width="1918" height="985" alt="Image" src="https://github.com/user-attachments/assets/b610243d-84d8-42fc-9c20-abfbd2f9bf20" />

---

## **Troubleshooting**

| Issue                          | Quick Fix                                                     |
| ------------------------------ | ------------------------------------------------------------- |
| ZAP can’t reach app            | Ensure Golang app is listening on correct port (e.g., `8080`) |
| `Connection refused` in Docker | Use `host.docker.internal` instead of `localhost`             |
| Reports empty                  | Use `zap-full-scan.py` for deeper analysis                    |
| Long scans                     | Prefer `zap-baseline.py` for quicker passive scans            |
| use `-port`                    | change port                                                   |

---

## **Conclusion**

By integrating **OWASP ZAP DAST** into your **Golang CI/CD pipeline**, you can automatically test your app against common runtime vulnerabilities, reducing risks before production release.

This ensures stronger **security posture**, faster feedback, and compliance with **OWASP Top 10** guidelines.

---

## **Contact Information**

| **Name**       | **Email Address**                                                                     |
| -------------- | ------------------------------------------------------------------------------------- |
| Ashutosh Kumar | [ashutosh.kumar.snaatak@mygurukulam.co](mailto:ashutosh.kumar.snaatak@mygurukulam.co) |

---

## **References**

| **Links**                                                                        | **Overview**                      |
| -------------------------------------------------------------------------------- | --------------------------------- |
| [OWASP ZAP Docs](https://www.zaproxy.org/docs/)                                  | Official documentation            |
| [OWASP Top 10](https://owasp.org/www-project-top-ten/)                           | Most critical web app risks       |
| [ZAP GitHub Actions](https://github.com/marketplace/actions/owasp-zap-full-scan) | Ready-to-use CI integration       |
| [Go Security Policy](https://golang.org/security)                                | Official Golang security guidance |

