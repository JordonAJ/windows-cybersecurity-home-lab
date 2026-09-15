# windows-cybersecurity-home-lab
A hands-on Windows 11 cybersecurity home lab focused on user management, access control, security monitoring, and incident investigation.

## Lab Environment

- Oracle VirtualBox
- Windows 11 Pro
- Local administrator account: Labadmin
- Standard user account: Employee01

## Lab 1: User Accounts & Least Privilege

### Objective

The objective of this lab was to practice Windows user account management, privilege separation, and file access control using the principle of least privilege.

### Tasks Completed

- Created Employee01 as a standard Windows user.
- Verified the account's membership in the Users group.
- Tested User Account Control (UAC) and confirmed administrator credentials were required for privilege elevation.
- Created a simulated confidential company folder.
- Examined the folder's Access Control List (ACL).
- Disabled inherited permissions.
- Removed access for the Users and Authenticated Users groups.
- Verified that Employee01 could not access the confidential folder.

### UAC Privilege Elevation Test

Employee01 was unable to perform an administrative operation without providing administrator credentials.

![UAC requiring administrator credentials](01-uac-admin-credentials-required.png)

### NTFS Permission Hardening

Permissions on the confidential folder were restricted to Administrators and SYSTEM.

![Hardened NTFS permissions](02-ntfs-permissions-hardened.png)

### Access Control Verification

After changing the NTFS permissions, I logged into Employee01 and attempted to access the confidential folder. Windows denied access, confirming that the permissions were working as intended.

![Employee01 access denied](03-confidential-folder-access-denied.png)

## Skills Practiced

- Windows user and group management
- User Account Control (UAC)
- Principle of least privilege
- NTFS file and folder permissions
- Access Control Lists (ACLs)
- Authentication and authorization
- Security control testing and verification
