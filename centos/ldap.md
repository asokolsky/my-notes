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
