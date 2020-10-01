# LDAP Server Setup

Before you begin make sure sure the server where LDAP will be installed has a DNS domain name assigned, 
usually localdomain on home LAN.  Verify by doing a ping by a fully qualified DNS name, e.g. 

  ping fiji.local

# LDAP Server Installation

I used Fedora 389 server and followed these instructions:

https://webhostinggeeks.com/howto/setup-389-directory-server-on-centos-7/

And managed it following these instructions:

https://www.unixmen.com/manage-389-directory-server-graphically-using-389-management-console/

I used Windows laptop to install Java and then 389 Management Console from
https://directory.fedoraproject.org/docs/389ds/releases/release-windows-console-1-1-15.html
