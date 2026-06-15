# Enterprise USB Device Restriction Using Active Directory GPO

This project demonstrates how to centrally block removable storage devices across a Windows Active Directory environment using Group Policy Objects (GPO).

The implementation prevents users from accessing USB flash drives, external hard drives, external SSDs, SD cards, and other removable storage devices. This security control helps reduce data exfiltration risks, prevent malware introduction through removable media, and improve enterprise endpoint security.

---

# Overview

Removable storage devices remain one of the most common methods for:

* Unauthorized data exfiltration
* Insider threats
* Malware delivery
* Ransomware introduction
* Circumvention of enterprise security controls

This project demonstrates how Active Directory Group Policy can be used to centrally enforce removable media restrictions across domain-joined endpoints.

The solution uses Microsoft's built-in Removable Storage Access policies to block all removable storage classes without requiring third-party software.

---

# Lab Environment

| Component         | Description                            |
| ----------------- | -------------------------------------- |
| Domain            | saada.local                            |
| Domain Controller | Windows Server 2025                    |
| Client Machine    | Windows 10 Enterprise (Domain Joined)  |
| Management Tool   | Group Policy Management Console (GPMC) |

---

# Technologies Used

* Active Directory Domain Services (AD DS)
* Group Policy Management Console (GPMC)
* Windows Server 2025
* Windows 10 Enterprise
* Group Policy Objects (GPO)

---

Enterprise-USB-Device-Restriction-Using-Active-Directory-GPO
│
├── README.md
│
├── architecture
│   └── lab-diagram.png
│
├── before-policy
│   ├── usb-device-detected
│   │   └── 01-usb-device-detected.png
│   │
│   ├── removable-storage-accessible
│   │   └── 02-removable-storage-accessible.png
│   │
│   ├── file-copy-to-usb
│   │   ├── 03-copy-file-to-usb.png
│   │   └── 04-file-copied-successfully.png
│   │
│   └── file-opened-from-usb
│       ├── 05-open-file-from-usb.png
│       └── 06-file-opened-successfully.png
│
├── gpo-configuration
│   ├── gpo-creation
│   │   ├── 01-open-gpmc.png
│   │   ├── 02-create-usb-restriction-gpo.png
│   │   └── 03-edit-usb-restriction-gpo.png
│   │
│   ├── removable-storage-policy
│   │   ├── 04-removable-storage-access-node.png
│   │   ├── 05-deny-all-removable-storage-access.png
│   │   └── 06-policy-enabled.png
│   │
│   ├── policy-linking
│   │   ├── 07-link-gpo-to-saada-local.png
│   │   └── 08-gpo-linked-to-domain.png
│   │
│   ├── policy-deployment
│   │   ├── 09-gpupdate-force.png
│   │   └── 10-client-restart.png
│   │
│   └── gpresult-verification
│       └── 11-gpresult-generated.png
│
└── after-policy
    ├── usb-device-blocked
    │   └── 01-usb-device-blocked.png
    │
    ├── removable-storage-denied
    │   └── 02-removable-storage-access-denied.png
    │
    ├── access-denied-message
    │   └── 03-location-is-not-available.png
    │
    └── gpresult-verification
        └── 04-gpresult-policy-applied.png

# Security Control Implemented

| Control                        | Purpose                                     |
| ------------------------------ | ------------------------------------------- |
| USB Device Restriction         | Block removable storage devices             |
| Centralized Policy Enforcement | Apply restrictions through Active Directory |
| Endpoint Hardening             | Reduce attack surface                       |
| Data Loss Prevention (DLP)     | Prevent unauthorized data transfers         |
| Malware Prevention             | Reduce removable media malware risks        |

---

# Architecture Diagram

This lab simulates an enterprise environment where removable media restrictions are centrally managed through Active Directory Group Policy.

![Architecture Diagram](architecture/lab-diagram.png)

## Policy Flow

1. Administrator creates USB Restriction GPO
2. GPO is linked to the domain
3. Windows clients receive policy updates
4. Removable storage access is blocked
5. Users cannot access USB storage devices

---

# Scenario

## Before Policy Implementation

Users can:

* Insert USB devices
* Browse removable storage
* Copy files to USB devices
* Copy files from USB devices
* Access external storage devices freely

📸 See screenshots in:

```text
before-policy/
```

---

## After Policy Implementation

Users cannot:

* Access USB flash drives
* Open removable storage devices
* Browse USB contents
* Copy files to USB devices
* Copy files from USB devices

All removable storage devices are blocked through Group Policy.

📸 See screenshots in:

```text
after-policy/
```

---

# GPO Configuration

## Removable Storage Access Policy

Navigate to:

```text
Computer Configuration
 └ Policies
    └ Administrative Templates
       └ System
          └ Removable Storage Access
```

Configure:

```text
All Removable Storage classes: Deny all access
```

Set:

```text
Enabled
```

---

## Purpose

This policy:

* Blocks USB flash drives
* Blocks external hard drives
* Blocks external SSDs
* Blocks SD cards
* Blocks removable storage access
* Prevents unauthorized data transfers

---

# Implementation Steps

## 1. Create Group Policy Object

Open:

```text
Group Policy Management Console (GPMC)
```

Create:

```text
USB Device Restriction Policy
```

---

## 2. Configure Removable Storage Access Policy

Navigate to:

```text
Computer Configuration
 └ Policies
    └ Administrative Templates
       └ System
          └ Removable Storage Access
```

Open:

```text
All Removable Storage classes: Deny all access
```

Select:

```text
Enabled
```

Click:

```text
Apply
```

Then:

```text
OK
```

---

## 3. Link the GPO

Link the GPO to:

```text
saada.local
```

or the appropriate Organizational Unit (OU).

---

## 4. Update Group Policy

On the Domain Controller:

```cmd
gpupdate /force
```

On the Windows 10 client:

```cmd
gpupdate /force
```

Restart the client:

```cmd
shutdown /r /t 0
```

---

# Security Validation Testing

## Test 1 – USB Device Detection Before Policy

Expected:

```text
USB device accessible
```

Result:

```text
Successful
```

Screenshot:

```text
before-policy/usb-device-detected/01-usb-device-detected.png
```

---

## Test 2 – Browse USB Contents Before Policy

Expected:

```text
User can access removable storage
```

Result:

```text
Successful
```

Screenshot:

```text
before-policy/removable-storage-accessible/02-removable-storage-accessible.png
```

---

## Test 3 – Copy File to USB Before Policy

Expected:

```text
File copied successfully
```

Result:

```text
Successful
```

Screenshot:

```text
before-policy/file-copy-to-usb/03-copy-file-to-usb.png
```

---

## Test 4 – USB Access After Policy

Expected:

```text
Access denied
```

Result:

```text
Successful enforcement
```

Screenshot:

```text
after-policy/removable-storage-denied/02-removable-storage-access-denied.png
```

---

## Test 5 – Group Policy Verification

Verification Method:

```cmd
gpresult /h report.html
```

Expected:

```text
USB Restriction Policy Applied
```

Result:

```text
Successful
```

Screenshot:

```text
after-policy/gpresult-verification/04-gpresult-policy-applied.png
```

---

# Screenshots

## Before Policy

* USB Device Detected
* Removable Storage Accessible
* File Copy to USB
* File Copy from USB
* USB Files Open Successfully

---

## GPO Configuration

* Open GPMC
* Create USB Restriction GPO
* Configure Removable Storage Access Policy
* Enable Deny All Access
* Link GPO to Domain
* Group Policy Update

---

## After Policy

* USB Access Blocked
* Removable Storage Denied
* Access Denied Message
* Group Policy Verification

---

# Security Impact

This implementation:

* Prevents unauthorized removable media usage
* Reduces insider threat data exfiltration risks
* Blocks malware introduction through USB devices
* Supports Data Loss Prevention (DLP) objectives
* Enforces centralized endpoint security controls
* Improves endpoint security posture
* Standardizes removable media governance

---

# Protection Against Data Exfiltration

By blocking removable storage devices, organizations can significantly reduce the risk of sensitive information being copied to unauthorized external media.

This control supports enterprise Data Loss Prevention (DLP) initiatives and helps protect confidential information.

---

# Security Outcomes

This implementation successfully:

* Blocked removable storage devices
* Reduced data exfiltration opportunities
* Reduced malware delivery vectors
* Strengthened endpoint security
* Improved enterprise security governance
* Demonstrated centralized policy enforcement
* Supported DLP objectives

---

# Compliance & Data Loss Prevention (DLP) Impact

This project supports Data Loss Prevention (DLP) by restricting removable storage devices commonly used for unauthorized data transfers.

## Compliance Framework Alignment

| Framework   | Alignment                              |
| ----------- | -------------------------------------- |
| ISO 27001   | Media handling controls                |
| NIST 800-53 | MP-7, AC-19                            |
| GDPR        | Data protection and confidentiality    |
| HIPAA       | Device and media controls              |
| PCI DSS     | Endpoint protection and access control |

---

# Security Principles Demonstrated

* Least Privilege
* Defense in Depth
* Centralized Policy Enforcement
* Endpoint Hardening
* Data Loss Prevention (DLP)
* Security Governance

---

# Key Learning Outcomes

* Active Directory Administration
* Group Policy Management
* Endpoint Security Controls
* Data Loss Prevention Concepts
* Windows Security Hardening
* Enterprise Security Operations
* Security Baseline Development

---

# Future Improvements

* AppLocker Application Whitelisting
* BitLocker Deployment via GPO
* Microsoft Defender Hardening
* Windows Firewall Management
* LAPS Implementation
* USB Device Auditing
* Security Event Monitoring
* SIEM Integration

---

# Author

**Abdirahman Ali**

GitHub: https://github.com/abdira

LinkedIn: https://www.linkedin.com/in/abdirahman-ali-5b8a2b3b4/

---

# Project Value

This project demonstrates practical enterprise-level experience with:

* Active Directory
* Group Policy Management
* Endpoint Hardening
* Data Loss Prevention
* Security Governance
* Enterprise Security Operations

making it highly relevant for System Administrator, SOC Analyst, Blue Team, and Cybersecurity Engineering roles.

