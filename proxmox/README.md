# ProxMox HOWTO

## Sources

* my experience
* https://www.youtube.com/watch?v=GoZaMgEgrHw

## Update repositories

You will get an error during the update:
```
Err:4 https://enterprise.proxmox.com/debian/pve buster InRelease
  401  Unauthorized [IP: 144.217.225.162 443]
```

It comes from /etc/apt/sources.list.d/pve-enterprise.list:
```
root@pve02:/etc/apt/sources.list.d# cat pve-enterprise.list 
deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise
```

Change it to:
```
root@pve02:/etc/apt/sources.list.d# cat pve-enterprise.list
# deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise
```
And then:
```
root@pve02:/etc/apt# cat sources.list
deb http://ftp.us.debian.org/debian buster main contrib

deb http://ftp.us.debian.org/debian buster-updates main contrib

# security updates
deb http://security.debian.org buster/updates main contrib

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve buster pve-no-subscription
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

## Make NIC VLAN-aware

In GUI: Server-name\Network\vmbr0, check VLAN aware

## NIC Aggregation

Add in the /etc/network/interfaces
```
iface bond0 inet manual
  bond-slaves eno1 eno1
  bond-miimon 100
  bond-mode 802.3ad
  bond-xmit-hash-policy layer2+3

auto vmbr0
iface vmbr0 inet static
  address 192.168.1.123/24
  gateway 192.168.1.1
  bridge-ports bond0
  bridge-stp off
  bridge-fd 0
  bridge-vlan-aware yes
  bridge-vids 2-4094
```

Follow up on this with the switch configuration.

## Install ifupdown2

```
root@pve02:/etc/apt/sources.list.d# apt install ifupdown2
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  ethtool python3-gvgen python3-mako
The following packages will be REMOVED:
  ifenslave ifupdown
The following NEW packages will be installed:
  ifupdown2
0 upgraded, 1 newly installed, 2 to remove and 0 not upgraded.
Need to get 227 kB of archives.
After this operation, 1,347 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://download.proxmox.com/debian/pve buster/pve-no-subscription amd64 ifupdown2 all 3.0.0-1+pve3 [227 kB]
Fetched 227 kB in 1s (203 kB/s)   
dpkg: ifupdown: dependency problems, but removing anyway as you requested:
 ifenslave depends on ifupdown (>= 0.7.46).

(Reading database ... 50726 files and directories currently installed.)
Removing ifupdown (0.8.35+pve1) ...
Selecting previously unselected package ifupdown2.
(Reading database ... 50690 files and directories currently installed.)
Preparing to unpack .../ifupdown2_3.0.0-1+pve3_all.deb ...
Unpacking ifupdown2 (3.0.0-1+pve3) ...
dpkg: ifenslave: dependency problems, but removing anyway as you requested:
 pve-manager depends on ifenslave (>= 2.6) | ifupdown2 (>= 2.0.1-1+pve8); however:
  Package ifenslave is to be removed.
  Package ifupdown2 is not configured yet.

(Reading database ... 50810 files and directories currently installed.)
Removing ifenslave (2.9) ...
Setting up ifupdown2 (3.0.0-1+pve3) ...
Installing new version of config file /etc/default/networking ...
Processing triggers for man-db (2.8.5-2) ...
root@pve02:/etc/apt/sources.list.d# 
```

## Install lm-sensors

```
root@pve02:/etc/apt/sources.list.d# apt install lm-sensors
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libsensors-config libsensors5
Suggested packages:
  fancontrol read-edid i2c-tools
The following NEW packages will be installed:
  libsensors-config libsensors5 lm-sensors
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 198 kB of archives.
After this operation, 573 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ftp.us.debian.org/debian buster/main amd64 libsensors-config all 1:3.5.0-3 [31.6 kB]
Get:2 http://ftp.us.debian.org/debian buster/main amd64 libsensors5 amd64 1:3.5.0-3 [52.6 kB]
Get:3 http://ftp.us.debian.org/debian buster/main amd64 lm-sensors amd64 1:3.5.0-3 [114 kB]
Fetched 198 kB in 1s (174 kB/s)     
Selecting previously unselected package libsensors-config.
(Reading database ... 50798 files and directories currently installed.)
Preparing to unpack .../libsensors-config_1%3a3.5.0-3_all.deb ...
Unpacking libsensors-config (1:3.5.0-3) ...
Selecting previously unselected package libsensors5:amd64.
Preparing to unpack .../libsensors5_1%3a3.5.0-3_amd64.deb ...
Unpacking libsensors5:amd64 (1:3.5.0-3) ...
Selecting previously unselected package lm-sensors.
Preparing to unpack .../lm-sensors_1%3a3.5.0-3_amd64.deb ...
Unpacking lm-sensors (1:3.5.0-3) ...
Setting up libsensors-config (1:3.5.0-3) ...
Setting up libsensors5:amd64 (1:3.5.0-3) ...
Setting up lm-sensors (1:3.5.0-3) ...
Created symlink /etc/systemd/system/multi-user.target.wants/lm-sensors.service → /lib/systemd/system/lm-senso
rs.service.
Processing triggers for systemd (241-7~deb10u4) ...
Processing triggers for man-db (2.8.5-2) ...
Processing triggers for libc-bin (2.28-10) ...
```

Then

```
sensors-detect
```

And finally:
```
watch -n 1 sensors
```
or just
```
root@pve02:/etc/apt/sources.list.d# sensors
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +36.0°C  (high = +94.0°C, crit = +100.0°C)
Core 0:        +35.0°C  (high = +94.0°C, crit = +100.0°C)
Core 1:        +35.0°C  (high = +94.0°C, crit = +100.0°C)
Core 2:        +36.0°C  (high = +94.0°C, crit = +100.0°C)
Core 3:        +34.0°C  (high = +94.0°C, crit = +100.0°C)
Core 4:        +32.0°C  (high = +94.0°C, crit = +100.0°C)
Core 5:        +32.0°C  (high = +94.0°C, crit = +100.0°C)

acpitz-acpi-0
Adapter: ACPI interface
temp1:        +27.8°C  (crit = +119.0°C)

pch_cannonlake-virtual-0
Adapter: Virtual device
temp1:        +44.0°C  
```

## PCI Pass Through Config

Involves:

* boot command line
* kernel modules to load and blacklist

See [proxmox-gpu-passthrough.md](proxmox-gpu-passthrough.md).

## Add the server to Datacenter/Cluster

Make sure the host has:

* unique host name
* networking is defined - can't change after
* no VMs

## Data Center Storage

Add connection to the NFS server:  Datacenter\Storage Add NFS 

## Configure UPS

[howto-configure-ups-on-proxmox](https://diyblindguy.com/howto-configure-ups-on-proxmox/)

## Schedule Backups

Datacenter\Backup, Add


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

## Enable CPU Turbo Boost

The problem statement here is relevant for AMD CPUs:
https://forum.level1techs.com/t/gigabyte-server-activity-corner-proxmox-docker-and-config-notes/167614

More on these utilities:
https://www.hreniuc.dev/2019/03/28/how-to-use-cpupower-cpu-governors/

```
% cpupower
WARNING: cpupower not found for kernel 5.4.0-66

  You may need to install the following packages for this specific kernel:
    linux-tools-5.4.0-66-generic
    linux-cloud-tools-5.4.0-66-generic

  You may also want to install one of the following packages to keep up to date:
    linux-tools-generic
    linux-cloud-tools-generic

% sudo apt install linux-tools-common
```
or
```
% apt install linux-cpupower cpufrequtils 
```

Then
```
root@fuji:~# cpupower frequency-info
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 800 MHz - 3.90 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 800 MHz and 3.90 GHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 3.90 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes
```

Just make sure that:

```
root@duo:~# cat /sys/devices/system/cpu/intel_pstate/no_turbo 
0
```

## Enable Power Saving

From https://forum.proxmox.com/threads/cpu-power-throttle-back-to-save-energy.27510/

To enable powersave governor:

```
% echo "powersave" | tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

To monitor CPU frequency:

```
% cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_cur_freq
```

