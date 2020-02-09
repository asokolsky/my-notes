# Customizing CENTOS Install

## Update the Install

```
sudo yum update
sudo yum upgrade
```
## Add EPEL repo

```
sudo yum -y install epel-release
sudo yum repolist
```

## Install Favorite Packages

```
sudo yum search htop
sudo yum install htop
sudo yum install emacs
```

htop keyboard commands: s, l, H
https://www.cyberciti.biz/faq/how-to-install-htop-on-centos-linux-8/

## Install Tweaks

In GUI:
Software \ Search tweaks \ GNOME Tweaks \ Install

In CLI:

```
sudo yum install gnome-tweak-tool
```
More: https://linuxhint.com/tweaking_gnome_desktop_centos8/

## Keyboard: Swap CapsLock & Ctrl

In Tweaks: Keyabord & Mouse\ Ctrl Position \ Swap Ctrl & CapsLock

Alternatively:
```
/usr/bin/setxkbmap -option "ctrl:nocaps"
```
More: http://www.noah.org/wiki/CapsLock_Remap_Howto

## Install Apache
```
sudo yum install httpd
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
## Configure Apache

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


