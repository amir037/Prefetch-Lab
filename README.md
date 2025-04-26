# 🕵️‍♂️🔥 DFIR Lab Project: Windows Prefetch Analysis

Welcome to my Digital Forensics and Incident Response (DFIR) project, focusing specifically on Windows Prefetch file analysis. This project demonstrates techniques to analyze Prefetch files, which are critical forensic artifacts useful in uncovering evidence related to program execution on Windows systems.

---

## 📖 Project Overview

In this DFIR lab, I investigate a simulated scenario involving suspicious program execution. Windows Prefetch files (`.pf`) will be analyzed to determine:

- Executed programs
- Frequency and times of execution
- Files and directories accessed during program execution

The analysis is performed in a Windows VM hosted on Vultr.

---

## 🛠️ Lab Environment Setup

### Step 1: Hosting Windows VM

- Provision a Windows Server VM on Vultr.
- Access VM via Remote Desktop Protocol (RDP).

### Step 2: Preparing the Analysis Environment

- Install essential forensic analysis tools:
  - `.NET 6`
  - `PECmd` (Prefetch Parser)
  - `Timeline Explorer`

### Step 3: Preparing Evidence

- Prefetch files collected and stored at:

```
C:\Cases\Prefetch\
```

---

## 🔍 Analysis Procedure

### Extract Prefetch Information

- Use `PECmd` to extract details from `.pf` files:

```powershell
C:\Tools\PECmd.exe -q -d C:\Cases\Prefetch\ --csv \"C:\Cases\Analysis\" --csvf prefetch.csv
```

This command generates:
- `prefetch.csv`: Detailed artifact data
- `prefetch_Timeline.csv`: Timeline of executable usage

### Visual Analysis with Timeline Explorer

- Launch **Timeline Explorer** and load:
  - `prefetch.csv`
  - `prefetch_Timeline.csv`

- Analyze the timeline to identify unusual or malicious activities.

---

## 📌 Key Findings

*Include your summarized findings here.*

- Identified suspicious executables
- Frequency of suspicious executable runs
- Evidence supporting potential malware execution

---


## 🧠 Learning Outcomes

Through this project, I gained practical experience in:

- Identifying and interpreting critical forensic artifacts
- Using specialized forensic tools
- Conducting detailed timeline analysis for DFIR investigations

---


