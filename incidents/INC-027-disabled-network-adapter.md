# INC-027: Disabled Network Adapter

## Scenario

A Windows 11 user reports a complete loss of network connectivity.

The objective was to determine whether the issue was caused by the network adapter, IP configuration, routing, DNS or another network component.

## Investigation

The network adapters and their current states were inspected using PowerShell:

```powershell
Get-NetAdapter -IncludeHidden
```

The Ethernet adapter showed:

```text
Ethernet    Intel(R) PRO/1000 MT Desktop Adapter    Disabled
```

This identified that the network interface itself was disabled.

Because the adapter was not operational, troubleshooting higher-level components such as DNS or application connectivity would not yet be appropriate.

## Root Cause

The Windows Ethernet network adapter had been disabled.

Without an active network interface, the workstation could not establish normal network connectivity.

## Resolution

The Ethernet adapter was enabled using PowerShell:

```powershell
Enable-NetAdapter -Name "Ethernet"
```

## Verification

The workstation's IP configuration was checked:

```powershell
ipconfig
```

The Ethernet adapter had obtained an IPv4 address and default gateway.

External IP connectivity was then tested:

```powershell
ping 8.8.8.8
```

The test succeeded.

DNS resolution was also verified:

```powershell
nslookup google.com
```

The hostname resolved successfully, confirming that network connectivity and DNS resolution had been restored.

## Key Learning

Troubleshooting should begin at the lowest relevant layer.

Checking the network adapter state early can prevent unnecessary investigation of DNS, firewall or application issues when the network interface itself is unavailable.

The incident also reinforced PowerShell's Verb-Noun command structure:

```text
Get-NetAdapter
Disable-NetAdapter
Enable-NetAdapter
```

A useful endpoint troubleshooting sequence is:

**Network adapter → IP configuration → default gateway → external connectivity → DNS → TCP port → application**
