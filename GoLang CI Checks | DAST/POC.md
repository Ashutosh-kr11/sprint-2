
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
  * [Step 4: Integrate with CI/CD](#step-4-integrate-with-cicd)
  * [Step 5: Review Reports](#step-5-review-reports)
* [Troubleshooting Common Issues](#troubleshooting-common-issues)
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

**Option B: Using Docker (Recommended for CI/CD)**

```bash
docker pull owasp/zap2docker-stable
```

---

### Step 2: Start Golang App

Example Go web app (REST API or web server):

```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, Golang DAST POC!")
}

func main() {
	http.HandleFunc("/", handler)
	fmt.Println("Server running on http://localhost:8080")
	http.ListenAndServe(":8080", nil)
}
```

Run it:

```bash
go run main.go
```

---

### Step 3: Run DAST Scan

**Quick ZAP Scan (CLI):**

```bash
zaproxy -cmd -quickurl http://localhost:8080 -quickout zap_report.html
```

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

---

## **Troubleshooting Common Issues**

| Issue                          | Quick Fix                                                     |
| ------------------------------ | ------------------------------------------------------------- |
| ZAP can’t reach app            | Ensure Golang app is listening on correct port (e.g., `8080`) |
| `Connection refused` in Docker | Use `host.docker.internal` instead of `localhost`             |
| Reports empty                  | Use `zap-full-scan.py` for deeper analysis                    |
| Long scans                     | Prefer `zap-baseline.py` for quicker passive scans            |

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

---

👉 Do you want me to prepare this as a **Markdown `.md` file** (ready to drop in your repo like your SonarQube doc), with example screenshots placeholders, so it’s in the exact same format?
