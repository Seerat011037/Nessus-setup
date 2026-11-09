# Task 3: Basic Vulnerability Scan with Nessus on Kali Linux

![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?logo=kalilinux&logoColor=white)
![Tool](https://img.shields.io/badge/Tool-Nessus%20Essentials-00C1DE)
![Task](https://img.shields.io/badge/Task-Vulnerability%20Assessment-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

A hands-on introduction to vulnerability scanning. In this task I installed **Tenable Nessus** on **Kali Linux**, scanned my own machine, and documented the setup, the exact commands I ran, the findings, and how to fix them.

> **Legal notice:** Only scan systems you own or have written permission to test. Everything in this repository was done on my own machine for learning purposes.

---

## Table of Contents

1. [Objective](#1-objective)
2. [Environment](#2-environment)
3. [Installation and Setup](#3-installation-and-setup)
4. [Running the Vulnerability Scan](#4-running-the-vulnerability-scan)
5. [Scan Results](#5-scan-results)
6. [Example Finding](#6-example-finding)
7. [Remediation Summary](#7-remediation-summary)
8. [Troubleshooting](#8-troubleshooting)
9. [Interview Questions and Answers](#9-interview-questions-and-answers)
10. [Key Concepts](#10-key-concepts)
11. [Repository Structure](#11-repository-structure)
12. [References](#12-references)

---

## 1. Objective

Use a free tool to identify common vulnerabilities on a personal computer, review their severity, and research simple fixes.

**Deliverable:** A vulnerability scan report with identified issues and mitigations.

**Outcome:** Introductory vulnerability assessment experience and an understanding of common PC risks.

---

## 2. Environment

| Item | Details |
|---|---|
| Operating System | Kali Linux |
| Scanner | Tenable Nessus (Essentials, free tier) |
| Nessus Package | `Nessus-10.12.6-ubuntu1604_amd64.deb` |
| Browser | Firefox |
| Scan Target | `localhost` / `127.0.0.1` (own machine) |
| Nessus Web Interface | `https://localhost:8834` |

---

## 3. Installation and Setup

### Step 1: Download Nessus

1. Open Firefox and go to the [Tenable Nessus download page](https://www.tenable.com/downloads/nessus).
2. Choose **Version:** `Nessus - 10.12.6`
3. Choose **Platform:** `Linux - Ubuntu - amd64` (this `.deb` package works on Kali, which is Debian-based).
4. Click **Download** and accept the license agreement.

<p align="center">
  <img src="screenshots/01-nessus-download.png" alt="Downloading Nessus from the Tenable website in Firefox" width="750">
  <br>
  <em>Figure 1: Downloading the Nessus .deb package from the Tenable website</em>
</p>

### Step 2: Move to the Downloads folder and confirm the file

```bash
cd Downloads
ls
```

You should see `Nessus-10.12.6-ubuntu1604_amd64.deb` in the list.

### Step 3: Install the package

```bash
sudo dpkg -i Nessus-10.12.6-ubuntu1604_amd64.deb
```

Enter your Kali password when prompted. The installer unpacks Nessus and runs its self-tests (HMAC, SHA, AES, RSA, ECDSA and others). All of them should show `Pass`.

<p align="center">
  <img src="screenshots/02-dpkg-install.png" alt="Terminal output of the dpkg install command for Nessus" width="750">
  <br>
  <em>Figure 2: Installing Nessus with dpkg and passing the integrity checks</em>
</p>

### Step 4: Start the Nessus service

```bash
sudo systemctl start nessusd.service
```

### Step 5: Verify the service is running

```bash
sudo systemctl status nessusd.service
```

Look for `Active: active (running)` in the output.

<p align="center">
  <img src="screenshots/03-start-service-status.png" alt="systemctl status showing nessusd.service active and running" width="750">
  <br>
  <em>Figure 3: nessusd.service is active (running)</em>
</p>

### Step 6 (optional): Start Nessus automatically at boot

The status output shows the service as `disabled`, so it will not start after a reboot. To change that:

```bash
sudo systemctl enable nessusd.service
```

### Step 7: Open the web interface

In Firefox, go to:

```
https://localhost:8834
```

Your browser will show a certificate warning because Nessus uses a self-signed certificate. Click **Advanced** and then **Accept the Risk and Continue**.

### Step 8: Choose Nessus Essentials and activate

1. Select **Nessus Essentials** (free, up to 16 IP addresses).
2. Register at Tenable with your name and email to receive a free **activation code**.
3. Enter the activation code, then create a Nessus **username and password**.

> Do not publish your activation code or password in this repository.

### Step 9: Wait for initialization and plugin download

Nessus compiles its plugins on first launch. This can take **10 to 30 minutes or more**, depending on your connection and hardware.

<p align="center">
  <img src="screenshots/04-nessus-initializing.png" alt="Nessus initializing screen at localhost:8834" width="750">
  <br>
  <em>Figure 4: Nessus initializing</em>
</p>

<p align="center">
  <img src="screenshots/05-downloading-plugins.png" alt="Nessus downloading plugins during initialization" width="750">
  <br>
  <em>Figure 5: Nessus downloading plugins</em>
</p>

---

## 4. Running the Vulnerability Scan

### Step 1: Find your machine's IP address

```bash
ip a
# or
hostname -I
```

For this task I scanned the local machine. Example addresses used in this README:

| Purpose | Address |
|---|---|
| Loopback (localhost) | `127.0.0.1` |
| Sample LAN IP of my Kali machine | `192.168.1.105` |

> `192.168.1.105` is a **sample private IP** used for documentation. Replace it with your own address if you publish your own results.

### Step 2: Create a new scan

1. In Nessus, click **New Scan**.
2. Select **Basic Network Scan** (a good full scan template for a single host).
3. Fill in the settings:

| Field | Value |
|---|---|
| Name | `Task3 - Localhost Basic Scan` |
| Description | `Basic vulnerability scan of my own Kali Linux machine` |
| Targets | `127.0.0.1` (or `192.168.1.105`) |

4. Click **Save**.

<p align="center">
  <img src="screenshots/06-scan-configuration.png" alt="Nessus scan configuration screen" width="750">
  <br>
  <em>Figure 6: Scan configuration (add your screenshot here)</em>
</p>

### Step 3: Launch the scan

Click the **Launch** (play) button next to the scan and wait for it to finish. A scan of a single host usually takes **30 to 60 minutes**.

<p align="center">
  <img src="screenshots/07-scan-running.png" alt="Nessus scan in progress" width="750">
  <br>
  <em>Figure 7: Scan in progress (add your screenshot here)</em>
</p>

---

## 5. Scan Results

After the scan completes, open it and review the **Vulnerabilities** tab. Nessus groups findings by severity: **Critical, High, Medium, Low, and Info**.

<p align="center">
  <img src="screenshots/08-scan-results-summary.png" alt="Nessus scan results summary by severity" width="750">
  <br>
  <em>Figure 8: Results summary by severity (add your screenshot here)</em>
</p>

Fill in this table from your own scan:

| Severity | Count |
|---|---|
| Critical | 0 |
| High | 0 |
| Medium | 1 |
| Low | 0 |
| Info | 0 |

> Replace these numbers with the real counts from your scan.

### Export the report

1. Open the completed scan.
2. Click **Report** in the top right.
3. Choose **PDF** or **HTML**, then select **Executive Summary** or **Complete List of Vulnerabilities by Host**.
4. Save the file in the `report/` folder of this repository.

---

## 6. Example Finding

> **Note:** The finding below is a **sample example** showing how I document an issue, using a sample IP address. Replace the values with the actual details from your own Nessus report before publishing.

### Finding 1: SSL Self-Signed Certificate

| Field | Details |
|---|---|
| **Target** | `192.168.1.105` (sample IP of my Kali machine) |
| **Port / Protocol** | `8834 / tcp` (Nessus web interface) |
| **Plugin** | SSL Self-Signed Certificate |
| **Severity** | Medium |
| **CVSS** | Around 6.4 (check your report for the exact vector) |
| **Detected by** | Nessus Basic Network Scan |

**Description**

The service on port 8834 presents an X.509 certificate that is self-signed. Because no trusted Certificate Authority (CA) has verified it, users cannot confirm the server's identity, and a man-in-the-middle attacker could present a fake certificate that looks equally untrusted.

**Risk**

Without a trusted certificate, users get used to clicking through browser warnings. This weakens protection against interception of the login credentials and scan data sent to the service.

**Evidence (sample)**

```text
The following certificate was found at the top of the certificate
chain sent by the remote host, but is self-signed and was not
found in the list of known certificate authorities :

|-Subject : O=Nessus Users United/OU=Nessus Server/CN=kali
```

<p align="center">
  <img src="screenshots/09-vulnerability-detail.png" alt="Nessus vulnerability detail page for the finding" width="750">
  <br>
  <em>Figure 9: Vulnerability detail page (add your screenshot here)</em>
</p>

**Remediation**

- Replace the self-signed certificate with one issued by a trusted CA, or by an internal CA that your devices trust.
- If Nessus is only used locally, restrict access to `localhost` so the service is not exposed on the network.
- Block port `8834` from other machines with a firewall rule:

```bash
sudo apt install ufw -y
sudo ufw deny 8834/tcp
sudo ufw enable
sudo ufw status
```

> Because this is a local-only lab tool, the practical risk is low. In a production environment it would still need to be fixed.

---

## 7. Remediation Summary

| # | Vulnerability | Severity | Recommended Fix | Status |
|---|---|---|---|---|
| 1 | SSL Self-Signed Certificate (sample) | Medium | Use a CA-signed certificate or restrict access to localhost | Documented |
| 2 | Outdated packages (common finding) | Varies | `sudo apt update && sudo apt full-upgrade -y` | To do |
| 3 | Unneeded open ports/services (common finding) | Varies | Stop and disable unused services, add firewall rules | To do |

Useful hardening commands for a Kali machine:

```bash
# Update the system
sudo apt update && sudo apt full-upgrade -y

# List listening ports and the services behind them
sudo ss -tulpn

# Stop and disable a service you do not need
sudo systemctl stop <service-name>
sudo systemctl disable <service-name>
```

---

## 8. Troubleshooting

| Problem | Fix |
|---|---|
| Downloads in Firefox show `Failed` | Retry the download with the refresh button, or use the **Download by curl** option on the Tenable page. |
| `dpkg` reports missing dependencies | Run `sudo apt --fix-broken install -y` and install again. |
| `https://localhost:8834` does not load | Check the service with `sudo systemctl status nessusd.service` and start it if needed. |
| Stuck on "Downloading plugins" | Check your internet connection and wait. You can also run `sudo /opt/nessus/sbin/nessuscli update --all`. |
| Browser certificate warning | Expected. Nessus uses a self-signed certificate (see the example finding above). |
| Scan is very slow | Close other heavy applications and avoid scanning over a weak network. |

To stop Nessus when you are finished:

```bash
sudo systemctl stop nessusd.service
```

---

## 9. Interview Questions and Answers

**1. What is vulnerability scanning?**
It is the automated process of checking systems, networks, and applications for known weaknesses such as missing patches, misconfigurations, and weak settings, then reporting them by severity.

**2. What is the difference between vulnerability scanning and penetration testing?**
A vulnerability scan is automated and identifies *potential* weaknesses. A penetration test is mostly manual: a tester actively tries to *exploit* weaknesses to prove real impact and show how far an attacker could get.

**3. What are some common vulnerabilities in personal computers?**
Outdated operating systems and software, missing security patches, weak or reused passwords, unnecessary open ports and services, disabled firewalls, default configurations, and self-signed or expired certificates.

**4. How do scanners detect vulnerabilities?**
They discover live hosts and open ports, identify running services and versions (fingerprinting), then run checks (plugins) that compare those versions and configurations against a database of known vulnerabilities such as CVEs. Some checks also send safe probe requests to confirm the weakness.

**5. What is CVSS?**
The Common Vulnerability Scoring System is a standard that rates vulnerability severity from 0.0 to 10.0 (None, Low, Medium, High, Critical) based on factors such as attack vector, complexity, privileges required, and impact on confidentiality, integrity, and availability.

**6. How often should vulnerability scans be performed?**
At least monthly or quarterly for most organizations, after any major system change, and continuously for critical or internet-facing assets. Some compliance standards set a required frequency.

**7. What is a false positive in vulnerability scanning?**
A finding reported as a vulnerability that does not actually exist, for example because the scanner misread a version number or a patch was applied in a way it could not detect. Each important finding should be verified manually.

**8. How do you prioritize vulnerabilities?**
By severity (CVSS), whether a public exploit exists, how exposed the asset is, how critical the asset or its data is to the business, and how easy the fix is. Critical, exploitable, internet-facing issues come first.

---

## 10. Key Concepts

- **Vulnerability scanning:** Automated discovery of known weaknesses.
- **Risk assessment:** Combining severity, likelihood, and impact to decide what to fix first.
- **CVSS:** A standard 0 to 10 severity score.
- **Remediation:** Fixing or reducing a vulnerability through patching, configuration changes, or compensating controls.
- **Security tools:** Nessus, OpenVAS, Nmap, and others.

---

## 11. Repository Structure

```text
.
├── README.md
├── screenshots/
│   ├── 01-nessus-download.png
│   ├── 02-dpkg-install.png
│   ├── 03-start-service-status.png
│   ├── 04-nessus-initializing.png
│   ├── 05-downloading-plugins.png
│   ├── 06-scan-configuration.png        <- add
│   ├── 07-scan-running.png              <- add
│   ├── 08-scan-results-summary.png      <- add
│   └── 09-vulnerability-detail.png      <- add
└── report/
    └── Nessus_Scan_Report.pdf           <- add
```

---

## 12. References

- [Tenable Nessus downloads](https://www.tenable.com/downloads/nessus)
- [Nessus documentation](https://docs.tenable.com/nessus/Content/GetStarted.htm)
- [CVSS specification (FIRST)](https://www.first.org/cvss/)
- [OWASP Vulnerability Management Guide](https://owasp.org/)
- [Kali Linux documentation](https://www.kali.org/docs/)

---

## Author

**Seerat Fatima**
Cybersecurity Analyst | Penetration Testing | Vulnerability Assessment
[LinkedIn](https://linkedin.com/in/seeratfatima-25b5b9412)

---

*This project was completed for educational purposes on a machine I own.*
