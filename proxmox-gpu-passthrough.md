# Proxmox GPU Pass Through

Sources:

* https://pve.proxmox.com/wiki/Pci_passthrough
* https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/
* https://www.youtube.com/watch?reload=9&v=fgx3NMk6F54

## Grub Kernel Boot Command Line

Edit grub for IOMMU, update grub, reboot, then:

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

## VFIO Modules

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
## IOMMU Interrupt Remapping

Verify remapping is enabled:

```
root@fuji:~# dmesg | grep 'remapping'
[    0.048727] DMAR-IR: Queued invalidation will be enabled to support x2apic and Intr-remapping.
[    0.050142] DMAR-IR: Enabled IRQ remapping in x2apic mode
```

List PCI devices:
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

After VGA install and on another PC:
```
root@duo:~# lspci
00:00.0 Host bridge: Intel Corporation 8th Gen Core Processor Host Bridge/DRAM Registers (rev 07)
00:01.0 PCI bridge: Intel Corporation Skylake PCIe Controller (x16) (rev 07)
00:02.0 Display controller: Intel Corporation UHD Graphics 630 (Desktop)
00:14.0 USB controller: Intel Corporation 200 Series/Z370 Chipset Family USB 3.0 xHCI Controller
00:14.2 Signal processing controller: Intel Corporation 200 Series PCH Thermal Subsystem
00:16.0 Communication controller: Intel Corporation 200 Series PCH CSME HECI
00:17.0 SATA controller: Intel Corporation 200 Series PCH SATA controller [AHCI mode]
00:1c.0 PCI bridge: Intel Corporation 200 Series PCH PCI Express Root Port (rev f0)
00:1c.5 PCI bridge: Intel Corporation 200 Series PCH PCI Express Root Port (rev f0)
00:1d.0 PCI bridge: Intel Corporation 200 Series PCH PCI Express Root Port (rev f0)
00:1f.0 ISA bridge: Intel Corporation Z370 Chipset LPC/eSPI Controller
00:1f.2 Memory controller: Intel Corporation 200 Series/Z370 Chipset Family Power Management Controller
00:1f.3 Audio device: Intel Corporation 200 Series PCH HD Audio
00:1f.4 SMBus: Intel Corporation 200 Series/Z370 Chipset Family SMBus Controller
00:1f.6 Ethernet controller: Intel Corporation Ethernet Connection (2) I219-V
01:00.0 VGA compatible controller: NVIDIA Corporation GP108 (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP108 High Definition Audio Controller (rev a1)
03:00.0 Ethernet controller: Intel Corporation I211 Gigabit Network Connection (rev 03)
04:00.0 Non-Volatile memory controller: Sandisk Corp Device 5009 (rev 01)
root@duo:~# lspci -n -s 01:00
01:00.0 0300: 10de:1d01 (rev a1)
01:00.1 0403: 10de:0fb8 (rev a1)
```

The latter is important.  


## Blacklist Drivers

Prevent host from using GT710:
```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf 
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf 
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf 
```

reboot

# Add GPU to VFIO

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

Use vendor ids for the GPU determined before:

```
root@duo:~# lspci -n -s 01:00
01:00.0 0300: 10de:1d01 (rev a1)
01:00.1 0403: 10de:0fb8 (rev a1)
root@duo:~# cat /etc/modprobe.d/vfio.conf
cat: /etc/modprobe.d/vfio.conf: No such file or directory
root@duo:~# echo "options vfio-pci ids=10de:1d01,10de:0fb8" > /etc/modprobe.d/vfio.conf
root@duo:~# cat /etc/modprobe.d/vfio.conf
options vfio-pci ids=10de:1d01,10de:0fb8
```

# VM Configuration

Edit VM Hardware - set:

* BIOS: OVMF;
* Display: default
* Machine: q35;
* Add EFI disk;

Then add PCIe GPU:

* select GT710,
* check allfunctions,
* checl primary VGA, PCIe;
* Did not add associated HDMI audio yet

Connection to console did not work, so peripherals add: add USB port.


Start VM.  The above change in BIOS rendered the HD unbootable - I reinstalled centos8.

Then:
```
root@fuji:~# qm monitor 200
Entering Qemu Monitor for VM 200 - type 'help' for help
qm> info pci

```


