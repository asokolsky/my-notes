# Customizing New CENTOS Installation

## Update

```
sudo yum update
```

## Install Tweaks

## Keyboard: Swap CapsLock & Ctrl

In Tweaks: Keyabord & Mouse\ Ctrl Position \ Swap Ctrl & CapsLock

Alternatively:
```
/usr/bin/setxkbmap -option "ctrl:nocaps"
```
More: http://www.noah.org/wiki/CapsLock_Remap_Howto

## Install Emacs

```
sudo yum install epel-release
sudo yum install emacs
```

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

Modify firewal to allow for incoming HTTP and HTTPS.
```
sudo firewall-cmd ––permanent ––add-port=80/tcp
sudo firewall-cmd ––permanent ––add-port=443/tcp
sudo firewall-cmd ––reload
```
## Configure Apache

Each virtual host is a separate conf file in /etc/httpd/conf.d/

More: https://phoenixnap.com/kb/install-apache-on-centos-7

