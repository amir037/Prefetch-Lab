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
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/4002f842-9feb-4ce4-8e21-fad6bc5f0365">

---

## 🔍 Analysis Procedure

### Extract Prefetch Information

- Use `PECmd` to extract details from `.pf` files:

```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -q -d C:\Cases\Prefetch\ --csv "C:\Cases\Analysis\" --csvf prefetch.csv
```

This command generates:
- `prefetch.csv`: Detailed artifact data
- `prefetch_Timeline.csv`: Timeline of executable usage

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/70bf7b6a-a6b2-4c80-91be-cdccf6b0a3bf">


### Visual Analysis with Timeline Explorer

- Launch **Timeline Explorer** and load:
  - `prefetch.csv`
  - `prefetch_Timeline.csv`
 
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/00a627c4-77a8-4396-a30d-3eb6703e94cd">


- Analyze the timeline to identify unusual or malicious activities.
- We know Bill claims to have downloaded and executed a suspicious program called "Burpsuite". Search for the keyword `burp` in the global search bar and tag the related activities.

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/b2aa2069-998f-410e-883a-b512b80bf8d8">

- Looking at processes around the time of execution, we can see an executable named `7ZG.exe`, which might be benign and looks related to `7-Zip.` We will then examine all executions that occurred up to one hour after `burpsuite-pro-cracked.exe` was executed and tag any suspicious ones.

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/ce8238f7-997b-49cd-bdcb-84ebce2c5058">

- Although not all of the executables above are suspicious, a deeper investigation might reveal how our threat actor used them.

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


