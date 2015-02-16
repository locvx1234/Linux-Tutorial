### Locating Applications
Depending on the specific distribution, programs and software packages can be installed in various directories. In general, executable programs should live in the following directories

```
/bin
/usr/bin
/sbin
/usr/sbin
/opt.
```

One way to locate programs is to employ the which ``utility``. For example, to find out exactly where the diff program resides on the filesystem:
```
$ which diff
/usr/bin/diff
```
If which does not find the program, whereis is a good alternative because it looks for packages in a broader range of system directories:
```
$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz
```
###Accessing Directories
The following commands are useful for directory navigation:

|Command|Result|
|-------|-----------|
|cd 	|Change to your home directory|
|cd ..|Change to parent directory|
|cd - |Change to previous directory|
|cd /	|Changes your current directory to the root (/) directory|

###Exploring the Filesystem
The tree command is a good way to get a bird’s-eye view of the filesystem tree. The following commands can help in exploring the filesystem:

|Command|Result|
|-------|-----------|
|ls 	  |List the contents of the present working directory|
|ls –a  |List all files including hidden files and directories|
|tree   |Displays a tree view of the filesystem|
|tree -d|Just list the directories and suppress listing file names|

###Hard and Symbolic Links
The ``ln`` command can be used to create hard links and or soft links, also known as symbolic links or symlinks. These two kinds of links are very common in UNIX-based operating systems.

Suppose that file1.txt already exists. A hard link, called file2.txt, is created with the command:
```
# ln file1.txt file2.txt
```
Note that two files now appear to exist. However, a closer inspection of the file listing shows that this is not quite true.

```
# ls -l file*
-rw-r--r--. 2 root root 604 Feb 16 11:49 file1.txt
-rw-r--r--. 2 root root 604 Feb 16 11:49 file2.txt
# ls -li file*
134415251 -rw-r--r--. 2 root root 604 Feb 16 11:49 file1.txt
134415251 -rw-r--r--. 2 root root 604 Feb 16 11:49 file2.txt
```
The -i option prints out in the first column the i-node number, which is a unique quantity for each file object. This field is the same for both of the two files; what is really going on here is that it is only one file but it has more than one name associated with it,  as is indicated by the 2 that appears in the output.

```
# ln file1.txt file3.txt
# ls -li file*
134415251 -rw-r--r--. 3 root root 604 Feb 16 11:49 file1.txt
134415251 -rw-r--r--. 3 root root 604 Feb 16 11:49 file2.txt
134415251 -rw-r--r--. 3 root root 604 Feb 16 11:49 file3.txt
```



