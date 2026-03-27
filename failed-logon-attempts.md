# Identifying Failed Logon Attempts on Windows Systems

## Objective
The goal of this lab was to identify failed logon attempts on a Windows system and understand how failed authentication events can be reviewed as part of security monitoring and troubleshooting.

## Tools Used
- Windows
- Event Viewer
- Security logs

## Environment
This lab was performed in a Windows environment using built-in system tools to review authentication-related security events.

## Steps Performed
1. Opened **Event Viewer**.
2. Navigated to **Windows Logs > Security**.
3. Reviewed security events related to logon activity.
4. Identified failed logon attempts by looking for relevant event details in the Security log.
5. Examined the event information to understand when the failed attempt occurred and what account was involved.

## Key Observations
- Windows records authentication activity in the **Security** log.
- Failed logon attempts provide useful information for troubleshooting and security monitoring.
- Reviewing event details can help identify patterns such as repeated failures, incorrect credentials, or suspicious login behavior.

## What I Learned
This lab helped me better understand how Windows tracks authentication activity and how failed logon attempts can be investigated using Event Viewer. It also reinforced the importance of log review as part of security monitoring and incident awareness.

## Why It Matters
Failed logon attempts can be an early sign of account misuse, password issues, or unauthorized access attempts. Knowing how to identify and review these events is a useful foundational skill in IT support, system administration, and cybersecurity.

## Screenshots
![Event Viewer Security Log](./images/failed-logon-security-log.png)
_Add screenshots of Event Viewer, the Security log, and relevant failed logon events here._
