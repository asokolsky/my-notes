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

## Install Tweaks

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

More: https://phoenixnap.com/kb/install-apache-on-centos-7

After reconfig do not forget:

```
sudo systemctl restart httpd
```
Apache status: https://www.tecmint.com/monitor-apache-web-server-load-and-page-statistics/
