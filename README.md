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

## 🤿 Deep-Diving into Interesting Prefetch Files

- We will then dig deeper using `PECmd`.
  ```powershell
  C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -k burpsuite -f C:\Cases\Prefetch\7ZG.EXE-D9AA3A0B.pf
  ```
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/37ab50a1-b1d3-4bd4-9b77-f1e0a0da937f">
  <br><br>
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/8301994a-f21d-4100-8142-9b0468b172e4">

- Examine `BURPSUITE-PRO-CRACKED.EXE-EF7051A8.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -f C:\Cases\Prefetch\BURPSUITE-PRO-CRACKED.EXE-EF7051A8.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/a1ac07ac-6b2c-4842-8943-87365942ce0a">

- Examine `B.EXE-B3590BF0.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -f C:\Cases\Prefetch\B.EXE-B3590BF0.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/e5f1e89b-9952-457f-a94b-d2689fc54ff6">
<br><br>
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/5f1926e9-fa96-4e2c-be89-22990152b2d5">

- Examine `C.EXE-C6AEC675.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -f C:\Cases\Prefetch\C.EXE-C6AEC675.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/aa45ddd9-bf8e-47c2-b592-598264c9f2b3">

- Examine `P.EXE-C2093F36.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -f C:\Cases\Prefetch\P.EXE-C2093F36.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/5026c5e8-27fd-4b55-9e6b-235a88cf76c7">

- Examine `POWERSHELL.EXE-022A1004.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -f C:\Cases\Prefetch\POWERSHELL.EXE-022A1004.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/36f3c111-3850-456a-98ea-943c60c38264">

- Examine `RCLONE.EXE-56772E5D.pf`
-- Let’s be sure to include keywords for newly learned interesting items
  ```powershell
  C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -k backup,xls,pdf,zip -f C:\Cases\Prefetch\RCLONE.EXE-56772E5D.pf
  ```
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/f4f64366-8024-4955-b62c-c9cf1a5a3d16">
  <br><br>
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/6b2b8f99-9f9d-4023-84c5-bfed47edc46f">
  
- Let's see what program referenced `lsass.dmp`
  
  <img width="1000" alt="image" src="https://github.com/user-attachments/assets/679d1b44-71cc-431c-b452-7197e04d1159">
- Examine `SD.EXE-A541D1D9.pf`
```powershell
C:\DFIR_Tools\ZimmermanTools\net6\PECmd.exe -k backup -f C:\Cases\Prefetch\SD.EXE-A541D1D9.pf
```
<img width="1000" alt="image" src="https://github.com/user-attachments/assets/f9214bdb-5125-420a-b323-560e6e32aed4">

---

## 📌 Timeline

| Timestamp                | Event                                                                                                                                                 |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **2024-02-12 19:19:48**  | Execution of `RCLONE.EXE` – data‑migration utility accessed multiple sensitive business documents                                                     |
| **2024-03-12 18:36:11**  | Download and execution of `BURPSUITE‑PRO‑CRACKED.EXE` – initial malware delivery via cracked Burp Suite                                               |
| **2024-03-12 18:55:00**  | Execution of `7ZG.EXE` – archive extraction, likely unpacking the Burp Suite payload                                                                  |
| **2024-03-12 19:02:37**  | Execution of `C.EXE` – suspicious temporary executable in `C:\Windows\Temp`                                                                           |
| **2024-03-12 19:26:00**  | Execution of `POWERSHELL.EXE` – high‑volume access of company files (e.g., backups, exports), indicating potential exfiltration                       |
| **2024-03-12 (various)** | Analysis of `B.EXE`, `C.EXE` and `RCLONE.EXE` – these TEMP‑directory executables manipulated files in `C:\Windows\Temp` and backup logs               |


---



