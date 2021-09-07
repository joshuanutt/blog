---
tags: vSphere ESXI
---


Check if AD Object returned is group or user:

```powershell
if ($adObject.objectClass -eq "group"){

}

if ($adObject.objectClass -eq "user"){

}
```


Check if multiple users returned from a get-aduser query:

```powershell
if ($User -is [system.array]){
    # Throw an error, multiple users returned
}
elseif ($User -is [Microsoft.ActiveDirectory.Management.ADAccount]){
    # do something
}
```