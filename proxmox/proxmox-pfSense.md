# pfSense VM in Proxmox

## Sources

https://pfstore.com.au/blogs/guides/run-pfsense-in-proxmox

## pfSense VM Setup

Defaults are safe.  To make it better:

* CPU - enable pdpe1gb, aes
* Hard Disk\IO Thread enabled
* CPU - 1 OK
* RAM - 1GB
* Network - either bridge or PCIe devise passed through.

Either way:
* Firewall - disabled
* Model - VirtIO (paravirtualized)
* Multiqueue - 8

After VM created - add another NIC.

## pfSense Setup

System\Advanced\Networking

Disable all the offloading

