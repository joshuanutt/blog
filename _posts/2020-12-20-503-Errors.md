---
tags: vSphere ESXI
---


# vSphere... broken!
This weekend when I went to play in the homelab but found my vSphere web client inaccessible with a 503 error.

> 503 Service Unavailable (Failed to connect to endpoint: [N7Vmacore4Http20NamedPipeServiceSpecE:0x00007fb288019690] _serverNamespace = / action = Allow _pipeName =/var/run/vmware/vpxd-webserver-pipe)


I ssh'ed into the vSphere appliance to check the status of the services and found vpxd was stopped and would not start.  The error message was too generic to be useful.

```sh
service-control --status --all
service-control --start vpxd
```

<br>

## How I got the logs off the server
If I had the log browser setup, I would have used that.
https://kb.vmware.com/s/article/2032888

Instead, I used SCP to pull the logs off the server.

To do that, you need to set the shell to bash on the vSphere appliance.

```sh
chsh -s "/bin/bash" root
```

SCP will work like normal.

```sh
scp root@vsphereIP:<path>  <path>
```

Set it back to the appliance shell.

```sh
chsh -s /bin/appliancesh root
```

<br>

## Disk space
I started by checking the disk space and found 0% free on one of the partitions.

```sh
df -h

Disk path		Capacity	Free
/storage/core	24.47 GB	0 GB (0%)
```


**Files were filling up this path, this issue is resolved on vCenter Server 6.7 (Update 3)**

<br>

## Unable to start service 'vpxd'
Even with free space the vpxd service would not start.

Several blog posts indicated services may not start if there is a disk issue.
Suggestion was to fsck a volume, but this didn't help.
https://kb.vmware.com/s/article/2147154

I started looking through the logs and found something useful with this.

```sh
cat /var/log/vmware/cm/cm.log | grep "lookup service failed"
```

Hundreds of error messages similar to this:

```
Service Failed; 
uri:https://vsphere.joshnutt.lan/lookupservice/sdk 
[com.vmware.vim.vmomi.clent.exception.connectionException: java.net.UnknownHostException: vsphere.joshnutt.lan: Name or service not known]
```

I tried removing the dns entry for the vsphere server from my firewall.  In hindsight this doesn't really make sense, but it did at the time.

After that I was able to start the vsphere-client service.  No luck with vpxd though.

```sh 
service-control --start vsphere-client
```

After some digging I discovered that the root password was expired.

```sh
chage -l root
passwd root
```

Still no luck.

<br>

## Found in vpxd.log
I started poking around in the other logs.   VPXD log had some useful information in it though.

```sh
Failed to connect socket; 

[ConnectAndLogin] Failed to loginBySamlToken:
```

Turns out a second password was expired.

```sh
service-control --stop --all
vcenter-restore -u administrator -p <administrator@vsphere.local password>
service-control --start --all
service-control --start vpxd
```

<br>

### Fixing the DNS entry
Now I can get to the vsphere login, but can't sign in.   When trying to sign in, it attempts to redirect to vsphere.joshnutt.lan which it can't find.

It can't find it because... I removed the DNS entry earlier!

All I needed to do was readd the DNS entry and now I can get back to automating infrastructure with Terraform!

<br>

