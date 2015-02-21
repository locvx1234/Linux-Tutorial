###Logical Volume Manager layout
Basically a Logical Volume Manager layout **LVM** looks like this:

* **Logical Volume(s)**: ``/dev/fileserver/share``, ``/dev/fileserver/backup``, ``/dev/fileserver/media``
* **Volume Group(s)**: ``fileserver``
* **Physical Volume(s)**: ``/dev/sdb1``, ``/dev/sdc1``, ``/dev/sdd1``, ``/dev/sdc1``

You have one or more physical volumes, and on these physical volumes you create one or more volume groups, and in each volume group you can create one or more logical volumes. If you use multiple physical volumes, each logical volume can be bigger than one of the underlying physical volumes (but of course the sum of the logical volumes cannot exceed the total space offered by the physical volumes). It is a good practice to not allocate the full space to logical volumes, but leave some space unused. That way you can enlarge one or more logical volumes later on if you feel the need for it.

With LVM, a hard drive or set of hard drives is allocated to one or more physical volumes. LVM physical volumes can be placed on other block devices which might span two or more disks. The physical volumes are combined into logical volumes, with the exception of the ``/boot`` partition. The ``/boot`` partition cannot be on a logical volume group because the boot loader cannot read it. If the root partition is on a logical volume, create a separate ``/boot`` partition which is not a part of a volume group. Since a physical volume cannot span over multiple drives, to span over more than one drive, create one or more physical volumes per drive.

The volume groups can be divided into logical volumes, which are assigned mount points, such as ``/home`` and root and file system types, such as **ext2** or **ext3**. When the partitions reach their full capacity, free space from the volume group can be added to the logical volume to increase the size of the partition. When a new hard drive is added to the system, it can be added to the volume group, and partitions that are logical volumes can be increased in size.
