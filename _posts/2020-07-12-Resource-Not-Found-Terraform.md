
## Terraform can't find my Datastore?
Terraform couldn't find my datastore so I used [GOVC](https://github.com/vmware/govmomi/tree/master/govc) to get the exact path.
<br>

Once installing `GOVC` I was able to find the path via `govc datastore.info`.
<br>

Output from the script will contain the path:
> Name: TXDatastore01 \
> Path: /Datacenter/datastore/TXDatastore01
<br>

I took the path and added it to the Terraform file:
```
    data "vsphere_datastore" "datastore" { 
        name          = "/Datacenter/datastore/TXDatastore01" 
        datacenter_id = data.vsphere_datacenter.dc.id 
     }
```

