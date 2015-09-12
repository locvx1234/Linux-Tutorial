####Samba server and Windows file sharing
Samba is an open source implementation of the SMB/CIFS protocol. It allows the networking of Microsoft Windows®, Linux, UNIX, and other operating systems. Samba allows a Linux/Unix server to appear as a Windows server to Windows clients.

With Samba, an administrator can do:

1. Serve directory trees and printers to Linux, UNIX, and Windows clients
2. Assist in network browsing with or without NetBIOS
3. Authenticate Windows domain logins
4. Provide WINS name server resolution

Samba is comprised of **smb**, **nmb**, and **winbind** services.

The ``smbd`` server daemon provides file sharing and printing services to Windows clients. In addition, it is responsible for user authentication, resource locking, and data sharing through the SMB protocol. The default ports on which the server listens for SMB traffic are TCP ports 139 and 445.

The ``nmbd`` server daemon understands and replies to NetBIOS name service requests produced by SMB in Windows-based systems. The default port that the server listens to for NMB traffic is UDP port 137.

The ``winbindd`` service resolves user and group information received from a server running Windows. This makes Windows user and group information understandable by Linux and UNIX platforms. This allows Windows domain users to appear and operate as Linux and UNIX users on a Linux or UNIX machine. Both ``winbindd`` and ``smbd`` are bundled with the Samba distribution, but the ``winbindd`` service is controlled separately from the ``smbd`` service.

####Setup a Samba server
We'll setup a Samba server to make Linux file sharing available to Windows clients. Install the Samba package, enable and start the ``smbd`` and ``nmbd`` services

```
# yum install samba
# systemctl enable smb
# systemctl enable nmb
# systemctl start smb
# systemctl start nmb
```

Samba uses ``/etc/samba/smb.conf`` as its configuration file. 

```
# mv /etc/samba/smb.conf /etc/samba/smb.conf.orig
# vi /etc/samba/smb.conf

# =============== Global configuration ===============
[global]
; Windows workgroup name and server description
workgroup = WORKGROUP
server string = My SMB Server %v
; NetBIOS name as the Linux machine will appear in Windows clients
netbios name = MYSMBSERVER
; interfaces where the service is listening to
interfaces = lo ens32
; permitted hosts to use the Samba server
hosts allow = 127. 10.10.10.
; protocol version
max protocol = SMB3
; type of security
security = user
; DNS proxy
dns proxy = no

# =============== Shares configuration ===============
[share1]
; path of files to share
path = /samba/admin/data
; users admitted to use the file sharing service
valid users = admin
; no guest user is admitted
guest ok = no
; make the share writable as Samba make it as readonly by default
writable = yes
; make the share visible as shared folder
browsable = yes

[share2]
path = /samba/user/3ts/data
valid users = user admin
guest ok = no
writable = yes
browsable = yes
```

More than one user can be admitted to access the same share. In our case, the share2 is accessible to both "admin" and "user" users while the share1 is only accessible to "admin" and denied to "user".

