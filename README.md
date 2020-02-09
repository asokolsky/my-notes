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

### Apache
```
sudo yum install httpd
```

## Customize Keyboard: Swap CapsLock & Ctrl

In Tweaks: Keyabord & Mouse\ Ctrl Position \ Swap Ctrl & CapsLock

Alternatively:
```
/usr/bin/setxkbmap -option "ctrl:nocaps"
```
More: http://www.noah.org/wiki/CapsLock_Remap_Howto

## Apache Custmization

To start service:
```
sudo systemctl start httpd
```

To ensure httpd starts on power-up:
```
sudo systemctl enable httpd
```

Check status:
```
sudo systemctl status httpd
```

Modify firewal to allow for incoming HTTP and HTTPS:
```
sudo firewall-cmd ––permanent ––add-port=80/tcp
sudo firewall-cmd ––permanent ––add-port=443/tcp
sudo firewall-cmd ––reload
```

Each virtual host has a separate conf file in /etc/httpd/conf.d/.
Logs are in /var/www/html in a separate folders.

More:
https://phoenixnap.com/kb/install-apache-on-centos-7

https://www.tecmint.com/monitor-apache-web-server-load-and-page-statistics/

After reconfig do not forget:

```
httpd -t
sudo systemctl restart httpd
```
Apache status:
http://localhost/server-status?refresh=2

https://www.tecmint.com/monitor-apache-web-server-load-and-page-statistics/

Apache performance optimization:
http://httpd.apache.org/docs/current/misc/perf-tuning.html

https://serverfault.com/questions/383526/how-do-i-select-which-apache-mpm-to-use

To gracefully restart Apache:
```
apache2ctl -k graceful
```

## Kernel

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

## Java

It's all about GC. At least Java 7 update 51.

Choose Garbage First (G1) Collector

* set the heap size;
* set target GC pause time.

https://youtu.be/7dkSze52i-o?t=1771
