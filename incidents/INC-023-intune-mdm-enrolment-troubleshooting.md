# INC-023: Intune and MDM Enrolment Troubleshooting

## Scenario

A Windows 11 Pro virtual workstation was connected to Microsoft Entra ID as part of an endpoint-management lab.

The device was successfully Entra joined, but it was not appearing in the Microsoft Intune Windows device inventory.

The objective was to determine why the device was registered with Microsoft Entra ID but was not being confirmed as an Intune-managed Windows device.

## Investigation

The Windows device registration state was checked using:

```powershell
dsregcmd /status
```

The results confirmed:

```text
AzureAdJoined : YES
DeviceAuthStatus : SUCCESS
```

This verified that the workstation was successfully joined to Microsoft Entra ID.

However, Entra joining a device does not automatically prove that the device is enrolled and managed by Microsoft Intune.

The Intune device inventory was checked and the Windows device was not present.

The Microsoft 365 licence assigned to the test administrator was reviewed and confirmed to include the required Intune services.

The Intune automatic enrolment configuration was then investigated.

The MDM user scope was found to be:

```text
None
```

With the MDM user scope set to None, users were not in scope for automatic MDM enrolment.

## MDM Scope Configuration

The MDM user scope was changed from:

```text
None
```

to a scoped configuration.

During further investigation, the difference between the available MDM user scopes was reviewed:

```text
None  → No users are automatically enrolled
Some  → Only users in the selected groups are in scope
All   → All eligible users are in scope
```

This demonstrated that selecting **Some** requires the correct user to be included in the security group targeted by the MDM scope.

## Windows-Side Verification

After changing the MDM scope, the Windows registration information was checked again:

```powershell
dsregcmd /status | Select-String "Mdm"
```

MDM discovery information was now populated, including the MDM enrolment endpoints.

Additional Windows-side investigation included:

```powershell
deviceenroller.exe /c /AutoEnrollMDM
```

and reviewing the Device Management Enterprise Diagnostics event log:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-DeviceManagement-Enterprise-Diagnostics-Provider/Admin" -MaxEvents 20
```

Windows generated MDM PolicyManager activity, providing evidence that the operating system was processing MDM-related configuration.

## Findings

The investigation demonstrated several different states that must not be treated as equivalent:

```text
Microsoft Entra joined
MDM discovery configured
MDM enrolment activity
Intune device inventory confirmation
```

A device can be successfully joined to Microsoft Entra ID without that alone proving that it is fully enrolled and visible as an Intune-managed device.

The initial MDM automatic enrolment scope of **None** prevented automatic enrolment.

Further investigation also demonstrated the importance of correctly targeting users when the scope is configured as **Some**.

## Verification Status

Microsoft Entra join was successfully verified from Windows.

MDM discovery endpoints and Windows MDM PolicyManager activity were also observed after the enrolment scope was changed.

However, the device was not confirmed in the Intune Windows device inventory during the lab.

For this reason, the lab does not claim successful end-to-end Intune device management.

## Key Learning

Microsoft Entra device registration and Microsoft Intune device management are related but separate processes.

Troubleshooting should verify each stage independently rather than assuming that an Entra-joined device is automatically Intune managed.

A useful troubleshooting sequence is:

**Check licence → verify Entra join → check MDM user scope → verify group targeting → inspect MDM discovery information → review Windows MDM events → check Intune device inventory**

The incident also reinforced the importance of final verification.

Configuration changes and Windows-side MDM activity are useful evidence, but successful endpoint management should ultimately be confirmed from the management platform.
