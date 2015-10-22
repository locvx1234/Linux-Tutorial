###Volume Manager, a real example
Let to see, step-by-step, a real example of Volume Manager on a CentOS 7 setup. 
On the host caldera01 there is an additional disk ``/dev/sdb`` we want to use for store a mysql database.

```
[root@caldera01 ~]# df -Th
Filesystem             Type      Size  Used Avail Use% Mounted on
/dev/mapper/os-root    xfs        50G  2.1G   48G   5% /
devtmpfs               devtmpfs  3.8G     0  3.8G   0% /dev
tmpfs                  tmpfs     3.8G     0  3.8G   0% /dev/shm
tmpfs                  tmpfs     3.8G  8.6M  3.8G   1% /run
tmpfs                  tmpfs     3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/os-data    xfs       175G  256M  175G   1% /data
/dev/sda1              xfs       497M  190M  308M  39% /boot

[root@caldera01 ~]# lsblk
NAME                         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                            8:0    0 232.9G  0 disk
├─sda1                         8:1    0   500M  0 part /boot
└─sda2                         8:2    0 232.4G  0 part
  ├─os-swap                  253:0    0   7.8G  0 lvm  [SWAP]
  ├─os-root                  253:1    0    50G  0 lvm  /
  └─os-data                  253:2    0 174.6G  0 lvm  /data
sdb                            8:16   0 232.9G  0 disk
└─sdb1                         8:17   0 232.9G  0 part

[root@caldera01 ~]# fdisk /dev/sdb
Disk /dev/sdb: 250.1 GB, 250059350016 bytes, 488397168 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0003b431

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048   488397167   244197560   8e  Linux LVM
```

Create a LVM layout
```
[root@caldera01 ~]# pvcreate /dev/sdb1
[root@caldera01 ~]# vgcreate vgdb /dev/sdb1
[root@caldera01 ~]# lvcreate -l 100%FREE -n lvol1 vgdb
[root@caldera01 ~]# pvs
  PV         VG   Fmt  Attr PSize   PFree
  /dev/sda2  os   lvm2 a--  232.39g    0
  /dev/sdb1  vgdb lvm2 a--  232.88g    0
[root@caldera01 ~]# vgs
  VG   #PV #LV #SN Attr   VSize   VFree
  os     1   3   0 wz--n- 232.39g    0
  vgdb   1   1   0 wz--n- 232.88g    0
[root@caldera01 ~]# lvs
  LV    VG   Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  data  os   -wi-ao---- 174.63g
  root  os   -wi-ao----  50.00g
  swap  os   -wi-ao----   7.77g
  lvol1 vgdb -wi-ao---- 232.88g
```

Mount the partition under /db directory and make an ``ext3`` file system
```
[root@caldera01 ~]# mkdir /db
[root@caldera01 ~]# mount /dev/sdb1 /db
```

Install a mysql database on the new filesystem
```
[root@caldera01 ~]# sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
[root@caldera01 ~]# yum install -y mysql-server
[root@caldera01 ~]#  vi /etc/my.cnf
[mysqld]
...
datadir=/db/mysql
...
[root@caldera01 ~]# systemctl start mysqld
[root@caldera01 ~]# systemctl status mysqld
[root@caldera01 ~]# systemctl enable mysqld

```



