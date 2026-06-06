# IKB21403-Vulnerability Assessment Labs

---

## Student Information

| Item             | Details                     |
| ---------------- | --------------------------- |
| Course           | Vulnerability Assessment    |
| Tool Used        | OpenVAS 
| Target Machine   | Metasploitable2     |
| Attacker Machine | Kali Linux                  |

---

# Lab 1: Host-Based Vulnerability Assessment

## Objective

To identify, classify, and prioritize vulnerabilities on a target host using a vulnerability scanner.


## Scope Definition

| Item              | Details                  |
| ----------------- | ------------------------ |
| Target Host       | Metasploitable2          |
| Target IP Address |   10.0.2.4               |
| Purpose           | Vulnerability Assessment |
| Scanner           |      openVas             |
| Scan Template     | Basic Network Scan       |


## Network Verification

 Ping Test

```bash
ping 10.0.2.4
```

## **Evidence**

<img width="490" height="113" alt="image" src="https://github.com/user-attachments/assets/8c56f241-535e-4169-b30b-313b556fa643" />


## Scan Configuration

## **Scan Settings**

* Scanner: openVas
* Template: Full and Fast
* Target: 10.0.2.4


## **Scan Results**
<img width="959" height="250" alt="image" src="https://github.com/user-attachments/assets/7cb99c21-413e-4368-adbf-342cdd91b236" />


## **Top 5 Vulnerabilities Identified**

| No. | Vulnerability Name | CVE ID | CVSS Score | Severity | Port/Service |
|------|-------------------|---------|------------|----------|-------------|
| 1 | Distributed Ruby (dRuby/DRb) Multiple Remote Code Execution | N/A | 10.0 | Critical | 8787/tcp |
| 2 | Possible Backdoor: Ingreslock | N/A | 10.0 | Critical | 1524/tcp |
| 3 | TWiki < 4.2.4 Cross-Site Scripting (XSS) / Command Execution | CVE-2008-5304, CVE-2008-5305 | 10.0 | Critical | 80/tcp |
| 4 | The rexec Service is Running | CVE-1999-0618 | 10.0 | Critical | 512/tcp |
| 5 | OS End of Life Detection (Ubuntu 8.04) | N/A | 10.0 | Critical | General / Multiple Services |


## **Conclusion**

The scan successfully identified multiple vulnerabilities affecting the target host. These findings will be further analyzed in Lab 2.

---
# Lab 2: Vulnerability Analysis and Interpretation

## **Objective**

The objective of this lab is to analyze vulnerabilities identified during Lab 1, understand their technical details, and determine whether they are exploitable within the current lab environment.

---

## Finding 1: TWiki < 4.2.4 XSS / Command Execution Vulnerabilities

## **Vulnerability Information**

| Attribute     | Value                                 |
| ------------- | ------------------------------------- |
| Vulnerability | TWiki < 4.2.4 XSS / Command Execution |
| CVE           | CVE-2008-5304, CVE-2008-5305          |
| CVSS Score    | 10.0 (CVE-2008-5305)                  |
| Severity      | Critical                              |
| Port          | 80/tcp                                |
| Service       | HTTP (TWiki Web Application)          |

## **CVSS Analysis**

 **CVSS Vector**

```text
AV:N/AC:L/Au:N/C:C/I:C/A:C
```

## **Breakdown**

| Metric                 | Value    |
| ---------------------- | -------- |
| Attack Vector          | Network  |
| Privileges Required    | None     |
| User Interaction       | Required   |

The vulnerability can be exploited remotely over the network without authentication and may result in complete compromise of the affected application.

## **CWE Mapping**

| CVE           | CWE                           |
| ------------- | ----------------------------- |
| CVE-2008-5304 | CWE-79 (Cross-Site Scripting) |
| CVE-2008-5305 | CWE-94 (Code Injection)       |

 ## **Environment Verification**

| Verification Item        | Result        |
| ------------------------ | ------------- |
| Service Running          | Yes           |
| Port Reachable from Kali | Yes (Port 80) |
| Authentication Required  | No            |


## **Conclusion**

**Likely Exploitable**

The TWiki web application is accessible through port 80 and does not require authentication for exploitation. Since the service is reachable from Kali Linux and the vulnerability can be triggered remotely, it is likely exploitable within the lab environment.

---

## Finding 2: The rexec Service is Running

## **Vulnerability Information**

| Attribute     | Value                        |
| ------------- | ---------------------------- |
| Vulnerability | The rexec Service is Running |
| CVE           | CVE-1999-0618                |
| CVSS Score    | 10.0                         |
| Severity      | Critical                     |
| Port          | 512/tcp                      |
| Service       | rexec                        |

## **CVSS Analysis**

## **CVSS Vector**

```text
AV:N/AC:L/Au:N/C:C/I:C/A:C
```

## **Breakdown**

| Metric                 | Value    |
| ---------------------- | -------- |
| Attack Vector          | Network  |
| Privileges Required    | None     |
| User Interaction       | None     |

The vulnerability exists because the rexec service is enabled and accessible over the network.

## **CWE Mapping**

| CWE                    |
| ---------------------- |
| CWE-16 (Configuration) |

## **Environment Verification**

| Verification Item        | Result                           |
| ------------------------ | -------------------------------- |
| Service Running          | Yes                              |
| Port Reachable from Kali | Yes (Port 512)                   |
| Authentication Required  | User credentials may be required |


## **Conclusion**

## **Potentially Exploitable**

Although the service is reachable from Kali Linux, successful exploitation may depend on the availability of valid user credentials. The risk remains high because the service exposes remote command execution functionality and should not be enabled on modern systems.

---

## Finding 3: Operating System (OS) End of Life (EOL) Detection

## **Vulnerability Information**

| Attribute          | Value                   |
| ------------------ | ----------------------- |
| Vulnerability      | Ubuntu 8.04 End of Life |
| CVE                | N/A                     |
| Severity           | Critical                |
| Affected Component | Operating System        |

## **CVSS Analysis**

This finding does not have a specific CVE because it is related to an unsupported operating system rather than a single software vulnerability.

## **CWE Mapping**

| CWE                                                   |
| ----------------------------------------------------- |
| CWE-1104 (Use of Unmaintained Third-Party Components) |

## **Environment Verification**

| Verification Item        | Result        |
| ------------------------ | ------------- 
| Service Running          | Yes (Ubuntu 8.04)  |
| Port Reachable from Kali | Yes  |
| User Interaction         | None   |


## **Conclusion**

**Indirectly Exploitable**

The operating system itself is not a vulnerability; however, because Ubuntu 8.04 has reached end-of-life status, newly discovered vulnerabilities are no longer patched. This significantly increases the likelihood that attackers can exploit unpatched software and services running on the system.


## **Overall Analysis**

The three vulnerabilities demonstrate different categories of security issues:

| Category        | Finding                       |
| --------------- | ----------------------------- |
| Web Application | TWiki XSS / Command Execution |
| Network Service | rexec Service Running         |
| System Platform | Ubuntu 8.04 End of Life       |

Although all findings were reported as Critical by the scanner, actual risk depends on environmental factors such as service availability, network accessibility, and authentication requirements. This demonstrates that CVSS scores alone should not be used to determine real-world risk.

---

# Lab 3: Vulnerability Validation

##  **Objective**

To manually verify scanner findings and determine whether they represent actual security risks.

## Finding 1: SSL Weak Cipher

```bash
nmap --script ssl-enum-ciphers -p 443 10.0.2.4
```


**Result**

<img width="387" height="98" alt="image" src="https://github.com/user-attachments/assets/10fe6f91-05ea-4638-a80f-d227792c7e57" />


**Verdict & Reasoning**

False Positive, because the scanner flagged SSL weak cipher but port 443 is closed on this host. No SSL/TLS service is running, therefore this finding cannot be validated.
Thus, the risk does not exist in this environment

---

## Finding 2: Open Port with No Authentication

```bash
nmap -sV <target-ip>
```


**Result**

<img width="448" height="113" alt="image" src="https://github.com/user-attachments/assets/43c7c52f-5dcc-4712-9e31-41ee85356255" />



**Verdict & Reasoning**

True Positive, because the rexec service is confirm running on port 512. The manual verification using nmap -sV confirms the service
is active and accessible from Kali with no authentication required.

---

## **Finding 3: Outdated Service**

```bash
nmap -sV <target-ip>
```



**Result**

<img width="323" height="101" alt="image" src="https://github.com/user-attachments/assets/d59adf4d-27bf-402d-8777-7f615b63648d" />



**Verdict & Reasoning**

True positive. This is because banner grab confirms Apache 2.2.8 and PHP 5.2.4 are both end of life and no longer
receiving security updates. These outdaated versions contain known vulnerabilities exploitable from Kali. Thus, validated the finding.

---

# Lab 4: Risk-Based Vulnerability Prioritisation

**Objective**

To prioritize vulnerabilities based on risk rather than severity alone.

**Risk Scoring Table**

| No. | Vulnerability | Exploitability (1–5) | Impact (1–5) | Exposure (1–5) | Total Score |
|-----|---------------|----------------------|--------------|----------------|-------------|
| 1 | TWiki < 4.2.4 XSS / Command Execution | 5 | 5 | 5 | 15 |
| 2 | Distributed Ruby (dRuby/DRb) Multiple RCE | 5 | 5 | 5 | 15 |
| 3 | Possible Backdoor: Ingreslock | 5 | 5 | 5 | 15 |
| 4 | The rexec Service is Running | 4 | 5 | 5 | 14 |
| 5 | OS End of Life Detection (Ubuntu 8.04) | 3 | 5 | 4 | 12 |

## **Remediation Priority List**

---

### Priority 1

### TWiki < 4.2.4 XSS / Command Execution

Reason:
This vulnerability is ranked as the highest priority because it affects a web application that is directly reachable
from the attacker machine that is Kali Linux over HTTP. It does not require authentication and has known remote command execution potential.
In a lab environment where DVWA/TWiki is fully accessible, the exposure is maximum, making it highly exploitable.

---

### Priority 2

### Distributed Ruby (dRuby/DRb) Multiple RCE

Reason:
This service allows remote code execution over the network without authentication. It is highly critical due to the potential for full system compromise.
However, compared to TWiki, is it less intuitive to be exploit in a real world scenario.

---

### Priority 3

### **Possible Backdoor: Ingreslock**

Reason:
This service exposes a legacy backdoor on port 1524/tcp. This is dangerous because it is publicly reachable in the lab environment and may allow unauthorized access.
However, exploitability may depend on how the service responds during the manual validation.

---

### Priority 4

### **rexec Service Running**

Reason:
The rexec service is insecure due to plaintext authentication and remote command execution capability. However, it may require valid credentials, which reduces it immediate exploitability in the 
current environment

---

### Priority 5

### **OS End of Life Detection**

Reason:
Although it is critical in severity, this finding is not directly exploitable. It risk depends on other services running on the system.
However, it remains important because an unsupported OS increases long term vulnerability exposure

---

## Medium CVSS vs High CVSS Example

A Medium severity vulnerability that is internet-facing and requires no authentication may pose a greater risk than a High severity vulnerability located on an isolated internal system. This demonstrates why contextual risk assessment is essential when prioritizing remediation efforts.

---

# Overall Reflection

Through these labs, vulnerability assessment skills were developed by learning how to identify vulnerabilities, interpret scanner findings, validate results manually, and prioritize remediation efforts based on risk rather than severity alone.

