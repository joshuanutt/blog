# Splatting Custom Attributes with Active Directory

Using splatted values can help better organize your code for managing AD Objects.

<br>

## Splatting Basics
Splatting is a method of passing a collection of parameter values to a command as a unit. 

They are simply hash tables that contain value pairs consisting of ```parameter name``` and ```value```. With the main difference is that when referenced you use ```@``` instead of ```$```.

```powershell
$splat = @{
	Identity="josh"
}

Get-ADUser @splat
```

This example probably won't sell you on splatting so lets look at some slightly longer examples.

Compare these two:

```powershell
Set-ADUser -Identity Josh -DisplayName "Joshua" -EmailAddress "Josh@Company.com" -Department "IT" -Country "US"
```

```powershell
$Arguments = @{
	Identity = "Josh"
	DisplayName = "Joshua"
	EmailAddress = "Josh@company.com"
	Department = "IT"
	Country = "US"
}

Set-ADUser @Arguments
```

<br>

#### Which values can I splat?
Easiest way to tell is to use Intellisense.

If the parameter is suggested when starting to type it into the console, then it can be splatted.

Can splat Email Address:<br>
![PowerShell Console Image](splat01.png)

```powershell
$Arguments = @{
	EmailAddress = "Josh@corp.com"
}

Set-ADUser test.josh @Arguments
```

Can not splat extension attributes:<br>
![PowerShell Console Image](splat02.png)

```powershell
$Arguments = @{
	ExtensionAttribute1 = "This will not work."
}

Set-ADUser test.josh @Arguments
```

<br>

## Ok, So What About the Other Attributes?
How do we splat parameters that cannot be modified using a cmdlet parameter (like ```-Country``` for ```Set-ADUser```)?
**Nested Hash Tables!**

```-Add``` and ```-OtherAttributes``` are just parameters, so treat them the same.  Since they take a hash table as a value just pass them valid parameter/value pairs and you're good to go.

<br>

#### ```New-ADUser``` Example

```powershell
$OtherAttributes = @{
    Title = "Title";
    Description = "Description"
    Department = "Department"
    C = "US"
    GivenName = "Josh"
    SN = "Nutt"
    extensionAttribute1 = "value"
    myCustomAttribute = "SomeValue"
}

$UserSettings = @{
    Name = "Josh Nutt"
    SamAccountName = "Josh"
    DisplayName = "Joshua Nutt"
    UserPrincipalName = "Josh@corp.com"
    Path   = "OU=People Named Josh,DC=corp,DC=lan"
    Server = "DomainServer02"

    OtherAttributes = $OtherAttributes
}

new-aduser @UserSettings -ErrorAction Stop
```

```$UserSettings``` contains a value "OtherAttributes" which contains another hashtable of attributes

If you did this without splatting it would get out of hand pretty quickly!

```powershell
New-ADUser -Name "Josh Nutt" -SamAccountName "Josh" -DisplayName "Joshua Nutt" -OtherAttributes @{Title="Title";Department="Department";C = "US";SN = "Nutt";extensionAttribute1 = "value"}
```

<br>

#### ```Set-ADUser``` Example:

```powershell
$CustomProperties = @{
	extensionAttribute1  = "Value1"
	extensionAttribute2  = "Value2"
}

$UserSettings = @{
	Identity="Josh"
	DisplayName="Joshua Nutt"

	# Or you can use Replace
	Add=$CustomProperties
}

Set-ADUser @UserSettings
```

Same concept with ```New-ADUser``` except Microsoft chose to use ```-Add``` instead of ```-OtherAttributes```.

<br>	

## Conclusion
Using splatting has cleaned up my code and made it easier to maintain, I hope it helps somebody else.

Additional information can be found here: <br>	
[Set-ADUser Documentation](https://docs.microsoft.com/en-us/powershell/module/addsadministration/set-aduser?view=win10-ps#parameters) 
<br>	
[Splatting Documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_splatting?view=powershell-7.1)
<br>	
[Microsoft Dev Blog - Splatting](https://devblogs.microsoft.com/scripting/use-splatting-to-simplify-your-powershell-scripts/)
