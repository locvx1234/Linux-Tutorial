###System Services
Systemd is the new init system for modern Linux distributions replacing the old init based on ``/etc/init.d/script``. It provides many powerful features for starting, stopping and managing processes. Here is an example to create a MineCraft service for systemd. MainCraft is a Java based game from Mojang.

First, install the game and its envinronment.
```
# yum install java-1.8.0-openjdk.x86_64
# which java
/bin/java
# mkdir /root/Minecraft
# cd /root/Minecraft
# wget -O minecraft_server.jar https://s3.amazonaws.com/Minecraft.Download/versions/1.8.6/minecraft_server.1.8.6.jar
# ls -lrt
-rw-r--r--. 1 root root 9780573 May 25 11:47 minecraft_server.jar
-rw-r--r--. 1 root root       2 Jun  1 11:48 whitelist.json
-rw-r--r--. 1 root root     180 Jun  1 12:01 eula.txt
drwxr-xr-x. 2 root root    4096 Jun  1 16:09 logs
-rw-r--r--. 1 root root     785 Jun  1 16:09 server.properties
-rw-r--r--. 1 root root       2 Jun  1 16:09 banned-players.json
-rw-r--r--. 1 root root       2 Jun  1 16:09 banned-ips.json
-rw-r--r--. 1 root root       2 Jun  1 16:09 ops.json
-rw-r--r--. 1 root root     109 Jun  1 16:10 usercache.json
drwxr-xr-x. 8 root root    4096 Jun  1 16:37 world
```


