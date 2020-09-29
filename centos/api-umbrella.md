# Install API Umbrella on CentOS7

Follow instructions from https://apiumbrella.io/install/

```
[alex@localhost tmp]$ curl https://bintray.com/nrel/api-umbrella-el7/rpm | sudo tee /etc/yum.repos.d/api-umbrella.repo
[alex@localhost tmp]$ sudo yum install api-umbrella

Dependencies Resolved

===========================================================================================================================================
 Package                               Arch             Version                             Repository                                Size
===========================================================================================================================================
Installing:
 api-umbrella                          x86_64           0.15.1-1.el7                        bintray--nrel-api-umbrella-el7           151 M
Installing for dependencies:
 copy-jdk-configs                      noarch           3.3-10.el7_5                        base                                      21 k
 java-1.8.0-openjdk-headless           x86_64           1:1.8.0.262.b10-0.el7_8             updates                                   33 M
 javapackages-tools                    noarch           3.4.1-11.el7                        base                                      73 k
 libgcrypt-devel                       x86_64           1.5.3-14.el7                        base                                     129 k
 libgpg-error-devel                    x86_64           1.12-3.el7                          base                                      16 k
 libxml2-devel                         x86_64           2.9.1-6.el7.4                       base                                     1.0 M
 libxslt                               x86_64           1.1.28-5.el7                        base                                     242 k
 libxslt-devel                         x86_64           1.1.28-5.el7                        base                                     309 k
 libyaml                               x86_64           0.1.4-11.el7_0                      base                                      55 k
 lksctp-tools                          x86_64           1.0.17-2.el7                        base                                      88 k
 pcsc-lite-libs                        x86_64           1.8.8-8.el7                         base                                      34 k
 python-javapackages                   noarch           3.4.1-11.el7                        base                                      31 k
 python-lxml                           x86_64           3.2.1-4.el7                         base                                     758 k
 tcl                                   x86_64           1:8.5.13-8.el7                      base                                     1.9 M
 tzdata-java                           noarch           2020a-1.el7                         updates                                  188 k
 xz-devel                              x86_64           5.2.2-1.el7                         base                                      46 k

Transaction Summary
===========================================================================================================================================
Install  1 Package (+16 Dependent packages)

Total download size: 189 M
Installed size: 820 M
Is this ok [y/d/N]: y
...
Installed:
  api-umbrella.x86_64 0:0.15.1-1.el7

Dependency Installed:
  copy-jdk-configs.noarch 0:3.3-10.el7_5      java-1.8.0-openjdk-headless.x86_64 1:1.8.0.262.b10-0.el7_8      javapackages-tools.noarch 0:3.4.1-11.el7
  libgcrypt-devel.x86_64 0:1.5.3-14.el7       libgpg-error-devel.x86_64 0:1.12-3.el7                          libxml2-devel.x86_64 0:2.9.1-6.el7.4
  libxslt.x86_64 0:1.1.28-5.el7               libxslt-devel.x86_64 0:1.1.28-5.el7                             libyaml.x86_64 0:0.1.4-11.el7_0
  lksctp-tools.x86_64 0:1.0.17-2.el7          pcsc-lite-libs.x86_64 0:1.8.8-8.el7                             python-javapackages.noarch 0:3.4.1-11.el7
  python-lxml.x86_64 0:3.2.1-4.el7            tcl.x86_64 1:8.5.13-8.el7                                       tzdata-java.noarch 0:2020a-1.el7
  xz-devel.x86_64 0:5.2.2-1.el7

Complete!

[alex@localhost tmp]$ sudo /etc/init.d/api-umbrella start
Starting api-umbrella (via systemctl):                     [  OK  ]
```
