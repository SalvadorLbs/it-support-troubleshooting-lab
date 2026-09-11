# IT Support Troubleshooting Lab

A hands-on IT support lab built to develop practical 1st and 2nd line support skills through realistic incidents and troubleshooting scenarios.

The environment combines Windows 11, Microsoft Entra ID, Microsoft 365, Exchange Online, Microsoft Intune and PowerShell. Rather than following theory-only exercises, faults were introduced and investigated using a structured troubleshooting process.

## Technologies Used

- Windows 11 Pro
- Microsoft Entra ID
- Microsoft 365
- Exchange Online
- Microsoft Intune
- PowerShell
- Windows Command Prompt
- VirtualBox
- TCP/IP and DNS
- Windows Firewall
- Windows Services

## Skills Demonstrated

- User account and access administration
- Authentication and sign-in troubleshooting
- Microsoft 365 licensing troubleshooting
- Group membership and dynamic groups
- Role-based access and least privilege
- User onboarding and offboarding
- Shared mailbox administration
- Full Access, Send As and Send on Behalf permissions
- Windows endpoint troubleshooting
- TCP/IP and DNS troubleshooting
- TCP port connectivity testing
- Windows Firewall troubleshooting
- Windows service management
- Microsoft Entra device join
- Intune and MDM enrolment troubleshooting
- PowerShell administration
- Incident investigation and documentation

## Troubleshooting Approach

The labs followed a structured support process:

1. Confirm the reported symptoms
2. Gather evidence
3. Identify the affected layer or service
4. Form a hypothesis
5. Make the smallest appropriate change
6. Verify the technical fix
7. Confirm the original issue is resolved
8. Document the incident

## Lab Areas

### Identity and Access

Troubleshot user sign-in failures, disabled accounts, licensing issues, group membership, dynamic groups, password resets, session revocation and role assignments using Microsoft Entra ID.

### Microsoft 365 and Exchange Online

Configured and troubleshot shared mailbox access, Full Access, Send As and Send on Behalf permissions, permission propagation and Microsoft 365 service licensing.

### Windows Endpoint Support

Built a Windows 11 virtual workstation and performed practical endpoint troubleshooting using PowerShell and Windows diagnostic tools.

Fault scenarios included incorrect DNS configuration, a stopped Print Spooler service, blocked outbound HTTPS traffic and a disabled network adapter.

### Intune and MDM

Microsoft Entra joined the Windows 11 workstation and investigated MDM enrolment. Troubleshooting included MDM user scope, device registration status, MDM discovery endpoints and Windows MDM event logs.

## Example Troubleshooting Scenarios

Detailed incident documentation in this repository demonstrates the investigation, root cause, resolution and verification process for selected support scenarios.

These include:

- Microsoft 365 licensing and access
- User account and sign-in failures
- User offboarding and least privilege
- Exchange Online shared mailbox permissions
- Intune and MDM enrolment
- DNS resolution failure
- Windows Print Spooler failure
- HTTPS connectivity blocked by Windows Firewall
- Disabled Windows network adapter

## Environment

The endpoint environment was built using Windows 11 Pro running in VirtualBox and integrated with a Microsoft cloud lab tenant.

All incidents were performed in a controlled lab environment using test accounts and deliberately introduced faults.

## Security and Privacy

Screenshots and documentation published in this repository are sanitised to remove tenant identifiers, account information and other environment-specific data.
