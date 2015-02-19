###Linux basic security
By default, Linux has several account types in order to isolate processes and workloads:

1. root
2. System
2. Normal
3. Network

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
