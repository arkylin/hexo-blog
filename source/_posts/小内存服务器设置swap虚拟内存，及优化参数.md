---
title: 小内存服务器设置swap虚拟内存，及优化参数
categories:
  - [技巧]
  - [常用配置]
tags:
  - [小内存]
  - [Swap]
  - [虚拟内存]
  - [优化参数]
date: 2019-08-08 17:18:57
translate_title: setting-up-swap-virtual-memory-on-small-memory-server-and-optimizing-parameters
keywords:
description:
top_img: /2019/08/08/setting-up-swap-virtual-memory-on-small-memory-server-and-optimizing-parameters/swap.png
comments:
cover: /2019/08/08/setting-up-swap-virtual-memory-on-small-memory-server-and-optimizing-parameters/swap.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
避免应用程序内存不足错误的最简单方法之一是为服务器添加一些Swap空间。Swap分区在系统的物理内存不够用的时候，把物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap分区中，等到那些程序要运行时，再从Swap分区中恢复保存的数据到内存中。

注意：虽然我们建议对使用传统机械硬盘驱动器的系统进行交换，但对于SSD来说，使用Swap可能会导致硬件随着时间的推移而出现问题。出于这种考虑，我们不建议在任何其他使用SSD存储上启用Swap。这样做会影响底层硬件的可靠性。本教程更建议采用传统机械硬盘的用户使用。

**第一步、检查系统的交换信息**
我们可以先来检查一下系统是否已经有可用的Swap空间。我们可以有多个Swap文件或Swap分区，但通常来讲一个便足够了。

我们可以通过输入以下内容来查看系统是否已配置Swap：
```
sudo swapon --show
```
如果您没有收到任何输出，这意味着您的系统当前没有可用的Swap空间。

您可以使用free命令证有没有活动的swap：
```
free -h
              total        used        free      shared  buff/cache   available
Mem:           985M         84M        222M        680K        678M        721M
Swap:            0B          0B          0B
```
正如您在输出的Swap行中所看到的，系统上没有正在活动的swap。

**第二步、检查硬盘驱动器分区上的可用空间**
在我们创建swap文件之前，我们将检查当前的磁盘使用情况，以确保我们有足够的空间。通过输入：
<img src="swap.png" width="100%" alt="Swap">
```
df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            481M     0  481M   0% /dev
tmpfs            99M  656K   98M   1% /run
/dev/vda1        25G  1.4G   23G   6% /
tmpfs           493M     0  493M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           493M     0  493M   0% /sys/fs/cgroup
/dev/vda15      105M  3.4M  102M   4% /boot/efi
tmpfs            99M     0   99M   0% /run/user/1000
```
Mounted on列中的```/dev/vda1```是我们的磁盘。在这个例子中我们有足够的空间（仅有1.4G被使用）。

对swap空间的适当大小实际上取决于您的个人偏好和您的应用程序要求。通常，等于或双倍于系统内存的量是一个很好的选择。如果您只是将其用作RAM后备，那么你的swap分区尽可能不要超过4G。

**第三步、创建Swap分区文件**
我们可以在文件系统上创建一个swap分区。我们将在根/目录中分配我们想要调用的swap大小的文件swapfile。

创建交换文件的最佳方法是使用fallocate。此命令将创建指定大小的文件。

由于我们示例中的服务器具有1G的RAM，因此我们将在本教程中创建1G文件：
```
sudo fallocate -l 1G /swapfile
```
我们可以通过输入以下内容来验证其是否保留了正确的空间量：
```
ls -lh /swapfile
-rw-r--r-- 1 root root 1.0G Apr 25 11:14 /swapfile
```
**第四步、启用Swap分区**
我们需要将swap文件转换为swap分区。首先，我们需要锁定文件的权限，以便只有具有root权限的用户才能读取内容。

通过输入以下内容使该文件只能由root访问：
```
sudo chmod 600 /swapfile
```
输入以下命令验证权限更改：
```
ls -lh /swapfile
-rw------- 1 root root 1.0G Apr 25 11:14 /swapfile
```
如您所见，只有root用户启用了读写标志。

我们现在可以通过输入以下内容将文件标记为swap空间
```
sudo mkswap /swapfile
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=6e965805-2ab9-450f-aed6-577e74089dbf
```
标记文件后，我们可以启用swap文件：
```
sudo swapon /swapfile
```
输入以下内容验证交换是否可用：
```
sudo swapon --show
NAME      TYPE  SIZE USED PRIO
/swapfile file 1024M   0B   -2
```
我们可以用free命令再次检查：
```
free -h
              total        used        free      shared  buff/cache   available
Mem:           985M         84M        220M        680K        680M        722M
Swap:          1.0G          0B        1.0G
```
我们的swap已成功设置，我们的操作系统将在必要时使用它。

**第五步、永久化swap文件**
我们已启用当前会话中的swap文件。但是，如果我们重新启动的话，服务器将不会自动保留swap设置。我们可以通过将交换文件添加到我们的```/etc/fstab```来进行更改。

为了避免出现任何问题，先备份```/etc/fstab```文件：
```
sudo cp /etc/fstab /etc/fstab.bak
```
输入以下内容，将swap文件信息添加到```/etc/fstab```文件末尾：
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
接下来，我们将调整我们的swap空间。

**第六步、调整您的Swap设置**
您可以配置一些选项，这些选项会在处理swap时对系统的性能产生影响。

调整Swappiness参数
该swappiness参数主要配置系统将数据从RAM交换到交换空间的频率。该参数的值是介于0和100之间的百分比。

当值接近于零时，除非绝对必要，否则内核不会将数据交换到磁盘。请记住，与swap文件的交互是“费时的”，因为它们比与RAM的交互花费更长的时间，并且它们可能导致性能的显着降低。

当该值接近100时，其将尝试将更多数据放入交换中以努力保留更多的RAM空间。我们可以通过输入以下内容来查看当前的swappiness值：
```
cat /proc/sys/vm/swappiness
60
```
对于服务器本身来说，您可能希望这个数值更接近于0。我们可以使用```sysctl```命令将swappiness设置为不同的值。

例如，要将swappiness设置为10，我们可以输入：
```
sudo sysctl vm.swappiness=10
vm.swappiness = 10
```
我们可以通过在```/etc/sysctl.conf```文件中添加以下行来自动设置此值：
```
sudo nano /etc/sysctl.conf
```
在底部，您可以添加：
```
vm.swappiness=10
```
完成后保存并关闭文件。

调整缓存压力设置
您可能想要修改的另一个相关值是```vfs_cache_pressure```。这将关系到系统选择多少缓存inode和dentry信息。

您可以通过proc命令来查看当前值：
```
cat /proc/sys/vm/vfs_cache_pressure
100
```
我的设置系统会很快地从缓存中删除inode信息。我们可以通过输入以下内容将其设置为更保守的值（如50）更快就200：
```
sudo sysctl vm.vfs_cache_pressure=50
vm.vfs_cache_pressure = 50
```
同样，这仅适用于我们当前的会话。我们可以通过将其添加到配置文件来改变它，就像我们使用swappiness设置一样：
```
sudo nano /etc/sysctl.conf
```
在底部，添加指定新值的行：
```
vm.vfs_cache_pressure=50
```
完成后保存并关闭文件。

结论
本教程将为您提供一些额外的空间来避免内存不足的异常。swap空间可以避免一些常见问题。如果遇到内存不足错误，或者发现系统无法使用所需的应用程序，您的最佳解决方案是优化应用程序配置或升级服务器。

转自：https://cloud.tencent.com/developer/article/1165387修改了部分