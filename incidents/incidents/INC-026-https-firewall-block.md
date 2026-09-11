# INC-026: HTTPS Connectivity Blocked by Windows Firewall

## Scenario

A Windows 11 user reports that internet connectivity appears to be available, but secure websites are not opening.

The objective was to determine whether the problem was related to general network connectivity, DNS, the application, or a specific network service.

## Investigation

External IP connectivity was tested first:

```powershell
ping 8.8.8.8
```

The test succeeded, confirming that external IP connectivity was available.

DNS resolution was then tested:

```powershell
nslookup google.com
```

The hostname resolved successfully, confirming that DNS was functioning.

HTTPS connectivity was tested directly on TCP port 443:

```powershell
Test-NetConnection google.com -Port 443
```

The result showed:

```text
PingSucceeded    : True
TcpTestSucceeded : False
```

This was an important distinction. The destination was reachable using ICMP, but a TCP connection to HTTPS port 443 could not be established.

A browser test produced:

```text
ERR_NETWORK_ACCESS_DENIED
```

The investigation therefore moved to local security controls, including Windows Firewall.

A firewall rule was identified that explicitly blocked outbound TCP traffic on port 443.

## Root Cause

An outbound Windows Firewall rule was blocking TCP port 443.

General IP connectivity and DNS continued to function, but applications requiring HTTPS could not establish a TCP connection.

## Resolution

Only the offending firewall rule was removed.

The entire Windows Firewall was not disabled because doing so would unnecessarily reduce the security of the workstation.

## Verification

HTTPS connectivity was tested again:

```powershell
Test-NetConnection google.com -Port 443
```

The result returned:

```text
TcpTestSucceeded : True
```

Secure websites were accessible again and the original issue was resolved.

## Key Learning

A successful ping does not prove that an application or network service is accessible.

Different tests validate different layers:

- `ping` tests basic IP reachability using ICMP.
- `nslookup` tests DNS name resolution.
- `Test-NetConnection -Port 443` tests TCP connectivity to a specific service port.

The incident also demonstrated the importance of making the smallest appropriate security change. Removing the specific problematic firewall rule was preferable to disabling Windows Firewall entirely.
