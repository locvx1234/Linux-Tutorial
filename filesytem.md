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
Using **NFS** (the Network File System) is one of the methods used for sharing data across physical systems. Many system administrators mount remote users' home directories on a server in order to give them access to the same files and configuration files across multiple client systems. This allows the users to log in to different machines yet still have access to the same files and resources.

On a generic Linux distribution, the NFS server daemon is typically started with the command ``service nfs start``. The file ``/etc/exports`` contains the directories and permissions that a host is willing to share with other systems over NFS. An entry in this file may look like ``/shared *(rw)``. This entry allows the directory ``/shared`` to be mounted using NFS with read and write (rw) permissions and shared with other hosts in the same domain. After modifying the ``/etc/exports`` file, you can use the ``exportfs -av`` command to notify Linux about the directories you are allowing to be remotely mounted using NFS.

On the client machine, if it is desired to have the remote filesystem mounted automatically upon system boot, the ``/etc/fstab`` file is modified to accomplish this. For example, an entry in the client's ``/etc/fsta``b file might look like ``<servername>:/shared /mnt/nfs/shared nfs defaults 0 0``. You can also mount the remote filesystem without a reboot or as a one-time mount by directly using the ``mount`` command. If ``/etc/fstab`` is not modified, this remote mount will not be present the next time the system is restarted.

On RedHat based distributions (CentOS-7) NFS server
```
# yum install -y nfs-utils
# mkdir /var/shared
```
Add an entry into the ``/etc/exports`` file
```
# vi /etc/exports
# /var/shared 10.10.10.0/24(no_root_squash,no_all_squash,rw,sync)
```
Where:
* ``/var/shared`` is the shared folder
* ``10.10.10.0/24`` is IP address range of clients
* ``rw`` is the permission to shared folder
* ``sync`` synchronizes shared folder
* ``no_root_squash`` enables the root privilege
* ``no_all_squash`` enables the user’s authority

Open the port 2049 into the firewall, start and enable the RPC daemon and the NFS server
```
# firewall-cmd --add-port=2049/tcp --permanent
# firewall-cmd --reload
# systemctl start rpcbind
# systemctl start nfs-server
# systemctl enable rpcbind
# systemctl enable nfs-server
# systemctl status rpcbind
# systemctl status nfs-server
```

On the client machine mount the shared folder to a local folder
```
# mkdir -p /mnt/nfs
# mount 10.10.10.97:/var/shared /mnt/nfs
# cd /mnt/nfs
# touch filename.txt
```
