###Filesystem Structure
On many systems, including Linux, the **filesystem** is structured like a tree. The tree is usually portrayed as inverted, and starts at what is most often called the **root** directory, which marks the beginning of the hierarchical filesystem and is also denoted by **/**.

The Filesystem Hierarchy Standard (**FHS**) grew out of historical standards from early versions of UNIX. The FHS provides Linux developers and system administrators with a standard directory structure for the filesystem, which provides consistency between systems and distributions. Linux supports various filesystem types created for Linux, along with compatible filesystems from other operating systems. Many older, legacy filesystems are supported. Some examples of filesystem types that Linux supports are:

1. **ext3**, **ext4**, **btrfs**, **xfs** (native Linux filesystems)
2. **vfat**, **ntfs**, **hfs** (filesystems from other operating systems)

Each filesystem resides on a hard disk **partition**. Partitions help to organize the contents of disks according to the kind of data contained and how it is used. For example, important programs required to run the system are often kept on a separate partition than the one that contains files owned by regular users. In addition, temporary files created and destroyed during the normal operation of Linux are often located on a separate partition; in this way, using all available space on a particular partition may not fatally affect the normal operation of the system.

Before you can start using a filesystem, you need to mount it to the filesystem tree at a **mountpoint**. This is simply a directory (which may or may not be empty) where the filesystem is to be attached (mounted). Sometimes you may need to create the directory if it doesn't already exist. If you mount a filesystem on a non-empty directory, the former contents of that directory are covered-up and not accessible until the filesystem is unmounted. Thus mount points are usually empty directories.

The ``mount`` command is used to attach a filesystem somewhere within the filesystem tree. Arguments include the device node and mount point.
```
$ mount /dev/sda5 /mnt
```
This will attach the filesystem contained in the disk partition associated with the ``/dev/sda5`` device node, into the filesystem tree at the ``/mnt`` mount point. Note that unless the system is otherwise configured only the root user has permission to run mount. If you want it to be automatically available every time the system starts up, you need to edit the file ``/etc/fstab`` accordingly. The name is short for Filesystem Table. Looking at this file will show you the configuration of all pre-configured filesystems.

The ``umount`` command is used to detach the filesystem from the mount point.
```
$ umount /mnt
```

The command ``df -Th`` (it stands for disk-free) will display information about mounted filesystems including type and usage statistics about currently used and available space.

```
# df -Th
Filesystem                 Type      Size  Used Avail Use% Mounted on
/dev/mapper/os-root        xfs        50G  2.0G   48G   4% /
devtmpfs                   devtmpfs  1.8G     0  1.8G   0% /dev
tmpfs                      tmpfs     1.9G  4.0K  1.9G   1% /dev/shm
tmpfs                      tmpfs     1.9G  8.6M  1.8G   1% /run
tmpfs                      tmpfs     1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/mapper/swift01-zone01 xfs        49G   33M   49G   1% /srv/node/z1d1
/dev/mapper/swift02-zone02 xfs        49G   33M   49G   1% /srv/node/z2d1
/dev/sda1                  xfs       497M  167M  331M  34% /boot
/dev/mapper/os-data        xfs        20G  261M   20G   2% /data
```
###Network Filesystem
