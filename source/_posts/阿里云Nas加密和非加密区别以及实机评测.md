---
title: 阿里云Nas加密和非加密区别以及实机评测
categories:
  - [技巧]
tags:
date: 2020-01-28 17:27:00
translate_title: alibaba-cloud-nas-encryption-and-non-encryption-differences-and-real-machine-evaluation
keywords:
description:
top_img: /2020/01/28/alibaba-cloud-nas-encryption-and-non-encryption-differences-and-real-machine-evaluation/Aliyun_Nas.png
comments:
cover: /2020/01/28/alibaba-cloud-nas-encryption-and-non-encryption-differences-and-real-machine-evaluation/Aliyun_Nas.png
toc:
toc_number:
copyright:
mathjax:
katex:
hide:
top:
---
*什么是文件存储NAS*
阿里云文件存储（Network Attached Storage，简称 NAS）是面向阿里云 ECS 实例、E-HPC 和容器服务等计算节点的文件存储服务。

定义
阿里云文件存储 NAS 是一个可共享访问、弹性扩展、高可靠以及高性能的分布式文件系统。它基于 POSIX 文件接口，天然适配原生操作系统，提供共享访问，同时保证数据一致性和锁互斥。

NAS 提供了简单的可扩展文件存储以供与 ECS 配合使用，多个ECS实例可以同时访问 NAS 文件系统，并且存储容量会随着您添加和删除文件而自动弹性增长和收缩，为在多个实例或服务器上运行的工作负载和应用程序提供通用数据源。

NAS 支持丰富的应用场景，详情请参见应用场景。

NAS 提供了容量型、性能型以及极速型存储类型，更多详情请参见存储类型。

产品优势
NAS 在成本、安全、简单、可靠性以及性能上都具有自身的优势。

成本
一个 NAS 文件系统可以同时挂载到多个计算节点上，由这些节点共享访问，从而节约大量拷贝与同步成本。
单个 NAS 文件系统的性能能够随存储容量线性扩展，使用户无需购买高端的文件存储设备，大幅降低硬件成本。
使用 NAS文件存储，您只需为文件系统使用的存储空间付费，不需要提前配置存储，并且不存在最低费用或设置费用。更多详情，请参见计量项和计费说明。
NAS 的高可靠性能够降低数据安全风险，从而大幅节约维护成本。
简单
一键创建文件系统，无需部署维护文件系统。

安全
基于 RAM 实现的资源访问控制，基于VPC实现的网络访问隔离，结合文件存储 NAS 的传输加密与存储加密特性，保障数据不被窃取或篡改。

高可靠性
NAS 提供 99.999999999% 的数据可靠性，能够有效降低数据安全风险。

高性能
基于分布式架构文件系统，随着容量的增长性能线性扩展，提供远高于传统存储的性能。

兼容性
NAS 文件存储提供良好的协议兼容性，支持 NFS 和 SMB 协议方案，兼容POSIX 文件系统访问语义，提供强大的数据一致性和文件锁定。
在 NAS 中，任何文件修改成功后，用户都能够立刻看到修改结果，便于用户实时修改存储内容。

<img src="Aliyun_Nas.png" width="100%" alt="Aliyun_Nas">

**test1为加密，test2为非加密**

test1 随机读IOPS设置
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randread -bs=4K -size=1G -time_based -runtime=60 -name=Fio -directory=/test1
Fio: (g=0): rw=randread, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Fio: Laying out IO file (1 file / 1024MiB)
Jobs: 1 (f=1): [r(1)][100.0%][r=97.8MiB/s][r=25.0k IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1482: Mon Jan 27 23:33:22 2020
  read: IOPS=24.1k, BW=94.3MiB/s (98.9MB/s)(5659MiB/60006msec)
    slat (nsec): min=1871, max=4225.7k, avg=8454.96, stdev=17814.51
    clat (usec): min=1150, max=66695, avg=5291.30, stdev=1830.86
     lat (usec): min=1157, max=66711, avg=5299.96, stdev=1830.83
    clat percentiles (usec):
     |  1.00th=[ 2409],  5.00th=[ 2933], 10.00th=[ 3294], 20.00th=[ 3785],
     | 30.00th=[ 4228], 40.00th=[ 4621], 50.00th=[ 5080], 60.00th=[ 5473],
     | 70.00th=[ 5997], 80.00th=[ 6587], 90.00th=[ 7504], 95.00th=[ 8455],
     | 99.00th=[10683], 99.50th=[11994], 99.90th=[17171], 99.95th=[19268],
     | 99.99th=[35914]
   bw (  KiB/s): min=87160, max=104926, per=99.98%, avg=96554.23, stdev=3373.87, samples=120
   iops        : min=21790, max=26231, avg=24138.51, stdev=843.44, samples=120
  lat (msec)   : 2=0.13%, 4=24.57%, 10=73.71%, 20=1.56%, 50=0.03%
  lat (msec)   : 100=0.01%
  cpu          : usr=5.74%, sys=15.74%, ctx=800135, majf=0, minf=137
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=1448757,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
   READ: bw=94.3MiB/s (98.9MB/s), 94.3MiB/s-94.3MiB/s (98.9MB/s-98.9MB/s), io=5659MiB (5934MB), run=60006-60006msec
```

test2不 随机读IOPS设置
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randread -bs=4K -size=1G -time_based -runtime=60 -name=Fio -directory=/test2
Fio: (g=0): rw=randread, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Fio: Laying out IO file (1 file / 1024MiB)
Jobs: 1 (f=1): [r(1)][100.0%][r=104MiB/s][r=26.5k IOPS][eta 00m:00s] 
Fio: (groupid=0, jobs=1): err= 0: pid=1494: Mon Jan 27 23:35:30 2020
  read: IOPS=25.7k, BW=100MiB/s (105MB/s)(6023MiB/60005msec)
    slat (nsec): min=1848, max=1328.1k, avg=8603.01, stdev=21240.04
    clat (usec): min=1126, max=152365, avg=4971.28, stdev=2067.07
     lat (usec): min=1132, max=152368, avg=4980.10, stdev=2067.06
    clat percentiles (usec):
     |  1.00th=[ 2245],  5.00th=[ 2671], 10.00th=[ 2999], 20.00th=[ 3458],
     | 30.00th=[ 3884], 40.00th=[ 4293], 50.00th=[ 4752], 60.00th=[ 5145],
     | 70.00th=[ 5669], 80.00th=[ 6259], 90.00th=[ 7111], 95.00th=[ 7963],
     | 99.00th=[10290], 99.50th=[11731], 99.90th=[17957], 99.95th=[22676],
     | 99.99th=[40633]
   bw (  KiB/s): min=86856, max=111976, per=99.98%, avg=102751.36, stdev=3685.76, samples=120
   iops        : min=21714, max=27994, avg=25687.74, stdev=921.31, samples=120
  lat (msec)   : 2=0.28%, 4=32.38%, 10=66.15%, 20=1.11%, 50=0.06%
  lat (msec)   : 100=0.01%, 250=0.01%
  cpu          : usr=5.96%, sys=15.47%, ctx=789134, majf=0, minf=138
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=1541761,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
   READ: bw=100MiB/s (105MB/s), 100MiB/s-100MiB/s (105MB/s-105MB/s), io=6023MiB (6315MB), run=60005-60005msec
```

test1随机写IOPS设置
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randwrite -bs=4K -size=1G -time_based -runtime=60 -name=Fio -directory=/test1
Fio: (g=0): rw=randwrite, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [w(1)][100.0%][w=75.7MiB/s][w=19.4k IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1498: Mon Jan 27 23:37:40 2020
  write: IOPS=18.2k, BW=71.2MiB/s (74.7MB/s)(4282MiB/60113msec); 0 zone resets
    slat (usec): min=2, max=616, avg= 7.55, stdev= 6.92
    clat (msec): min=2, max=232, avg= 7.01, stdev= 7.15
     lat (msec): min=2, max=232, avg= 7.02, stdev= 7.15
    clat percentiles (msec):
     |  1.00th=[    4],  5.00th=[    4], 10.00th=[    5], 20.00th=[    5],
     | 30.00th=[    5], 40.00th=[    6], 50.00th=[    6], 60.00th=[    7],
     | 70.00th=[    7], 80.00th=[    8], 90.00th=[   10], 95.00th=[   12],
     | 99.00th=[   36], 99.50th=[   47], 99.90th=[  116], 99.95th=[  140],
     | 99.99th=[  197]
   bw (  KiB/s): min=48336, max=85600, per=100.00%, avg=73062.90, stdev=7091.42, samples=120
   iops        : min=12084, max=21400, avg=18265.67, stdev=1772.83, samples=120
  lat (msec)   : 4=9.40%, 10=82.41%, 20=6.18%, 50=1.56%, 100=0.30%
  lat (msec)   : 250=0.14%
  cpu          : usr=5.55%, sys=14.95%, ctx=735321, majf=0, minf=10
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=0,1096310,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
  WRITE: bw=71.2MiB/s (74.7MB/s), 71.2MiB/s-71.2MiB/s (74.7MB/s-74.7MB/s), io=4282MiB (4490MB), run=60113-60113msec
```

test2随机写IOPS设置
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randwrite -bs=4K -size=1G -time_based -runtime=60 -name=Fio -directory=/test2
Fio: (g=0): rw=randwrite, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [w(1)][100.0%][w=79.6MiB/s][w=20.4k IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1503: Mon Jan 27 23:39:14 2020
  write: IOPS=19.1k, BW=74.5MiB/s (78.1MB/s)(4468MiB/60007msec); 0 zone resets
    slat (nsec): min=1997, max=791752, avg=7377.43, stdev=6986.23
    clat (msec): min=2, max=281, avg= 6.71, stdev= 7.43
     lat (msec): min=2, max=281, avg= 6.71, stdev= 7.43
    clat percentiles (msec):
     |  1.00th=[    4],  5.00th=[    4], 10.00th=[    4], 20.00th=[    5],
     | 30.00th=[    5], 40.00th=[    6], 50.00th=[    6], 60.00th=[    6],
     | 70.00th=[    7], 80.00th=[    8], 90.00th=[    9], 95.00th=[   11],
     | 99.00th=[   36], 99.50th=[   47], 99.90th=[  121], 99.95th=[  157],
     | 99.99th=[  220]
   bw (  KiB/s): min=46232, max=89360, per=99.99%, avg=76228.64, stdev=7455.48, samples=120
   iops        : min=11558, max=22340, avg=19057.13, stdev=1863.88, samples=120
  lat (msec)   : 4=11.69%, 10=81.90%, 20=4.37%, 50=1.60%, 100=0.31%
  lat (msec)   : 250=0.13%, 500=0.01%
  cpu          : usr=6.16%, sys=14.79%, ctx=714669, majf=0, minf=10
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=0,1143725,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
  WRITE: bw=74.5MiB/s (78.1MB/s), 74.5MiB/s-74.5MiB/s (78.1MB/s-78.1MB/s), io=4468MiB (4685MB), run=60007-60007msec
```

test1随机读吞吐
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randread -bs=1M -size=1G -time_based -runtime=60 -name=Fio -directory=/test1
Fio: (g=0): rw=randread, bs=(R) 1024KiB-1024KiB, (W) 1024KiB-1024KiB, (T) 1024KiB-1024KiB, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [r(1)][100.0%][r=1024KiB/s][r=1 IOPS][eta 00m:00s] 
Fio: (groupid=0, jobs=1): err= 0: pid=1511: Mon Jan 27 23:41:49 2020
  read: IOPS=119, BW=119MiB/s (125MB/s)(7292MiB/61170msec)
    slat (usec): min=54, max=3004, avg=163.83, stdev=196.42
    clat (msec): min=10, max=2494, avg=1072.78, stdev=189.89
     lat (msec): min=10, max=2495, avg=1072.94, stdev=189.86
    clat percentiles (msec):
     |  1.00th=[  241],  5.00th=[  902], 10.00th=[  961], 20.00th=[ 1028],
     | 30.00th=[ 1053], 40.00th=[ 1070], 50.00th=[ 1083], 60.00th=[ 1099],
     | 70.00th=[ 1116], 80.00th=[ 1133], 90.00th=[ 1200], 95.00th=[ 1250],
     | 99.00th=[ 1754], 99.50th=[ 2005], 99.90th=[ 2198], 99.95th=[ 2265],
     | 99.99th=[ 2500]
   bw (  KiB/s): min= 2048, max=247808, per=99.33%, avg=121252.40, stdev=27860.84, samples=121
   iops        : min=    2, max=  242, avg=118.36, stdev=27.22, samples=121
  lat (msec)   : 20=0.16%, 50=0.08%, 100=0.29%, 250=0.48%, 500=1.54%
  lat (msec)   : 750=0.37%, 1000=12.53%, 2000=84.01%, >=2000=0.53%
  cpu          : usr=0.08%, sys=1.19%, ctx=16345, majf=0, minf=32778
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.2%, 32=0.4%, >=64=99.1%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=7292,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
   READ: bw=119MiB/s (125MB/s), 119MiB/s-119MiB/s (125MB/s-125MB/s), io=7292MiB (7646MB), run=61170-61170msec
```

test2随机读吞吐
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randread -bs=1M -size=1G -time_based -runtime=60 -name=Fio -directory=/test2
Fio: (g=0): rw=randread, bs=(R) 1024KiB-1024KiB, (W) 1024KiB-1024KiB, (T) 1024KiB-1024KiB, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [r(1)][100.0%][r=11.0MiB/s][r=11 IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1514: Mon Jan 27 23:43:58 2020
  read: IOPS=119, BW=119MiB/s (125MB/s)(7304MiB/61174msec)
    slat (usec): min=52, max=5598, avg=183.95, stdev=270.59
    clat (msec): min=146, max=2256, avg=1071.05, stdev=171.09
     lat (msec): min=146, max=2257, avg=1071.23, stdev=171.04
    clat percentiles (msec):
     |  1.00th=[  317],  5.00th=[  902], 10.00th=[  961], 20.00th=[ 1020],
     | 30.00th=[ 1053], 40.00th=[ 1070], 50.00th=[ 1083], 60.00th=[ 1099],
     | 70.00th=[ 1116], 80.00th=[ 1133], 90.00th=[ 1200], 95.00th=[ 1234],
     | 99.00th=[ 1636], 99.50th=[ 1989], 99.90th=[ 2140], 99.95th=[ 2265],
     | 99.99th=[ 2265]
   bw (  KiB/s): min= 2048, max=288768, per=99.35%, avg=121466.95, stdev=25110.94, samples=121
   iops        : min=    2, max=  282, avg=118.60, stdev=24.52, samples=121
  lat (msec)   : 250=0.21%, 500=2.01%, 750=0.77%, 1000=13.81%, 2000=82.79%
  lat (msec)   : >=2000=0.41%
  cpu          : usr=0.09%, sys=1.19%, ctx=16454, majf=0, minf=32778
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.2%, 32=0.4%, >=64=99.1%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=7304,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
   READ: bw=119MiB/s (125MB/s), 119MiB/s-119MiB/s (125MB/s-125MB/s), io=7304MiB (7659MB), run=61174-61174msec
```

test1随机写吞吐
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randwrite -bs=1M -size=1G -time_based -runtime=60 -name=Fio -directory=/test1
Fio: (g=0): rw=randwrite, bs=(R) 1024KiB-1024KiB, (W) 1024KiB-1024KiB, (T) 1024KiB-1024KiB, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [w(1)][100.0%][w=28.0MiB/s][w=28 IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1522: Mon Jan 27 23:48:33 2020
  write: IOPS=127, BW=127MiB/s (134MB/s)(7771MiB/61014msec); 0 zone resets
    slat (usec): min=104, max=941, avg=172.70, stdev=37.40
    clat (msec): min=55, max=2021, avg=1004.46, stdev=156.46
     lat (msec): min=55, max=2021, avg=1004.64, stdev=156.46
    clat percentiles (msec):
     |  1.00th=[  150],  5.00th=[  986], 10.00th=[ 1011], 20.00th=[ 1020],
     | 30.00th=[ 1020], 40.00th=[ 1020], 50.00th=[ 1020], 60.00th=[ 1020],
     | 70.00th=[ 1020], 80.00th=[ 1028], 90.00th=[ 1028], 95.00th=[ 1045],
     | 99.00th=[ 1401], 99.50th=[ 1720], 99.90th=[ 1972], 99.95th=[ 2005],
     | 99.99th=[ 2022]
   bw (  KiB/s): min= 2048, max=370688, per=99.19%, avg=129364.72, stdev=25090.44, samples=121
   iops        : min=    2, max=  362, avg=126.29, stdev=24.51, samples=121
  lat (msec)   : 100=0.23%, 250=1.90%, 500=0.48%, 750=0.50%, 1000=3.53%
  lat (msec)   : 2000=93.31%, >=2000=0.05%
  cpu          : usr=1.07%, sys=1.22%, ctx=8135, majf=0, minf=9
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.2%, 32=0.4%, >=64=99.2%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=0,7771,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
  WRITE: bw=127MiB/s (134MB/s), 127MiB/s-127MiB/s (134MB/s-134MB/s), io=7771MiB (8148MB), run=61014-61014msec
```

test2随机写吞吐
```
[root@super /]# fio -numjobs=1 -iodepth=128 -direct=1 -ioengine=libaio -sync=1 -rw=randwrite -bs=1M -size=1G -time_based -runtime=60 -name=Fio -directory=/test2
Fio: (g=0): rw=randwrite, bs=(R) 1024KiB-1024KiB, (W) 1024KiB-1024KiB, (T) 1024KiB-1024KiB, ioengine=libaio, iodepth=128
fio-3.17
Starting 1 process
Jobs: 1 (f=1): [w(1)][100.0%][w=28.0MiB/s][w=28 IOPS][eta 00m:00s]
Fio: (groupid=0, jobs=1): err= 0: pid=1533: Mon Jan 27 23:51:47 2020
  write: IOPS=127, BW=127MiB/s (134MB/s)(7772MiB/61020msec); 0 zone resets
    slat (usec): min=101, max=1754, avg=173.58, stdev=42.61
    clat (msec): min=52, max=2019, avg=1004.43, stdev=161.16
     lat (msec): min=52, max=2019, avg=1004.60, stdev=161.16
    clat percentiles (msec):
     |  1.00th=[  133],  5.00th=[  894], 10.00th=[ 1003], 20.00th=[ 1011],
     | 30.00th=[ 1020], 40.00th=[ 1020], 50.00th=[ 1020], 60.00th=[ 1020],
     | 70.00th=[ 1020], 80.00th=[ 1028], 90.00th=[ 1028], 95.00th=[ 1070],
     | 99.00th=[ 1401], 99.50th=[ 1720], 99.90th=[ 1972], 99.95th=[ 2005],
     | 99.99th=[ 2022]
   bw (  KiB/s): min= 2048, max=371992, per=99.20%, avg=129375.29, stdev=26928.24, samples=121
   iops        : min=    2, max=  363, avg=126.30, stdev=26.28, samples=121
  lat (msec)   : 100=0.39%, 250=1.71%, 500=0.49%, 750=0.46%, 1000=5.96%
  lat (msec)   : 2000=90.97%, >=2000=0.03%
  cpu          : usr=1.31%, sys=0.98%, ctx=8027, majf=0, minf=10
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.2%, 32=0.4%, >=64=99.2%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.1%
     issued rwts: total=0,7772,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=128

Run status group 0 (all jobs):
  WRITE: bw=127MiB/s (134MB/s), 127MiB/s-127MiB/s (134MB/s-134MB/s), io=7772MiB (8150MB), run=61020-61020msec
```


----------
可以看出还是非加密好一点😁😁😁


  