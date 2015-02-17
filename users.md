###Users and Groups
Linux is a multiuser operating system where more than one user can log on at the same time. The ``who`` command lists the currently logged-on users. To identify the current user, use the ``whoami`` command.

```
# who -a
           system boot  2015-02-17 13:28
LOGIN      tty1         2015-02-17 13:28               761 id=tty1
root     + pts/0        2015-02-17 13:29   .         12379 (10.10.10.246)
           run-level 3  2015-02-17 13:29
root     + pts/1        2015-02-17 17:37   .         18762 (10.10.10.246)
```
Linux uses groups for organizing users. Groups are collections of accounts with certain shared permissions. Control of group membership is administered through the ``/etc/group`` file, which shows a list of groups and their members. By default, every user belongs to a default or primary group. When a user logs in, the group membership is set for their primary group and all the members enjoy the same level of access and privilege. Permissions on various files and directories can be modified at the group level.

All Linux users are assigned a unique user ID, the **uid**, which is just an integer, as well as one or more group ID’s, the  **gid**, including a default one which is the same as the user ID. Historically, RedHat based distros start uid's at 500. Other distributions begin at 1000. These numbers are associated with names through the files ``/etc/passwd`` and ``/etc/group``. Groups are used to establish a set of users who have common interests for the purposes of access rights, privileges, and security considerations. Access rights to files and devices are granted on the basis of the user and the group they belong to.
