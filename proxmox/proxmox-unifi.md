# Proxmox Containter with Unifi Controller

Sources:

* https://www.youtube.com/watch?v=FiZDit-xNvk
* https://proxmox-openvz.blogspot.com/2019/09/installing-ubiquiti-unifi-controller-as.html
* https://pve.proxmox.com/wiki/Command_line_tools

## Download container template

In Proxmox Datacenter\fuji\local open Template, download Ubuntu-20 container template

## Create container

I used:

* Ubuntu 20 template
* Disk: 8GB
* Cores: 1
* RAM: 2GB
* Swap: 512MB
* Net: DHCP
* 

## Upgrade Container

Usual apt update/upgrade


## Download install script

Read https://community.ui.com/questions/UniFi-Installation-Scripts-or-UniFi-Easy-Update-Script-or-Ubuntu-16-04-18-04-18-10-19-04-and-19-10-/ccbc7530-dd61-40a7-82ec-22b17f027776

Then

```
root@unicon:~# wget https://get.glennr.nl/unifi/install/unifi-6.0.28.sh
--2020-10-24 11:44:03--  https://get.glennr.nl/unifi/install/unifi-6.0.28.sh
Resolving get.glennr.nl (get.glennr.nl)... 51.38.48.41
Connecting to get.glennr.nl (get.glennr.nl)|51.38.48.41|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 149804 (146K) [text/x-sh]
Saving to: ‘unifi-6.0.28.sh’

unifi-6.0.28.sh                     100%[=================================================================>] 146.29K   104KB/s    in 1.4s    

2020-10-24 11:44:06 (104 KB/s) - ‘unifi-6.0.28.sh’ saved [149804/149804]

root@unicon:~# chmod a+x unifi-6.0.28.sh 
```

## Controller Setup

Point browser at https://unicon:8443

Step 1/6: Name controller: Apricot960

etc
