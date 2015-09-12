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


