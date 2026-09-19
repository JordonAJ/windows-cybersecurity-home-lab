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

![UAC requiring administrator credentials](screenshots/lab1/01-uac-admin-credentials-required.png)

### NTFS Permission Hardening

Permissions on the confidential folder were restricted to Administrators and SYSTEM.

![Hardened NTFS permissions](screenshots/lab1/02-ntfs-permissions-hardened.png)

### Access Control Verification

After changing the NTFS permissions, I logged into Employee01 and attempted to access the confidential folder. Windows denied access, confirming that the permissions were working as intended.

![Employee01 access denied](screenshots/lab1/03-confidential-folder-access-denied.png)

## Skills Practiced

- Windows user and group management
- User Account Control (UAC)
- Principle of least privilege
- NTFS file and folder permissions
- Access Control Lists (ACLs)
- Authentication and authorization
- Security control testing and verification


## Lab 2: Windows Event Viewer & Security Log Analysis

## Objective

The objective of this lab was to learn how to use Windows Event Viewer to identify, investigate, and document authentication activity. I simulated multiple failed login attempts and analyzed the resulting Windows Security events.

## Tasks Completed

- Navigated Windows Security logs using Event Viewer.
- Filtered security logs using Windows Event IDs.
- Identified Event ID 4624 for successful logons.
- Identified Event ID 4625 for failed logon attempts.
- Generated three controlled failed login attempts against Employee01.
- Investigated the account associated with the failed authentication attempts.
- Examined the failure reason and source information.
- Correlated multiple failed attempts with a subsequent successful login.
- Created a custom Event Viewer view for monitoring failed login attempts.

## Failed Login Investigation

Three incorrect passwords were intentionally entered for the Employee01 account to simulate repeated failed authentication attempts.

Event Viewer recorded these attempts as Event ID 4625.

### Account and Failure Information

The event identified Employee01 as the account associated with the failed logon attempt. The failure reason reported an unknown user name or bad password.

![Failed logon account and reason](01-failed-logon-account-and-reason.png)

### Source Information

The failed logon event showed the workstation as CYBER-LAB-01 and the source network address as 127.0.0.1, indicating that the recorded source was the local system.

![Failed logon source information](02-failed-logon-source-information.png)

## Multiple Failed Login Attempts

Filtering the Security log for Event ID 4625 revealed three failed login attempts within several seconds.

![Multiple failed logon attempts](03-multiple-failed-logon-attempts.png)

## Incident Timeline

| Time | Event ID | Result |
|------|----------|--------|
| 3:04:40 PM | 4625 | Failed logon |
| 3:04:42 PM | 4625 | Failed logon |
| 3:04:45 PM | 4625 | Failed logon |
| 3:04:50 PM | 4624 | Successful logon |

After three failed authentication attempts, Employee01 successfully logged into the system.

## Successful Logon Verification

Event ID 4624 confirmed that Employee01 successfully authenticated at 3:04:50 PM.

![Successful logon](04-successful-logon-after-failures.png)

## Failed Login Monitoring

I created a custom Event Viewer view called **Failed Login Attempts** that displays Event ID 4625 from the Windows Security log. This provides a reusable method for reviewing failed authentication attempts without manually filtering the Security log each time.

![Failed login custom view](05-failed-login-custom-view.png)

## Findings

The investigation identified three failed interactive authentication attempts against Employee01 followed by a successful authentication. Because these events were intentionally generated as part of the lab, they represented a controlled situation rather than an actual security incident.

This exercise showed how Windows Security logs can be used to identify authentication failures, correlate related events, and construct a basic incident timeline.

## Skills Practiced

- Windows Event Viewer
- Windows Security logs
- Security event filtering
- Event ID 4624 analysis
- Event ID 4625 analysis
- Failed login investigation
- Log correlation
- Incident timeline creation
- Basic security monitoring
- Authentication log analysis

# Lab 3: Microsoft Defender & Endpoint Security

## Objective

The objective of this lab was to explore Microsoft Defender, configure Windows endpoint security features, safely simulate a malware detection, investigate the alert, remediate the detected threat, and verify that the system was clean.

## Defender Security Configuration

I reviewed Microsoft Defender Antivirus settings and verified that important security features were enabled, including:

- Real-time protection
- Cloud-delivered protection
- Automatic sample submission
- Tamper Protection

![Microsoft Defender settings](lab3-01-defender-settings.png)

## Safe Malware Detection Test

I used the EICAR antivirus test file to safely test Microsoft Defender's detection capabilities. EICAR is a harmless test file designed to trigger antivirus software without using real malware.

Microsoft Defender successfully identified the file as:

**Virus:DOS/EICAR_Test_File**

The detection was classified as Severe.

![EICAR threat detected](lab3-02-eicar-threat-detected.png)

## Threat Investigation

I investigated the detection using Windows Security Protection History.

The investigation identified:

- Threat: Virus:DOS/EICAR_Test_File
- Severity: Severe
- Initial status: Active
- Affected file: C:\Users\Labadmin\Documents\eicar.com

![EICAR Protection History](lab3-03-eicar-protection-history.png)

## Threat Remediation

I quarantined the detected test file using Microsoft Defender. Protection History confirmed that the threat status changed to Quarantined and the file was no longer available from its original location.

![EICAR threat quarantined](lab3-04-eicar-quarantined.png)

## Potentially Unwanted Application Protection

I reviewed reputation-based protection settings and found that Potentially Unwanted Application (PUA) blocking was disabled.

I enabled:

- Potentially unwanted app blocking
- Block apps
- Block downloads

This provides additional protection against low-reputation or unwanted software that may not necessarily be classified as traditional malware.

![PUA protection enabled](lab3-05-pua-protection-enabled.png)

## Memory Integrity Troubleshooting

I attempted to enable Windows Memory Integrity as an additional system security control. Windows reported that an incompatible driver prevented the feature from being enabled.

I investigated the issue and identified:

**Driver:** E1G6032E.sys  
**Device:** Intel(R) PRO/1000 MT Desktop Adapter

I verified that the adapter was using a Microsoft-provided driver and checked for an updated driver. Windows reported that the best available driver was already installed.

Because the adapter provides network connectivity to the virtual machine, I did not remove the driver simply to enable Memory Integrity. The compatibility issue was documented for further investigation.

![Memory Integrity incompatible driver](lab3-06-memory-integrity-driver.png)

## Final Security Scan

After completing the security configuration and EICAR detection test, I performed a Microsoft Defender Full Scan.

Results:

- 228,299 files scanned
- 0 threats found
- No current threats

![Final Defender full scan](lab3-07-full-scan-clean.png)

## Skills Practiced

- Microsoft Defender Antivirus
- Endpoint security
- Real-time malware protection
- Antivirus scanning
- Threat detection and investigation
- Protection History analysis
- Threat quarantine and remediation
- Potentially Unwanted Application (PUA) protection
- Windows reputation-based protection
- Core isolation and Memory Integrity
- Driver compatibility troubleshooting
- Security verification
