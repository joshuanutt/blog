---
title: Converting the Maldev Academy OVA to Hyper-V
tags:
  - malware-development
  - Hyper-V
---

Just a quick note for myself on converting the Maldev Academy OVA to Hyper-V

i used VBox Manage to do this, which is included with Oracle Virtualbox installation.

Use VBoxManage to clone the HD to `.vhd` format:
```
.\VBoxManage.exe clonehd --format vhd "C:\path\from\Maldev-Academy-disk1.vmdk" "C:\path\to\MaldevDisk.vhd"
```

I then had to convert to `.vhdx` format:
```Powershell
Convert-VHD -Path C:\path\from\MaldevDisk.vhd -DestinationPath C:\path\to\MaldevDisk.vhdx
```

Then in Hyper-V just go to `action > import virtual machine` to import the `.vhdx`
