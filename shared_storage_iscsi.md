###Shared storage on the network with iSCSI
Many ways to share storage on a network exist. The iSCSI protocol defines a way to see a remote blocks device as a local disk. A remote device on the network is called iSCSI Target, a client which connects to iSCSI Target is called iSCSI Initiator.

###iSCSI Target Setup
Install admin tools first, configure target to persistantly start at boot time and then start it
```
# yum -y install targetcli
# systemctl enable target
# systemctl start target
```
To start using ``targetcli``, run it and to get a layout of the tree interface, run ls
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
####Create a Backstore
Backstores enable support for different methods of storing an object on the local machine. Creating a storage object defines the resources the backstore will use. The supported backstores are: block devices, files, pscsi and ramdisks. Block devices are in our case.
```
/> /backstores/block create name=block_storage dev=/dev/sdb1
Generating a wwn serial.
Created block storage object block_backend using /dev/sdb1.
```
####Create an iSCSI Target
Create an iSCSI target using a specified name
```
/> iscsi/ create iqn.2015-05.com.noverit.caldara02:3260
Created target iqn.2015-05.com.noverit.caldara02:3260.
Created TPG 1.
```
####Configure an iSCSI Portal
An iSCSI Portal is an object specifying the IP address and port where the iSCSI target listen to incoming connections
```
/> /iscsi/iqn.2015-05.com.noverit.caldara02:3260/tpg1/portals/ create
Using default IP port 3260
Binding to INADDR_ANY (0.0.0.0)
Created network portal 0.0.0.0:3260
```
By default, a portal is created when the iSCSI Target is created listening on all IP addresses (0.0.0.0) and the default iSCSI port 3260. Make sure that the 3260 is not used by another application, else specify a different port.

####Configure Access List
Create an Access List for each initiator that will be connecting to the target. This enforces authentication when that initiator connects, allowing only LUNs to be exposed to each initiator. Usually each initator has exclusive access to a LUN. All initiators have unique identifying names IQN. The initiator's unique name IQN must be known to configure ACLs. For open-iscsi initiators, this can be found in the ``/etc/iscsi/initiatorname.iscsi`` file.
```
# cat /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.1994-05.com.redhat:2268c31791
```
If required, use this IQN to enforce authentication by creating the ACLs.

####Configure the LUNs
A Logical Unit Number (LUN) is a number used to identify a logical unit, which is a device addressed by the standard SCSI protocol or Storage Area Network protocols which encapsulate SCSI, such as Fibre Channel or iSCSI itself. 
To configure LUNs, create LUNs of already created storage objects.
```
/> /iscsi/iqn.2015-05.com.noverit.caldara02:3260/tpg1/luns/ create /backstores/block/block_storage
Created LUN 0.
```
At the end of configuration, the iSCSI target envinronment should look like the following
```
/> ls
o- / ........................................................................................................... [...]
  o- backstores ................................................................................................ [...]
  | o- block .................................................................................... [Storage Objects: 2]
  | | o- ana-storage ...................................................... [/dev/sdb1 (20.0GiB) write-thru activated]
  | | o- oracle-storage .................................................. [/dev/sdb2 (120.0GiB) write-thru activated]
  | o- fileio ................................................................................... [Storage Objects: 0]
  | o- pscsi .................................................................................... [Storage Objects: 0]
  | o- ramdisk .................................................................................. [Storage Objects: 0]
  o- iscsi .............................................................................................. [Targets: 1]
  | o- iqn.2015-05.com.noverit.caldara02:3260 .............................................................. [TPGs: 1]
  |   o- tpg1 .................................................................................... [gen-acls, no-auth]
  |     o- acls ............................................................................................ [ACLs: 0]
  |     o- luns ............................................................................................ [LUNs: 2]
  |     | o- lun0 .................................................................... [block/ana-storage (/dev/sdb1)]
  |     | o- lun1 ................................................................. [block/oracle-storage (/dev/sdb2)]
  |     o- portals ...................................................................................... [Portals: 1]
  |       o- 10.10.10.98:3260 ................................................................................... [OK]
  o- loopback ........................................................................................... [Targets: 0]
/>
/> exit
Global pref auto_save_on_exit=true
Last 10 configs saved in /etc/target/backup.
Configuration saved to /etc/target/saveconfig.json
```
The ``/etc/target/saveconfig.json`` file contains the above configuration.

Restart the target service 
```
]# service target restart
Redirecting to /bin/systemctl restart  target.service
```


