###Shared storage on the network with iSCSI
Many ways to share storage on a network exist. The iSCSI protocol defines a way to see a remote blocks device as a local disk. A remote device on the network is called iSCSI Target, a client which connects to iSCSI Target is called iSCSI Initiator.

##iSCSI Target Setup
Install admin tools first, configure target to persistantly start at boot time and then start it
```
# yum -y install targetcli
# systemctl enable target
# systemctl start target
```
To start using targetcli, run targetcli and to get a layout of the tree interface, run ls
```
# targetcli
targetcli shell version 2.1.fb37
Copyright 2011-2013 by Datera, Inc and others.
For help on commands, type 'help'.

/> ls
o- / .............................................................................................................. [...]
  o- backstores ................................................................................................... [...]
  | o- block ....................................................................................... [Storage Objects: 0]
  | o- fileio ...................................................................................... [Storage Objects: 0]
  | o- pscsi ....................................................................................... [Storage Objects: 0]
  | o- ramdisk ..................................................................................... [Storage Objects: 0]
  o- iscsi ................................................................................................. [Targets: 0]
  o- loopback .............................................................................................. [Targets: 0]
/>
```

