# Networking in Proxmox

# Reference

https://pve.proxmox.com/wiki/Network_Configuration


# Making Bridge DHCP Client

Just edit /etc/network/interfaces:

```
auto lo
iface lo inet loopback

iface eno1 inet manual

iface enp2s0f0 inet manual

iface enp2s0f1 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.10.130/24
        gateway 192.168.10.1
        bridge-ports eno1
        bridge-stp off
        bridge-fd 0

auto vmbr1
iface vmbr1 inet dhcp
        bridge-ports enp2s0f0
        bridge-stp off
        bridge-fd 0
        post-up /usr/sbin/dhclient vmbr1
#WAN

auto vmbr2
iface vmbr2 inet static
        address 192.168.20.1/24
        bridge-ports enp2s0f1
        bridge-stp off
        bridge-fd 0
#LAN
```

Then

```
root@pve:/etc/network# ifdown vmbr1
root@pve:/etc/network# ifup vmbr1
```
