---
tags: [
	hacking,
	kerberos,
	crto
]
---

# Kerberos Encryption Types 

While working on the CRTO course I ran into a hash that I couldn't crack with Hashcat.  Instead returning an error indicating the hash is invalid.

John could crack it though. What the heck?!

The hash was in this format:
```PowerShell
$krb5asrep$svc_oracle@dev.cyberbotic.io:A20A58[...]
```

But it turns out the hash should have been like this, note the **23**:
```Powershell
# 23 = rc4-hmac
$krb5asrep$23$svc_oracle@dev.cyberbotic.io:A20A58[...]
```


## So.. what are Kerberos Encryption Types?
A Kerberos **encryption type** is a specific combination of a cipher algorithm with an integrity algorithm to provide both confidentiality and integrity to data.

#### How are they defined?
Kerberos Encryption Types (etype) are defined in an IANA Registry at: Kerberos Encryption Type Numbers](https://www.iana.org/assignments/kerberos-parameters/kerberos-parameters.xhtml#kerberos-parameters-1)

These are signed values ranging from -2147483648 to 2147483647.

-   Positive values should be assigned only for algorithms specified in accordance with this specification for use with Kerberos or related protocols.
-   Negative values are for private use; local and experimental algorithms should use these values.
-   Zero is reserved and may not be assigned.


## etypes for Windows
Kerberos Encryption Types for Microsoft Windows are decided by the `MsDS-SupportedEncryptionTypes` values or the defaults if not set.

`MsDS-SupportedEncryptionTypes` values can be set from a Group Policy Object.


## Why would somebody use a more insecure encryption type?
To improve interoperability with computers running older versions of Windows.  A service should use the strongest enctype that both the service and the Key Distribution Center support.

Contemporary non-Windows implementations of the Kerberos protocol support:
- RC4
- AES 128-bit
- AES 256-bit

Most implementations are deprecating DES encryption.


## Docs
- ["Kerberos Encryption Type Reference"](http://pig.made-it.com/kerberos-etypes.html)
