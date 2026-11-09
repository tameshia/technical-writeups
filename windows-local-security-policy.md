# Implementing and Validating Windows Local Security Policies

## Objective

The goal of this lab was to configure Windows local security controls, test their visible effects, and interpret the resulting Security log evidence. I focused on connecting policy settings to observed behavior while keeping confirmed results separate from expected outcomes and unconfirmed event correlation.

## Tools Used

- Local Users and Groups and Computer Management
- Local Security Policy
- File Properties and advanced auditing settings
- Event Viewer and Windows Security logs
- Notepad

## Environment

I completed the exercise in a dedicated Windows 10 training virtual machine using an administrator account and two local standard-user accounts, `secpol` and `secpol2`. These settings were used for controlled testing, including intentionally restricted rights and a single incorrect-password attempt.

## Configuration and Validation

I checked Users group membership and added `secpol2` to **Deny log on locally**. This documented the deny assignment; the evidence does not independently confirm a rejected sign-in for that account. The expected restriction should therefore remain distinct from a demonstrated test result.

For **Shut down the system**, I removed Users, leaving Administrators and Backup Operators. After signing in as `secpol`, Windows displayed **“There are currently no power options available.”** This confirmed the observed Start-menu restriction for the tested standard account, without establishing identical behavior for accounts with different rights.

I also configured the interactive logon title **WARNING** and message **You are being monitored**. The notice appeared before sign-in, confirming that the banner settings applied. The notice communicated expectations; account rights provided the access restrictions.

Advanced auditing covered **Audit Logoff — Success**, **Audit Logon — Failure**, and **Audit File System — Success and Failure**. On the administrator desktop file `secpoltest.txt`, I added a System Access Control List (SACL) auditing entry for Everyone, type All, selecting Read & execute, Read, and Write. The audit policy enabled the category, while the SACL selected qualifying file activity. This auditing entry was separate from the discretionary access permissions that determine access. Everyone was deliberately broad for the lab.

## Key Observations

### Event 4663: Notepad Writes and Background Reads

I opened `secpoltest.txt` in Notepad, typed `test`, and saved it. The reviewed **4663** record at **1:21:52 PM** identified **Tameshia**, the target file, `notepad.exe`, **WriteData/AppendData**, and access mask `0x6`. Those fields supported identifying the recorded access as part of the deliberate Notepad write test.

A separate 4663 showed a background process reading the same file. Sharing an event number and object name did not make the activities equivalent. Comparing **Process Name**, **Object Name**, and **Accesses** distinguished the write from the read. The read record alone did not establish another user edit or malicious activity.

### Events 4647 and 4634: Confirmed Logoff, Unconfirmed Correlation

Event **4647** showed `secpol` initiating logoff at **11:56:41 AM**, with Logon ID `0x1914C84`. A repeated test also recorded a user-initiated logoff at **12:42:12 PM**.

I investigated **4634**, which records that an account was logged off, for a corresponding session. I did **not** confirm a matching `secpol` 4634 in the records reviewed. Nearby UMFD or DWM events had different logon identifiers and were not accepted as matches.

The confirmed finding was the user-initiated 4647 event. A linked 4634 remained unconfirmed. This does not prove Windows never generated one; it describes the limits of the reviewed evidence. Timestamps alone were insufficient to connect sessions.

### Event 4625: Controlled Failed Authentication

I intentionally entered an incorrect administrator password once, then signed in correctly. Event **4625** at **12:42:28 PM** recorded an Audit Failure for **Tameshia**, **Logon Type 2** (interactive), status `0xC000006D`, and substatus `0xC000006A`, indicating an incorrect password.

The source address `127.0.0.1` was loopback, not a separate remote host. The event supported the controlled failed-password test; a single failure did not establish brute force or independently identify the person entering the credentials.

## What I Learned

This lab reinforced the distinction between configuring a setting, testing its effect, and interpreting its audit trail. Group membership, user rights, access permissions, and auditing entries serve different purposes. Event IDs helped locate records, but accounts, processes, access types, status codes, and session identifiers determined what conclusions the evidence supported.

## Why It Matters

Systems administration requires both effective controls and accurate documentation. Demonstrating a visible restriction or recorded file access is useful; documenting an unresolved session correlation is equally important. This approach supports troubleshooting without overstating what a log record proves.

## Related Hands-On Project

The [Windows Local Security Policy lab](https://github.com/tameshia/it-systems-administration/tree/main/windows-local-security-policy) contains the full procedure, investigation notes, and **15 screenshot files**. This write-up summarizes the configuration and findings without duplicating the walkthrough or screenshots.
