# 全面了解Linux服务器

### 1.查看CPU的详细情况
* 物理CPU的个数：`[root@WX202 ~]# cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l`
* 每个物理CPU中core的个数(核数)：`[root@WX202 ~]# cat /proc/cpuinfo | grep "cpu cores" | uniq`
* 逻辑CPU的个数：`[root@WX202 ~]# cat /proc/cpuinfo | grep "processor" | wc -l`

### 2.查看内存使用情况
* `[root@WX202 ~]# free -m`


						total	used       free     shared    buffers     cached
		Mem:         32168      31675        492          0        404      15574
		-/+ buffers/cache:      15696      16471
		Swap:        31996        620      31375

	在观察linux的内存使用情况时，只要没发现用swap的交换空间，就不用担心内存太少，但若常常看到swap用了很多，那么就要考虑增加物理内存了，这也是常在linux服务器上看内存是否够用的标准。

### 3.查看硬盘使用情况
* 查看硬盘及分区信息：`[root@WX202 ~]# fdisk -l`
* 检查文件系统的磁盘占用情况：`[root@WX202 ~]# df -h`
* 查看硬盘的I/O性能：``