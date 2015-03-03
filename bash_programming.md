###Bash shell programming
The **shell** is a command line interpreter which provides the user interface for terminal windows. It can also be used to run scripts, even in non-interactive sessions without a terminal window, as if the commands were being directly typed in.
```
#!/bin/bash
find /usr/lib -name "*.c" -ls
```

The first line of the script, that starts with ``#!/bin/bash`` contains the full path of the command interpreter that is to be used on the file. The command interpreter is tasked with executing statements that follow it in the script. Commonly used interpreters include:
```
/usr/bin/perl
/bin/bash
/bin/csh
/bin/tcsh
/bin/ksh
/usr/bin/python
/bin/sh
```

Scripting is not only limited to shell interpreter. It can be used for Python scripts too.
```
# ll script
-rwxr--r--. 1 root root 55 Mar  3 15:22 script
# cat script
#!/usr/bin/python
print "Welcome to the Python script"
# ./script
Welcome to the Python script
```

Scripts can be interactive too.

```
# cat script.sh
#!/bin/bash
   # Interactive reading of variables
   echo "ENTER YOUR NAME"
   read sname
   # Display of variable values
   echo "WELCOME "$sname"!"
# ./script.sh
ENTER YOUR NAME
Adriano
WELCOME Adriano!
```

All shell scripts generate a return value upon finishing execution. The value can be set with the ``exit`` statement. Return values permit a process to monitor the exit state of another process often in a parent-child relationship. This helps to determine how this process terminated and take any appropriate steps necessary, contingent on success or failure. By convention, success is returned as 0, and failure is returned as a non-zero value. The return value is always stored in the ``$?`` environment variable.
```
# cat names.txt
01 Mario Rossi
02 Antonio Esposito
03 Michele Laforca
04 Antonio Esposito
# echo $?
0
# cat names
cat: names: No such file or directory
# echo $?
1
```

###Basic syntax
Scripts require you to follow a standard language syntax. Rules delineate how to define variables and how to construct and format allowed statements, etc. The table lists some special character usages within bash scripts:

|Character|Description|
|---------|-----------|
|#|Used to add a comment, except when used as \#, or as #! when starting a script|
|\|Used at the end of a line to indicate continuation on to the next line|
|;|Used to interpret what follows as a new command|
|$|Indicates what follows is a variable|







