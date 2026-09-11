# INC-024: DNS Resolution Failure

## Scenario

A Windows 11 user reports that internet access is not working correctly and websites are failing to load.

The objective was to troubleshoot the issue systematically rather than immediately changing network settings.

## Investigation

First, the network configuration was checked:

```powershell
ipconfig /all
```

The workstation had a valid IP configuration and default gateway.

External IP connectivity was then tested:

```powershell
ping 8.8.8.8
```

The test succeeded, confirming that the workstation could reach an external IP address.

DNS resolution was tested next:

```powershell
nslookup google.com
```

The DNS request timed out and showed an unexpected DNS server.

The configured DNS servers were then reviewed:

```powershell
Get-DnsClientServerAddress -InterfaceAlias "Ethernet"
```

This identified an incorrect manually configured DNS server on the Ethernet adapter.

## Root Cause

The Ethernet adapter had been configured with an incorrect static DNS server.

The workstation still had IP connectivity, but DNS name resolution was failing.

## Resolution

The manual DNS configuration was removed and the adapter was returned to automatic DNS assignment:

```powershell
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ResetServerAddresses
```

This allowed the workstation to obtain the appropriate DNS configuration automatically.

## Verification

DNS resolution was tested again:

```powershell
nslookup google.com
```

Name resolution succeeded.

HTTPS connectivity was then verified:

```powershell
Test-NetConnection google.com -Port 443
```

The result returned:

```text
TcpTestSucceeded : True
```

The original connectivity issue was resolved.

## Key Learning

Successful IP connectivity does not prove that DNS is functioning.

Testing connectivity in stages helped isolate the problem:

**IP configuration → gateway → external IP connectivity → DNS resolution → TCP port → application**

The smallest appropriate change was made rather than replacing the DNS server with an arbitrary public DNS server.
