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

