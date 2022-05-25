---
title: "Constrained Delegation - Cobalt Strike and PowerView"
tags: [
	active-directory,
	hacking,
	cobalt-strike,
	red-team,
	crto
]
---

While working on the CRTO lab I wanted to try using PowerShell to find Constrained Delegation like I did in the CRTP course.  They used PowerView, so I figured I'd try using it here.

## Lets do it!
Listing with PowerView:
```PowerShell
# Load PowerView
beacon> powershell-import C:\Tools\PowerSploit\Recon\PowerView.ps1

# Get computer or user with constrained delegation
beacon> powershell Get-DomainUser –TrustedToAuth
beacon> powershell Get-DomainComputer –TrustedToAuth
beacon> powershell Get-DomainComputer –TrustedToAuth | select dnshostname

# interesting:
msds-allowedtodelegateto      : {eventlog/dc-2.dev.cyberbotic.io/dev.cyberbotic
                                .io, eventlog/dc-2.dev.cyberbotic.io, 
                                eventlog/DC-2, 
                                eventlog/dc-2.dev.cyberbotic.io/DEV...}

```

> **MSDS-AllowedToDelegateTo**
This is an attribute on service account (computer or user account) objects. It contains a list of Service Principal Names (SPNs). This attribute is used to configure a service so that it can obtain service tickets that can be used for Constrained Delegation.

Cool, lets expand that attribute to see what we can access:
```PowerShell
beacon> powershell get-domaincomputer -trustedtoauth | select -expandproperty MSDS-AllowedToDelegateTo

eventlog/dc-2.dev.cyberbotic.io/dev.cyberbotic.io
eventlog/dc-2.dev.cyberbotic.io
eventlog/DC-2
eventlog/dc-2.dev.cyberbotic.io/DEV
eventlog/DC-2/DEV
cifs/wkstn-2.dev.cyberbotic.io
cifs/WKSTN-2
```

We have `eventlog` on a few domain controllers, and `CIFS` on a workstation.

> **CIFS**
> CIFS (Common Internet File System) is a *particular implementation* of the Server Message Block protocol, created by Microsoft.