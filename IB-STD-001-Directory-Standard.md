IB-STD-001: Directory Standard
1. Purpose and scope
The purpose of this standard is to define account management for Ironbridge Bank & Trust and solve examination finding MRA #1. A second purpose is to ensure the bank can instantly answer who has access to any resource and why. The scope of this standard covers the entire ironbridge.local domain.


2. OU design, and reasons for each level
Organizational Units (OUs) group accounts and resources in Active Directory. Each level exists for a specific reason: policy, delegation, or clarity.

Users (Clarity): The top-level container for all user accounts, subdivided by department to make management and viewing clear.

Executive: Clarity (separates senior leadership accounts for specific grouping and management).

Lending: Clarity (groups loan officers and lending staff).

Wire Operations: Policy (applies strict security policies and monitoring required for high-risk funds transfer functions).

Wealth Management: Clarity (groups wealth management personnel).

Operations: Clarity (groups general banking operations staff).

Compliance: Policy (applies auditing and compliance-specific group policies).

IT: Delegation (restricts IT administrative accounts and permissions so only authorized technical staff can manage them).

Retail Banking: Clarity (groups retail staff and contains branch sub-OUs).

Branch OUs (e.g., Branch 1, Branch 2): Clarity (organizes users by physical branch location).

Groups (Clarity): Contains all security and distribution groups used for resource permissions.

Computers (Clarity): Contains all workstation and server computer objects, categorized for group policy application.

Service Accounts (Delegation): Isolates non-human service accounts so that specific service permissions and password policies can be delegated and monitored securely.

Disabled Users (Policy): Holds all deactivated accounts to satisfy retention, audit, and security policies, ensuring leavers are never immediately purged.


3. Naming conventions
Consistent naming makes accounts easy to identify and audit.

Users: First initial followed by last name.

Example: John Smith becomes jsmith.

Service Accounts: Prefix svc- followed by the service or application name.

Example: svc-sql-prod.

Groups: Prefix ROLE- followed by the business function.

Example: ROLE-Wire-Operator.

Computers: Prefix WKS- followed by the site identifier and a 3-digit number.

Example: WKS-MAIN-001.


4. Collision rule, and a worked example
When two employees share the same first initial and last name, a collision occurs.

Rule: The username format becomes the first two letters of the first name followed by the last name.

Worked Example: If Jane Smith and John Smith both work at the bank, John Smith registered first as jsmith. When Jane Smith joins, her username uses the first two letters of her first name (ja) plus her last name (smith), resulting in jasmith.


5. Required attributes, and why each one matters
Every user account must populate specific Active Directory attributes to maintain accountability and security:

Display Name: Shows the employee's full legal name for clear identification in the Global Address List and logs.

User Principal Name (UPN): The user's logon name in an email format (e.g., jsmith@ironbridge.local), required for modern authentication and cloud integrations.

Description: States the employee's exact job title and department, providing immediate context for why an account exists during audits.

Department / Office: Links the user to their physical location and business unit for organizational clarity.

Manager: Establishes the reporting chain, which is critical for automated approval workflows and access recertifications.


6. Access model
Ironbridge Bank & Trust enforces a strict access control model to satisfy MRA #1:

Role Groups Only: Permissions to files, folders, and applications are assigned exclusively to ROLE- security groups.

One Role Per Account: Each user account is assigned only the role group(s) necessary for their specific job function.

No Direct Permissions: Direct assignment of permissions to individual user accounts is strictly prohibited. If an account has access, it must be traceable to a role group, answering who has access and why.


7. Role catalog summary, and who owns changes to it
Role Catalog Summary: The Role Catalog is the master inventory of all ROLE- groups in the domain. It documents every business role, the precise access rights granted by that role, and the designated business owner.

Ownership: Changes to the Role Catalog (such as creating a new role or modifying permissions) require formal approval from Risk and Compliance, with IT executing the technical implementation. Business department heads own the determination of who belongs in each role.


8. Account states and leaver handling
Active Directory accounts can exist in several states:

Enabled: Active and able to authenticate.

Disabled: Deactivated; authentication is blocked.

Locked: Temporarily blocked due to incorrect password attempts.

Expired: Automatically disabled based on a set calendar date (used primarily for temporary contractors).

Why Leavers are Disabled, Not Deleted: When an employee leaves the bank, their account is moved to the Disabled Users OU and disabled—never deleted. Deleting an account destroys historical audit trails, log associations, and file ownership data. Disabling preserves forensic integrity while blocking all access, directly solving audit requirements for historical traceability.


9. Privileged access rules and the exception register
Privileged Access Rules: Domain Administrator and other high-level accounts are strictly separated from daily standard user accounts. Privileged tasks require dedicated administrative accounts, multi-factor authentication, and session logging. Admins must not use privileged accounts for routine tasks like email or web browsing.

Exception Register: Any temporary or permanent deviation from these standards (such as an emergency direct permission or a legacy service account naming exception) must be logged in the Privileged Access Exception Register, reviewed monthly by Information Security, and approved by the Chief Risk Officer.


10. Review cadence and document owner
Review Cadence: This standard, along with all directory access lists and role mappings, must be reviewed and recertified on a quarterly basis.

Document Owner: The IT and Identity Specialist at Ironbridge Bank & Trust is the official document owner responsible for maintaining, updating, and enforcing IB-STD-001.