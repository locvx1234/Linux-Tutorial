###Linux basic security
By default, Linux has several account types in order to isolate processes and workloads:

1. **root**
2. **system**
2. **normal**
3. **network**

For a safe environment, it is advised to grant the minimum privileges possible and necessary to accounts, and remove inactive accounts. The ``last`` command, which shows the last time each user logged into the system, can be used to help identify potentially inactive accounts which are candidates for system removal.
```
# last
adriano  pts/4        10.10.10.113     Thu Feb 19 16:50   still logged in
mina     pts/2        10.10.10.113     Thu Feb 19 16:39   still logged in
root     pts/1        10.10.10.113     Thu Feb 19 16:25 - 16:25  (00:00)
root     pts/0        10.10.10.113     Thu Feb 19 15:42   still logged in
adriano  pts/3        10.10.10.246     Wed Feb 18 17:53 - 18:44  (00:51)
root     pts/2        10.10.10.99      Wed Feb 18 17:14 - 18:44  (01:30)
adriano  pts/1        10.10.10.246     Wed Feb 18 16:57 - 19:19  (02:22)
root     pts/0        10.10.10.246     Wed Feb 18 16:25 - 19:19  (02:53)
root     pts/0        10.10.10.246     Tue Feb 17 13:29 - 19:29  (06:00)
reboot   system boot  3.10.0-123.20.1. Tue Feb 17 13:28 - 17:20 (2+03:51)
```

The **root** account is the most privileged account on a Linux/UNIX system. This account has the ability to carry out all facets of system administration, including adding accounts, changing user passwords, examining log files, installing software, etc. 

A regular account user can perform some operations requiring special permissions; however, the system configuration must allow such abilities to be exercised. Running a network client or sharing a file over the network are operations that do not require a root account.

In Linux you can use either ``su`` or ``sudo`` commands to temporarily grant root access to a normal user; these methods are actually quite different. When using the ``su`` command:

* to elevate the privilege, you need to enter the root password. Giving the root password to a normal user should never, ever be done
* once a user elevates to the root account, the normal user can do anything that the root user can do for as long as the user wants, without being asked again for a password
* there are limited logging features

When using the ``sudo`` command:

* you need to enter the user’s password and not the root password
* what the user is allowed to do can be precisely controlled and limited; by default the user will either always have to keep giving their password to do further operations with ``sudo``, or can avoid doing so for a configurable time interval
* detailed logging features are available

###The sudo command
Granting privileges using the ``sudo`` command is less dangerous than ``su`` and it should be preferred. By default, ``sudo`` must be enabled on a per-user basis. However, some distributions (such as Ubuntu) enable it by default for at least one main user, or give this as an installation option. To execute just one command with root privilege type ``sudo <command>``. When the command is complete you will return to being a normal unprivileged user. The ``sudo`` configuration files are stored in the ``/etc/sudoers`` file and in the ``/etc/sudoers.d/`` directory. By default, that directory is empty.
