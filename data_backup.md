###Backup the data
The ``rsync`` command is used to synchronize entire directory trees. Basically, it copies file as the ``cp`` command does. In addition, ``rsync`` checks if the file being copied already exists. If the file exists and there is no change in size or modification time, ``rsync`` will avoid an unnecessary copy and save time. Furthermore, because rsync copies only the parts of files that have actually changed, it can be very fast.

The ``rsync`` is very efficient when recursively copying one directory tree via network, because only the differences are transmitted. One often synchronizes the destination directory tree with the origin, using the  ``rsync -r`` option to recursively walk down the directory tree copying all files and directories below the one listed as the source.

```
# rsync -r project_ABC /data/backups
```

###Compress the data
File data is often compressed to save disk space and reduce the time it takes to transmit files over networks. Linux uses a number of methods to perform this compression.

|Command|Usage|
|-------|-----------|
|gzip 	|The most frequently used Linux compression utility|
|bzip2  |Produces files significantly smaller than those produced by gzip|
|xz     |The most space efficient compression utility used in Linux. It is now used by kernel.org to store archives of the Linux kernel.|
|zip    |Is often required to examine and decompress archives from other operating systems|

These techniques vary in the efficiency of the compression (how much space is saved) and in how long they take to compress; generally the more efficient techniques take longer. Decompression time doesn't vary as much across different methods.

Common usage is reported below.

|Command|Usage|
|-------|-----------|
|gzip *|Compresses all files in the current directory; each file is compressed and renamed with a .gz extension.|
|gzip -r projectX|Compresses all files in the projectX directory along with all files in all of the directories under projectX.|
|gunzip foo|De-compresses foo found in the file foo.gz. Under the hood, gunzip command is actually the same as gzip –d.|
|bzip2 *|Compress all of the files in the current directory and replaces each file with a file renamed with a .bz2 extension.|
|bunzip2 *.bz2|Decompress all of the files with an extension of .bz2 in the current directory. Under the hood, bunzip2 is the same as calling bzip2 -d.|
|xz *|Compress all of the files in the current directory and replace each file with one with a .xz extension.|
|xz -dk bar.xz|Decompress bar.xz into  bar and don't remove bar.xz even if decompression is successful.|
|zip backup *|Compresses all files in the current directory and places them in the file backup.zip.|
|unzip backup.zip|Extracts all files in the file backup.zip and places them in the current directory.|

###Archiving data
