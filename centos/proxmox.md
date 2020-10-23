# ProxMox HOWTO

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

## GPU Pass Through

Sources:

* https://pve.proxmox.com/wiki/Pci_passthrough
* https://www.youtube.com/watch?reload=9&v=fgx3NMk6F54

Edit grub, then:

```
root@fuji:~# dmesg | grep -e DMAR -e IOMMU
[    0.008035] ACPI: DMAR 0x0000000087ACAD28 000070 (v01 INTEL  SKL      00000001 INTL 00000001)
[    0.019700] DMAR: IOMMU enabled
[    0.048702] DMAR: Host address width 39
[    0.048703] DMAR: DRHD base: 0x000000fed90000 flags: 0x1
[    0.048706] DMAR: dmar0: reg_base_addr fed90000 ver 1:0 cap d2008c40660462 ecap f050da
[    0.048707] DMAR: RMRR base: 0x00000087813000 end: 0x00000087832fff
[    0.048709] DMAR-IR: IOAPIC id 2 under DRHD base  0xfed90000 IOMMU 0
[    0.048709] DMAR-IR: HPET id 0 under DRHD base 0xfed90000
[    0.048710] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[    0.050125] DMAR-IR: Enabled IRQ remapping in x2apic mode
[    0.653713] DMAR: No ATSR found
[    0.653745] DMAR: dmar0: Using Queued invalidation
[    0.655720] DMAR: Intel(R) Virtualization Technology for Directed I/O
```

Edit /etc/modules:
```
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

Reboot.  Then:

```
root@fuji:~# lsmod|grep vfio
vfio_pci               53248  0
vfio_virqfd            16384  1 vfio_pci
irqbypass              16384  2 vfio_pci,kvm
vfio_iommu_type1       32768  0
vfio                   32768  2 vfio_iommu_type1,vfio_pci
```

Verifying remapping is enabled:

```
root@fuji:~# dmesg | grep 'remapping'
[    0.048727] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[    0.050142] DMAR-IR: Enabled IRQ remapping in x2apic mode
```

List PCI devices:
```
root@fuji:~# 
root@fuji:~# lspci
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers (rev 05)
00:01.0 PCI bridge: Intel Corporation Skylake PCIe Controller (x16) (rev 05)
00:14.0 USB controller: Intel Corporation Sunrise Point-H USB 3.0 xHCI Controller (rev 31)
00:14.2 Signal processing controller: Intel Corporation Sunrise Point-H Thermal subsystem (rev 31)
00:16.0 Communication controller: Intel Corporation Sunrise Point-H CSME HECI #1 (rev 31)
00:16.1 Communication controller: Intel Corporation Sunrise Point-H CSME HECI #2 (rev 31)
00:17.0 SATA controller: Intel Corporation Sunrise Point-H SATA controller [AHCI mode] (rev 31)
00:1c.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #5 (rev f1)
00:1c.5 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #6 (rev f1)
00:1c.6 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #7 (rev f1)
00:1d.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #9 (rev f1)
00:1f.0 ISA bridge: Intel Corporation Sunrise Point-H LPC Controller (rev 31)
00:1f.2 Memory controller: Intel Corporation Sunrise Point-H PMC (rev 31)
00:1f.4 SMBus: Intel Corporation Sunrise Point-H SMBus (rev 31)
01:00.0 Non-Volatile memory controller: Phison Electronics Corporation E12 NVMe Controller (rev 01)
02:00.0 VGA compatible controller: Matrox Electronics Systems Ltd. MGA G200e [Pilot] ServerEngines (SEP1) (rev 05)
02:00.1 Co-processor: Emulex Corporation ServerView iRMC HTI
03:00.0 Ethernet controller: Intel Corporation I210 Gigabit Network Connection (rev 03)
04:00.0 Ethernet controller: Intel Corporation I210 Gigabit Network Connection (rev 03)
05:00.0 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
05:00.1 Ethernet controller: Intel Corporation I350 Gigabit Network Connection (rev 01)
```

After VGA install:
```
root@fuji:~# lspci
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers (rev 05)
00:01.0 PCI bridge: Intel Corporation Skylake PCIe Controller (x16) (rev 05)
00:14.0 USB controller: Intel Corporation Sunrise Point-H USB 3.0 xHCI Controller (rev 31)
00:14.2 Signal processing controller: Intel Corporation Sunrise Point-H Thermal subsystem (rev 31)
00:16.0 Communication controller: Intel Corporation Sunrise Point-H CSME HECI #1 (rev 31)
00:16.1 Communication controller: Intel Corporation Sunrise Point-H CSME HECI #2 (rev 31)
00:17.0 SATA controller: Intel Corporation Sunrise Point-H SATA controller [AHCI mode] (rev 31)
00:1c.0 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #5 (rev f1)
00:1c.5 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #6 (rev f1)
00:1c.6 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #7 (rev f1)
00:1c.7 PCI bridge: Intel Corporation Sunrise Point-H PCI Express Root Port #8 (rev f1)
00:1f.0 ISA bridge: Intel Corporation Sunrise Point-H LPC Controller (rev 31)
00:1f.2 Memory controller: Intel Corporation Sunrise Point-H PMC (rev 31)
00:1f.4 SMBus: Intel Corporation Sunrise Point-H SMBus (rev 31)
01:00.0 Non-Volatile memory controller: Phison Electronics Corporation E12 NVMe Controller (rev 01)
02:00.0 Display controller: Emulex Corporation ServerView iRMC HTI (rev 05)
02:00.1 Co-processor: Emulex Corporation ServerView iRMC HTI
03:00.0 Ethernet controller: Intel Corporation I210 Gigabit Network Connection (rev 03)
04:00.0 Ethernet controller: Intel Corporation I210 Gigabit Network Connection (rev 03)
05:00.0 VGA compatible controller: NVIDIA Corporation GK208 [GeForce GT 710B] (rev a1)
05:00.1 Audio device: NVIDIA Corporation GK208 HDMI/DP Audio Controller (rev a1)
```

Prevent host from using GT710:
```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf 
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf 
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf 
```

reboot

Note that GT710 (pci device 05:00:0x) has a dedicated iommu group:

```
root@fuji:~# find /sys/kernel/iommu_groups/ -type l
/sys/kernel/iommu_groups/7/devices/0000:00:1c.6
/sys/kernel/iommu_groups/5/devices/0000:00:1c.0
/sys/kernel/iommu_groups/13/devices/0000:05:00.1
/sys/kernel/iommu_groups/13/devices/0000:05:00.0
/sys/kernel/iommu_groups/3/devices/0000:00:16.0
/sys/kernel/iommu_groups/3/devices/0000:00:16.1
/sys/kernel/iommu_groups/11/devices/0000:03:00.0
/sys/kernel/iommu_groups/1/devices/0000:00:01.0
/sys/kernel/iommu_groups/1/devices/0000:01:00.0
/sys/kernel/iommu_groups/8/devices/0000:00:1c.7
/sys/kernel/iommu_groups/6/devices/0000:00:1c.5
/sys/kernel/iommu_groups/4/devices/0000:00:17.0
/sys/kernel/iommu_groups/12/devices/0000:04:00.0
/sys/kernel/iommu_groups/2/devices/0000:00:14.2
/sys/kernel/iommu_groups/2/devices/0000:00:14.0
/sys/kernel/iommu_groups/10/devices/0000:02:00.0
/sys/kernel/iommu_groups/10/devices/0000:02:00.1
/sys/kernel/iommu_groups/0/devices/0000:00:00.0
/sys/kernel/iommu_groups/9/devices/0000:00:1f.2
/sys/kernel/iommu_groups/9/devices/0000:00:1f.0
/sys/kernel/iommu_groups/9/devices/0000:00:1f.4
```

Edit VM Hardware - set:

* Machine: q35;
* Add EFI disk;
* BIOS: OVMF;
* Add PCI device, select GT710, check allfunctions, primary VGA, PCIe;
* Did not add associated HDMI audio yet

Start VM.  The above change in BIOS rendered the HD unbootable - I reinstalled centos8.

Then:
```
root@fuji:~# qm monitor 200
Entering Qemu Monitor for VM 200 - type 'help' for help
qm> info pci

```


