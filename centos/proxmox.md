# ProxMox HOWTO

## Container Manipulation

List containers:
```
 pct list
```
Start and enter into a container (without password):
```
 pct start VMID
 pct enter VMID
```

## Container Templates
```
 pveam update
 pveam available
 pveam download local ubuntu-18.10-standard_18.10-2_amd64.tar.gz
```

## Disable IPv6

Edit the /etc/sysctl.conf file to disable it for all the adapters

```
net.ipv6.conf.all.disable_ipv6 = 1
```
For a particular adapter enp0s3:

```
net.ipv6.conf.enp0s3.disable_ipv6 = 1
```

