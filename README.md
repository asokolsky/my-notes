# Customizing CENTOS Install

## Update the Install

```
sudo yum update
sudo yum upgrade
```
## Add EPEL repo

Add EPEL repo:

```
sudo yum -y install epel-release
sudo yum repolist
```

## Install Favorite Packages

### Emacs

```
sudo yum install emacs
```

### HTOP

```
sudo yum search htop
sudo yum install htop
```

htop keyboard commands: s, l, H

https://www.cyberciti.biz/faq/how-to-install-htop-on-centos-linux-8/

### Gnome Tweaks

In GUI:
Software \ Search tweaks \ GNOME Tweaks \ Install

In CLI:

```
sudo yum install gnome-tweak-tool
```
More: https://linuxhint.com/tweaking_gnome_desktop_centos8/

## Customize Keyboard: Swap CapsLock & Ctrl

In Tweaks: Keyabord & Mouse\ Ctrl Position \ Swap Ctrl & CapsLock

Alternatively:
```
/usr/bin/setxkbmap -option "ctrl:nocaps"
```
More: http://www.noah.org/wiki/CapsLock_Remap_Howto

## Kernel Tuning

To make those changes persisent, edit  /etc/sysctl.conf

### No Swapping

To set it:

```
sudo sysctl vm.swappiness=10

```
Check it
```
cat /proc/sys/vm/swappiness
```

### Adjusting Cache

For iO serer is makes sense allow more dirty pages, less dirty cache:

https://youtu.be/7dkSze52i-o?t=1607

https://lonesysadmin.net/2013/12/22/better-linux-disk-caching-performance-vm-dirty_ratio/

```
vm.dirty_background_ratio = 5
vm.dirty_ratio = 80
```

## Java Tuning

It's all about GC. At least Java 7 update 51.

Choose Garbage First (G1) Collector

* set the heap size;
* set target GC pause time.

https://youtu.be/7dkSze52i-o?t=1771

