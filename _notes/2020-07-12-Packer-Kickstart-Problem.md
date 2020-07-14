---
tags: Packer IaC Devops
---

I ran into an issue with **Packer** when building a template for **Centos7**. I was following every online example or template I could find, with no success.

<br>
There were no errors thrown during installation, but the kickstart file didn't appear to be doing anything.
<br>

#### Problem:
The Kickstart file was being ignored.
<br>

Every Packer example I could find used `.cfg` and looked something like this:
<br>
`"boot_command": ["<tab> text ks=http://{{ .HTTPIP }}:{{ .HTTPPort }}/ks.cfg<enter><wait>"]`
<br>

or:
<br>
`"boot_command": ["<esc><wait>linux inst.ks=hd:fd0:/kickstart.cfg<enter>"] `
<br><br>

#### Solution:
The [Centos7 Docs](https://docs.centos.org/en-US/centos/install-guide/Kickstart2/) examples all use `.ks` extension so I switched from `.cfg` to `.ks` and it worked perfectly.
<br>

```"boot_command": [ 
            "<esc>",
            "<wait>",
            "linux inst.ks=hd:fd0:/kickstart.ks",
            "<enter>"
        ]```
