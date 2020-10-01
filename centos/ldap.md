# LDAP Setup

Before you begin make sure sure the server where LDAP will be installed has a DNS domain name assigned, 
usually localdomain on home LAN.  I used domain local.
Verify by pinging a host by a FQDN, e.g. 
```
ping fuji.local
```
## 389 Server Installation

I used 
[Fedora 389 server](https://en.wikipedia.org/wiki/389_Directory_Server)
and followed 
[these instructions](https://webhostinggeeks.com/howto/setup-389-directory-server-on-centos-7/).
Once the server is running, you should be able to access
389® Administration Express at http://host:9830/dist/download

By the end you should have these services running:
```
[alex@centos7 ~]$ systemctl status dirsrv-admin
● dirsrv-admin.service - 389 Administration Server.
   Loaded: loaded (/usr/lib/systemd/system/dirsrv-admin.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2020-09-30 21:25:12 EDT; 19h ago
  Process: 1649 ExecStart=/usr/sbin/httpd -k start -f /etc/dirsrv/admin-serv/httpd.conf (code=exited, status=0/SUCCESS)
 Main PID: 1650 (httpd)
   CGroup: /system.slice/dirsrv-admin.service
           ├─1650 /usr/sbin/httpd -k start -f /etc/dirsrv/admin-serv/httpd.conf
           ├─1651 /usr/sbin/httpd -k start -f /etc/dirsrv/admin-serv/httpd.conf
           └─1652 /usr/sbin/httpd -k start -f /etc/dirsrv/admin-serv/httpd.conf

Sep 30 21:25:11 centos7.local systemd[1]: Starting 389 Administration Server....
Sep 30 21:25:12 centos7.local systemd[1]: Can't open PID file /var/run/dirsrv/admin-serv.pid (yet?) after start: No such file or directory
Sep 30 21:25:12 centos7.local systemd[1]: Started 389 Administration Server..
[alex@centos7 ~]$ systemctl status dirsrv@centos7
● dirsrv@centos7.service - 389 Directory Server centos7.
   Loaded: loaded (/usr/lib/systemd/system/dirsrv@.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2020-09-30 21:25:11 EDT; 19h ago
  Process: 879 ExecStartPre=/usr/sbin/ds_systemd_ask_password_acl /etc/dirsrv/slapd-%i/dse.ldif (code=exited, status=0/SUCCESS)
 Main PID: 902 (ns-slapd)
   Status: "slapd started: Ready to process requests"
   CGroup: /system.slice/system-dirsrv.slice/dirsrv@centos7.service
           └─902 /usr/sbin/ns-slapd -D /etc/dirsrv/slapd-centos7 -i /var/run/dirsrv/slapd-centos7.pid

Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.054475104 -0400] - NOTICE - ldbm_back_start - found 2160608k available
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.060912352 -0400] - NOTICE - ldbm_back_start - cache autosizing: db cache: 72851k
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.142657332 -0400] - NOTICE - ldbm_back_start - cache autosizing: userRoot entry cache (2 total): 131072k
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.224595208 -0400] - NOTICE - ldbm_back_start - cache autosizing: userRoot dn cache (2 total): 65536k
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.255643909 -0400] - NOTICE - ldbm_back_start - cache autosizing: NetscapeRoot entry cache (2 total): 131072k
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.266200376 -0400] - NOTICE - ldbm_back_start - cache autosizing: NetscapeRoot dn cache (2 total): 65536k
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.271207421 -0400] - NOTICE - ldbm_back_start - total cache size: 412001156 B;
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.333880699 -0400] - NOTICE - dblayer_start - Detected Disorderly Shutdown last time Directory Server was running, recovering database.
Sep 30 21:25:11 centos7.local ns-slapd[902]: [30/Sep/2020:21:25:11.601028264 -0400] - INFO - slapd_daemon - slapd started.  Listening on All Interfaces port 389 for LDAP requests
Sep 30 21:25:11 centos7.local systemd[1]: Started 389 Directory Server centos7..
[alex@centos7 ~]$
```

## 389 Server Management

[Instructions](https://www.unixmen.com/manage-389-directory-server-graphically-using-389-management-console/).

On my Windows laptop I installed Java and then
[389 Management Console](https://directory.fedoraproject.org/docs/389ds/releases/release-windows-console-1-1-15.html).

Possible alternative would be 
[ldp](https://www.active-directory-security.com/2016/06/ldp-for-active-directory-download-usage-tutorial-and-examples.html)

# Verification

Once organizational units and new users are defined, test it like this:

```
$ ldapsearch -x -b "dc=local"
```

To search for just users:
```
[root@centos7 api-umbrella]# ldapsearch -x -b "dc=local" "objectClass=person"
# extended LDIF
#
# LDAPv3
# base <dc=local> with scope subtree
# filter: objectClass=person
# requesting: ALL
#

# asokolsky, support_group, Support Division, local
dn: uid=asokolsky,cn=support_group,ou=Support Division,dc=local
mail: asokolsky@gmail.com
uid: asokolsky
givenName: Alex
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetorgperson
sn: Sokolsky
cn: Alex Sokolsky
telephoneNumber: 1-408.533.5542

# ssokolsky, support_group, Support Division, local
dn: uid=ssokolsky,cn=support_group,ou=Support Division,dc=local
mail: sonya@far.com
uid: ssokolsky
givenName: Sonya
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetorgperson
sn: Sokolsky
cn: Sonya Sokolsky

# search result
search: 2
result: 0 Success

# numResponses: 3
# numEntries: 2
```

To search for a specific username:

```
[root@centos7 api-umbrella]# ldapsearch -x -b "dc=local" "uid=asokolsky"
# extended LDIF
#
# LDAPv3
# base <dc=local> with scope subtree
# filter: uid=asokolsky
# requesting: ALL
#

# asokolsky, support_group, Support Division, local
dn: uid=asokolsky,cn=support_group,ou=Support Division,dc=local
mail: asokolsky@gmail.com
uid: asokolsky
givenName: Alex
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetorgperson
sn: Sokolsky
cn: Alex Sokolsky
telephoneNumber: 1-408.533.5542

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```

Finally, to test authentication against LDAP:

```


```
