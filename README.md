
# Enterprise User Lifecycle Administration (AD)(RBAC)

**Ironbridge Bank & Trust | MRA Finding #1 | Simulated enterprise environment**

---

## Video Walkthrough

**[▶ Watch the 3 to 7 minute walkthrough][(https://www.loom.com/share/fad1b7f484514db39cb6f4037b275dbf)]**

---

## Problem Statement

Ironbridge Bank & Trust is a 160 employee community bank. IT was outsourced to an MSP called VelocIT for five years, and after a regulatory examination the bank was issued a Matters Requiring Attention letter. Finding #1 was the directory.

What I inherited: every account in one flat container with no organizational units, no naming standard, passwords set to never expire, a terminated employee still enabled fourteen months after she left, and a former contractor still holding Domain Admin four years after he left the state.

The bank could not answer the examiners' question: who has access, and why.

The first thing I noticed when I opened the directory was how difficult it was to tell who should have access and why. Active employees, terminated users, service accounts, and unknown accounts were mixed with no clear structure, which showed me that access had not been consistently managed as people joined, changed roles, or left the bank. Once I compared Active Directory against the roster, I was able to see the impact more clearly. Some accounts had no matching HR record, a terminated employee was still enabled months after they were offboarded, and a privileged access existed without a clear business justification.

---

## Solution Overview

I reconciled Active Directory against the HR roster to establish who actually works at the bank, then classified every inherited account as keep, disable, leaver or service.

I designed and built an OU structure by department and branch, created role based security groups from a documented role catalog, and applied a naming standard to every object.

I migrated every surviving account with PowerShell, populating required attributes, granting access through role groups only, and clearing the non-expiring password flag across the bank. I then ran the full identity lifecycle against real tickets: a joiner, a mover, and two leavers.

---

## Key Work

**Remediated the examination finding.** Disabled the terminated account rather than deleting it, stripped its group memberships, reset the credential, and moved it to a Disabled Users OU with the termination documented on the object.

**Removed an unowned privileged account.** Processed it through a leaver script, dry run first, then live. Removed it from Domain Admins and preserved the account and logs as evidence.

**Answered the Domain Admin question.** Reviewed privileged membership recursively rather than by direct members, because a nested group grants admin rights to everyone inside it.

**Escalated rather than fixed.** Found a separation of duties conflict in the wire transfer workflow, where one person could both initiate and approve a transfer. Funds transfer access is owned by the wire system owner, so I documented it in an exception register and escalated it instead of changing it myself.

**Caught privilege creep in progress.** Processed a role change by adding the new role exactly as requested. The old role was still attached, so I removed it. Requests tell you what to add and never what to remove.

The step I would do differently next time is validate each PowerShell change in Active Directory before moving on to the next task. During the project, there was a script that stated an account was disabled, but during verification I noticed it was still enabled. Verifying the change immediately would have allowed me to catch the issue earlier and prevented me from assuming the change had been successfully completed.

---

## Tools Used

Windows Server, Active Directory Domain Services, PowerShell, Group Policy Management, VirtualBox or UTM.

---

## Repository Contents

- `Work/IB-STD-001-Directory-Standard.md` - the directory standard I authored
- `Work/scripts/` - PowerShell for recon, migration, cleanup and evidence
- `Runbook/` - the joiner, mover, leaver SOP this work followed
- `Evidence/` - before and after states, reports, exception register, audit log
- `Logs/` - dry run and live logs for every scripted change
- `screenshots/` - visual evidence at each phase

---

## What I Learned

**Disable, never delete.**
Every account has a hidden identifier called a SID (Security Identifier), and file permissions, mailbox rights and audit log entries all point at that SID rather than to the name. Delete the account and the SID goes with it. This breaks the connection to anything (documents and files) tied to the identity. If you attempt to recreate an account with the same name, a new SID is granted, so the original permissions and access do not carry over.

**Access belongs on groups, not people.**
A permission granted directly to a user works, but it never shows up in a group membership report, which means the bank can't answer who has access and why. Assigning access through role-based groups creates a connection between a user's job role and the allowed permissions.

**A mover is two operations.**
The request asked me to give her loan processing access. It never mentioned removing her teller access, and if I had only added the new loan processing access, she would still have access to her teller role, which is not needed anymore. This is privilege creep and violates the principle of least privilege. A role change means adding the access needed for the new role and removing access that is no longer needed.

**Identity decisions need an authoritative source.**
Comparing Active Directory against the HR roster showed me why I should not decide whether an account belongs only based on what I see. Looking at both together helped me identify which accounts should remain active, go through the leaver process, or be disabled.


---

*Built as part of the TotalThreat IT & IAM Career Accelerator. Ironbridge Bank & Trust is a simulated enterprise environment and not a real employer. The starting directory state and the scripts in `Tools/` were provided by the program. The investigation, directory standard, scripts, remediation and documentation are my own work.*

