There are several guides on how to extend logical volume space, but nothing showing what to do if your virtual group and logical volume is full.


create  partition
```sh
fdisk /dev/sda

command (m for help): n
command (m for help): p
First sector (93634560-314572799, default 93634560):
Last sector, +sectors or +size{K,M,G} (93634560-314572799, default 314572799):

```

Create a physical volume from the partition
```sh
lvm pvcreate /dev/sda3
```

My drive was too full to do the extension, so I deleted some old logs.

Extend vg
```sh
vgextend centos_centos7 /dev/sda3
```

The logical volume was too full, and the +100%FREE was not working
```sh
lvextend -An -r -L +10g /dev/centos_centos7/root
```

Once the LV was extended a bit I could extend the rest of the way
```sh
 lvextend /dev/centos_centos7/root -l+100%FREE
```


Extend XFS

```sh
df -h

Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/centos_centos7-root   23G   23G   20K 100% /
devtmpfs                         4.8G     0  4.8G   0% /dev
tmpfs                            4.9G  8.0K  4.9G   1% /dev/shm
tmpfs                            4.9G  490M  4.4G  10% /run
tmpfs                            4.9G     0  4.9G   0% /sys/fs/cgroup
/dev/sda1                       1014M  147M  868M  15% /boot
overlay                           23G   23G   20K 100% /var/lib/docker/overlay2/
```

100% full on overlay.

We need to extend the XFS file system

-n shows the xfs file data, and does not extend.
```sh
xfs_growfs -n /var/lib/docker/

meta-data=/dev/mapper/centos_centos7-root isize=512    agcount=8, agsize=798208 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=5814272, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

no flag, extends, note the blocks changing.
```sh
xfs_growfs /var/lib/docker/

meta-data=/dev/mapper/centos_centos7-root isize=512    agcount=8, agsize=798208 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=5814272, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 5814272 to 26784768
```

Check the use % change:

```sh
df -h

Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/centos_centos7-root  103G   23G   80G  22% /
devtmpfs                         4.8G     0  4.8G   0% /dev
tmpfs                            4.9G  8.0K  4.9G   1% /dev/shm
tmpfs                            4.9G  490M  4.4G  10% /run
tmpfs                            4.9G     0  4.9G   0% /sys/fs/cgroup
/dev/sda1                       1014M  147M  868M  15% /boot
overlay                          103G   23G   80G  22% /var/lib/docker/overlay2/02bc5da772bd1b882950b843d7a235e873b59de343ee30bb2a71df4cb0a51b3b/merged
```

# Gitlab Error

> Failed opening the RDB file dump.rdb (in server root dir /var/opt/gitlab/redis) for saving: No space left on device
