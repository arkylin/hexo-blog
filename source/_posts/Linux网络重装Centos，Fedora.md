---
title: Linux网络重装Centos，Fedora
categories:
  - [常用配置]
  - [技巧]
tags:
date: 2019-08-17 17:23:00
translate_title: linux-network-reload-centos-fedora
keywords:
description:
top_img: 
comments:
cover: 
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
```
wget -O /boot/initrd.img http://mirror.centos.org/centos/7/os/x86_64/images/pxeboot/initrd.img
wget -O /boot/vmlinuz http://mirror.centos.org/centos/7/os/x86_64/images/pxeboot/vmlinuz
cp /boot/initrd.img /
cp /boot/vmlinuz /
```
```
nano /etc/grub.d/40_custom
```
```
menuentry "InstallCentOS7" {
	set root='(hd0,msdos1)'
	linux /vmlinuz repo=http://mirror.centos.org/centos/7/os/x86_64/ ip=45.129.2.183 netmask=255.255.254.0 gateway=45.129.2.1 nameserver=1.1.1.1
	initrd /initrd.img
}
```
ip可以直接写```ip=dhcp```也可以，如果是用dncp的话
```
grub2-mkconfig --output=/boot/grub2/grub.cfg
grub2-set-default InstallCentOS7
```
或
```
grub-mkconfig --output=/boot/grub/grub.cfg
grub-set-default InstallCentOS7
```

**Fedora同理**
```
wget -O /boot/initrd.img https://dl.fedoraproject.org/pub/fedora/linux/development/rawhide/Everything/x86_64/os/images/pxeboot/initrd.img
wget -O /boot/vmlinuz https://dl.fedoraproject.org/pub/fedora/linux/development/rawhide/Everything/x86_64/os/images/pxeboot/vmlinuz
cp /boot/initrd.img /
cp /boot/vmlinuz /
```
```
nano /etc/grub.d/40_custom
```
```
menuentry "InstallFedora30" {
	set root='(hd0,msdos1)'
	linux /vmlinuz repo=https://dl.fedoraproject.org/pub/fedora/linux/development/rawhide/Everything/x86_64/os/ ip=dhcp
	initrd /initrd.img
}
```
注：这是rawhide版，也就相当于测试版，如需正式版，自己找
```
grub2-mkconfig --output=/boot/grub2/grub.cfg
grub2-set-default InstallFedora30
```

如果系统是grub的话
```
grub-mkconfig --output=/boot/grub/grub.cfg
grub-set-default InstallCentOS7
```
即可