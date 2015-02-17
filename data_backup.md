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
|xz     |The most space efficient compression utility used in Linux|
|zip    |Is often required to examine and decompress archives from other operating systems|


