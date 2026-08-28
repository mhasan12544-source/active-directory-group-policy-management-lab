# Active Directory Group Policy Management Lab

## Executive Summary
This repository documents hands-on Group Policy Objects (GPO) configuration and enforcement across domain endpoints. Each policy scenario was implemented and tested using both **Group Policy Management Console (GPMC)** and **PowerShell**, with ticket management tracked in Jira.

* **Full Step-by-Step Walkthrough:** [Download Complete PDF Documentation](./Project%203_%20Active%20Directory%20Group%20Policy%20Management_compressed.pdf)

---

## Technical Stack & Environment
* **Cloud Infrastructure:** Amazon Web Services (AWS EC2)
* **Directory Services:** Active Directory Domain Services (AD DS) on Windows Server 2022
* **Policy Management:** Group Policy Management Console (GPMC)
* **Scripting & Automation:** PowerShell (`GroupPolicy` module)
* **ITSM / Ticketing:** Jira Service Management

---

## Simulated Ticket Matrix

| Ticket ID | Issue / Request | Primary Method (GUI) | Automation Method (PowerShell) | Status |
| :--- | :--- | :--- | :--- | :--- |
| **GPO-001** | Workstation Lock Policy (60s Timeout) | GPMC Console | `New-GPO` / `Set-GPRegistryValue` | Resolved |
| **GPO-002** | Configure Password Complexity Policy | GPMC / Secpol | PowerShell Script | Resolved |
| **GPO-003** | Deploy Desktop Wallpaper Policy | GPMC Preferences | PowerShell Script | Resolved |
| **GPO-004** | Restrict Control Panel Access | GPMC Administrative Templates | PowerShell Script | Resolved |

---

## Ticket Execution Workflow Example

### Featured Ticket GPO-001: Workstation Lock Policy Implementation

#### Method 1: Manual GUI Execution
1. **Scenario & Intake:** Reviewed compliance ticket request in Jira to enforce a 60-second workstation inactivity lock policy for users in `OU=Staff-OU,DC=mydclab,DC=local`.
2. **Step-by-Step Implementation:** Opened `gpmc.msc`, created and linked `Workstation_Lock_Policy` GPO to `Staff-OU`, set **Interactive logon: Machine inactivity limit** to `60` seconds, and executed `gpupdate /force`.
3. **Verification:** Verified automatic screen locking after 60 seconds on `psdclab-client1` logged in as `MYDCLAB\janed`.

#### Method 2: PowerShell Automation
1. **Script Development:** Used `New-GPO` and `New-GPLink` to create/target `Workstation_Lock_Policy`, and `Set-GPRegistryValue` to set `InactivityTimeoutSecs` to `60`.
2. **Execution & Audit:** Verified policy update via `gpupdate /force` and confirmed the local registry setting on the client using `Get-ItemProperty`.

#### Resolution Summaries

**Ticket Resolution Summary (GUI Method)**
* **Issue Description:** Enforce workstation inactivity lock policy (60s timeout) for Staff-OU users.
* **Actions Taken (GUI):** Created `Workstation_Lock_Policy` in GPMC, configured machine inactivity limit to 60 seconds, linked GPO to target OU, and verified automatic locking on client machine.
* **Resolution:** Policy successfully applied and verified on endpoint. Ticket closed in Jira.

**Ticket Resolution Summary (PowerShell Method)**
* **Issue Description:** Automated GPO deployment for 60-second workstation lock settings.
* **Actions Taken (PowerShell):** Executed PowerShell script to instantiate GPO, set `InactivityTimeoutSecs` registry value to 60, and link to target OU.
* **Resolution:** GPO programmatically deployed and verified via registry query (`InactivityTimeoutSecs : 60`). Ticket closed in Jira.
