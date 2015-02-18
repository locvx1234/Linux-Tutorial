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

Only the root user can add and remove users and groups. Adding a new user is done with the ``useradd`` command and removing an existing user is done with the ``userdel`` command. In the simplest form an account for the new user adriano would be done with:
```
# useradd adriano
# cat /etc/passwd | grep adriano
adriano:x:1000:1000::/home/adriano:/bin/bash
# ls -lrta /home/adriano/
total 16
-rw-r--r--. 1 adriano adriano 231 Sep 26 03:53 .bashrc
-rw-r--r--. 1 adriano adriano 193 Sep 26 03:53 .bash_profile
-rw-r--r--. 1 adriano adriano  18 Sep 26 03:53 .bash_logout
drwxr-xr-x. 3 root    root     20 Feb 17 17:48 ..
-rw-------. 1 adriano adriano   9 Feb 17 17:49 .bash_history
drwx------. 2 adriano adriano  79 Feb 17 17:49 .
```
which by default sets the his home directory to ``/home/adriano``, populates it with some basic files and sets the default shell to ``/bin/bash``.

Remove the user account by typing:
```
# userdel adriano
# cat /etc/passwd | grep adriano
# ls -lrta /home/adriano/
total 16
-rw-r--r--. 1 1000 1000 231 Sep 26 03:53 .bashrc
-rw-r--r--. 1 1000 1000 193 Sep 26 03:53 .bash_profile
-rw-r--r--. 1 1000 1000  18 Sep 26 03:53 .bash_logout
drwxr-xr-x. 3 root root  20 Feb 17 17:48 ..
-rw-------. 1 1000 1000   9 Feb 17 17:49 .bash_history
drwx------. 2 1000 1000  79 Feb 17 17:49 .
```
However, this will leave the home directory intact. This might be useful if it is a temporary inactivation. To remove the home directory while removing the account one needs to use the related option.
```
# userdel -r adriano
# cat /etc/passwd | grep adriano
# ls -lrta /home/adriano/
ls: cannot access /home/adriano/: No such file or directory
```
The command ``id`` with no argument gives information about the current user. If given the name of another user as an argument, id will report information about that other user.
```
# id
uid=0(root) gid=0(root) groups=0(root)
# id adriano
uid=1000(adriano) gid=1000(adriano) groups=1000(adriano)
```

Adding a new group is done with the ``groupadd`` command and removed with the ``groupdel`` command.
```
# groupadd newgroup
# groupdel newgroup
```
Adding a user to an already existing group is done with the ``usermod`` command. Removing a user from the group is a somewhat trickier.

```
# groupadd newgroup
# usermod -G newgroup adriano
# groups adriano
adriano : adriano newgroup
# usermod -g newgroup adriano
# groups adriano
adriano : newgroup
#
```
All these commands update the ``/etc/group`` as necessary. The ``groupmod`` command can be used to change the group properties such as the Group ID or the name
```
# groupmod newgroup -n newgoupname
# groups adriano
adriano : newgoupname
```

###The root user
