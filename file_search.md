###Standard File Streams
When commands are executed, by default there are three standard file streams or descriptors always open for use:

1. standard input or **stdin**
2. standard output or **stdout**
3. standard error or **stderr**

Usually, **stdin** is your keyboard, **stdout** and **stderr** are printed on your terminal; often **stderr** is redirected to an error logging file. The **stdin** is often supplied by directing input to come from a file or from the output of a previous command through a pipe. The **stdout** is also often redirected into a file. Since **stderr** is where error messages are written, often nothing will go there.

In Linux, all open files are represented internally by what are called file descriptors. Simply put, these are represented by numbers starting at zero. The **stdin** is file descriptor 0, **stdout** is file descriptor 1, and **stderr** is file descriptor 2. Typically, if other files are opened in addition to these three, which are opened by default, they will start at file descriptor 3 and increase from there.

We can redirect the three standard filestreams so that we can get input from either a file or another command instead of from our keyboard, and we can write output and errors to files or send them as input for subsequent commands. For example, having a program *called do_something* that reads from **stdin** and writes to **stdout** and **stderr**, we can change its input source:
```
$ do_something < input-file
```
If you want to send the output to a file, use the this as in:
```
$ do_something > output-file
```
We can pipe the output of one command or program into another as its input.
```
$ command1 | command2 | command3
```
The above represents what we often call a _pipeline_ and allows linux to combine the actions of several commands into one. 

###Search for files
The ``locate`` utility performs a search through a previously constructed database of files and directories on your system, matching all entries that contain a specified character string. The ``locate`` utilizes the database created by another program, ``updatedb``. Most Linux systems run this automatically once a day. However, you can update it at any time by just running ``updatedb`` from the command line as the root user.
```
# yum install -y mlocate
# updatedb
# locate zip
```
The result of ``locate`` utility can sometimes result in a very long list. To get a shorter more relevant list we can use the ``grep`` program as a filter. It will print only the lines that contain one or more specified strings as in: 
```
$ locate zip | grep bin
/usr/bin/gpg-zip
/usr/bin/gunzip
/usr/bin/gzip
/usr/bin/zipdetails
```
which will list all files and directories with both "zip" and "bin" in their name. 

Wildcards can be used in search for a filename containing specific characters.

|Wildcards|Result|
|---------|-----------|
|?     |Matches any single character|
|*     |Matches any string of characters|
|[set] |Matches any character not in the set of characters|
|[!set]|Matches any character not in the set of characters|

The ``find`` is extremely useful and often-used utility program in the daily life of a Linux system administrator. It recurses down the filesystem tree from any particular directory (or set of directories) and locates files that match specified conditions. The default <pathname> is always the present working directory.
```
$ find /var -name *.log
/var/log/audit/audit.log
/var/log/tuned/tuned.log
/var/log/anaconda/anaconda.log
/var/log/anaconda/anaconda.program.log
/var/log/anaconda/anaconda.packaging.log
/var/log/anaconda/anaconda.storage.log
```
When no arguments are given, ``find`` lists all files in the current directory and all of its subdirectories.

Searching for files and directories named "gcc":
```
$ find /usr -name gcc
```
Searching only for directories named "gcc":
```
$ find /usr -type d -name gcc
```
Searching only for regular files named "test1":
```
$ find /usr -type f -name test1
```
Another good use of ``find`` is being able to run commands on the files that match your search criteria. To find and remove all files that end with .swp:
```
$ find -name "*.swp" -exec rm {} ’;’
```
The {} is a place holder that will be filled with all the file names that result from the find expression, and the preceding command will be run on each one individually. Note that you have to end the command with either ‘;’ or \; Both forms are fine. One can also use the -ok option which behaves the same as -exec except that find will prompt you for permission before executing the command. This makes it a good way to test your results before blindly executing any potentially dangerous commands.
