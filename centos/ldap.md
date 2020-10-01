# LDAP Setup

Before you begin make sure sure the server where LDAP will be installed has a DNS domain name assigned, 
usually localdomain on home LAN.  I used domain local.
Verify by pinging a host by a FQDN, e.g. 
```
ping fiji.local
```
## 389 Server Installation

I used Fedora 389 server and followed 
[these instructions](https://webhostinggeeks.com/howto/setup-389-directory-server-on-centos-7/).
Once the server is running, you should be able to access
389® Administration Express at http://<host>:9830/dist/download

## 389 Server Management

[Instructions](https://www.unixmen.com/manage-389-directory-server-graphically-using-389-management-console/).

On my Windows laptop I installed Java and then 389 Management Console from
https://directory.fedoraproject.org/docs/389ds/releases/release-windows-console-1-1-15.html
