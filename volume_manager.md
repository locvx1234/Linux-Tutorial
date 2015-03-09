###Logical Volume Manager layout
Basically a Logical Volume Manager layout **LVM** looks like this:

* **Logical Volume(s)**: ``/dev/fileserver/share``, ``/dev/fileserver/backup``, ``/dev/fileserver/media``
* **Volume Group(s)**: ``fileserver``
* **Physical Volume(s)**: ``/dev/sdb1``, ``/dev/sdc1``, ``/dev/sdd1``, ``/dev/sdc1``

You have one or more physical volumes, and on these physical volumes you create one or more volume groups, and in each volume group you can create one or more logical volumes. If you use multiple physical volumes, each logical volume can be bigger than one of the underlying physical volumes (but of course the sum of the logical volumes cannot exceed the total space offered by the physical volumes). It is a good practice to not allocate the full space to logical volumes, but leave some space unused. That way you can enlarge one or more logical volumes later on if you feel the need for it.

With LVM, an hard drive or set of hard drives or different partitions of the same hard drive are allocated to one or more physical volumes. The physical volumes can be placed on other block devices which might span two or more disks. The physical volumes are combined into logical volumes, with the exception of the ``/boot`` partition. The ``/boot`` partition cannot be on a logical volume group because the boot loader cannot read it. If the root partition is on a logical volume, create a separate ``/boot`` partition which is not a part of a volume group. Since a physical volume cannot span over multiple drives, to span over more than one drive, create one or more physical volumes per drive.

The volume groups can be divided into logical volumes, which are assigned mount points, such as ``/home`` and root and file system types, such as **ext2** or **ext3**. When the partitions reach their full capacity, free space from the volume group can be added to the logical volume to increase the size of the partition. When a new hard drive is added to the system, it can be added to the volume group, and partitions that are logical volumes can be increased in size.

###LVM example
On the local CentOS machine, there are 2 hard drive ``/dev/sda`` and ``/dev/sdb``. The ``/dev/sda`` is partioned as follow
```
# fdisk -l /dev/sda

Disk /dev/sda: 250.1 GB, 250059350016 bytes, 488397168 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000b78bc

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1026047      512000   83  Linux
/dev/sda2         1026048   155004927    76989440   8e  Linux LVM
/dev/sda3       155004928   488397167   166696120   83  Linux LVM
```

The ``/dev/sda1`` is for the ``/boot`` partition and is not into LVM layout. Both ``/dev/sda2`` and ``/dev/sda3`` partitions are part of the LVM layout. Note that both the partitions are part of the same physical disk. This is not so common in production but is possible to have. More common is the case of partitions belonging to different physical disks.
```
# lvmdiskscan
  /dev/os/swap [       3.89 GiB]
  /dev/sda1    [     500.00 MiB]
  /dev/os/root [      50.00 GiB]
  /dev/sda2    [      73.42 GiB] LVM physical volume
  /dev/os/data [     178.50 GiB]
  /dev/sda3    [     158.97 GiB] LVM physical volume
  /dev/sdb1    [     232.88 GiB]
  3 disks
  2 partitions
  0 LVM physical volume whole disks
  2 LVM physical volumes

# pvs
  PV         VG   Fmt  Attr PSize   PFree
  /dev/sda2  os   lvm2 a--   73.42g    0
  /dev/sda3  os   lvm2 a--  158.97g    0

# vgs
  VG   #PV #LV #SN Attr   VSize   VFree
  os     2   3   0 wz--n- 232.39g    0

# lvs
  LV   VG   Attr       LSize   Pool Origin Data%  Move Log Cpy%Sync Convert
  data os   -wi-ao---- 178.50g
  root os   -wi-ao----  50.00g
  swap os   -wi-ao----   3.89g
```
The two partitons are seen as two LVM physical volumes: ``/dev/sda2`` and ``/dev/sda3``. The two phisical volumes are part of the same volume group called ``os``. On top of this volume group there are three logical volumes: ``/root``, ``/data`` and ``/swap``.



