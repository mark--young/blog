##如何在Linux中添加Swap

###Swap是什么？
当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap中恢复保存的数据到内存中。这样，系统总是在物理内存不够时，才进行Swap交换。

实际问题：
使用 [Vultr](http://www.vultr.com/?ref=6862890) 有很长一段时间了，前几天在编译 PHP 的时候出现了进程被 killed 的状况，经过我的吐槽以及和别人交流后发现，是内存耗尽的缘故。其实是因为当时开着 MySQL 进程消耗了不少内存，后来觉得有必要手动添加一下 Swap（交换分区），这样以免以后再编译什么的时候进程被K 。
关于 Linux 中 Swap（交换分区），类似于 Windows 的虚拟内存，就是当内存不足的时候，把一部分硬盘空间虚拟成内存使用,从而解决内存容量不足的情况。
那么如何在 Linux 中手动添加 Swap 呢？

下面给大家介绍

### 检查Swap
在设置 Swap 文件之前，有必要先检查一下系统里有没有既存的 Swap 文件。
运行以下命令：

    swapon -s

如果返回的信息概要是空的，则表示 Swap 文件不存在。

### 检查文件系统

在设置 Swap 文件之前，同样有必要检查一下文件系统，看看是否有足够的硬盘空间来设置 Swap 。运行以下命令：

    df -hal

检查返回的信息，还剩余足够的硬盘空间即可。

### 创建Swap文件

一般来说可以按照如下规则设置swap大小：

4G以内的物理内存，SWAP 设置为内存的2倍。
4-8G的物理内存，SWAP 等于内存大小。
8-64G 的物理内存，SWAP 设置为8G。
64-256G物理内存，SWAP 设置为16G。

实际上，系统中交换分区的大小并不取决于物理内存的量，而是取决于系统中内存的负荷，所以在安装系统时要根据具体的业务来设置SWAP的值。

像我自己的Blog，内存有768M，我想用1G的Swap就应该够了。

下面使用 `dd` 命令来创建 Swap 文件。

    dd if=/dev/zero of=/tmp/swapfile bs=1024 count=1024k

打印信息：
    1048576+0 records in
    1048576+0 records out
    1073741824 bytes (1.1 GB) copied, 3.23377 s, 332 MB/s

**参数解读：**

* if=文件名：输入文件名，缺省为标准输入。即指定源文件。< if=input file >
* of=文件名：输出文件名，缺省为标准输出。即指定目的文件。< of=output file >
* bs=bytes：同时设置读入/输出的块大小为bytes个字节
* count=blocks：仅拷贝blocks个块，块大小等于bs指定的字节数。

###格式化并激活 Swap 文件
上面已经创建好 Swap 文件，还需要格式化后才能使用。运行命令：

    mkswap /tmp/swapfile

激活 Swap ，运行命令：

    swapon /tmp/swapfile

以上步骤做完，再次运行命令：

    swapon -s

你会发现返回的信息概要：

    Filename                Type            Size      Used    Priority
    /tmp/swapfile           file            1048572   0       -1

### 系统在什么情况下才会使用SWAP？

实际上，并不是等所有的物理内存都消耗完毕之后，才去使用swap的空间，什么时候使用是由swappiness 参数值控制。

    cat /proc/sys/vm/swappiness

该值默认值是60。

* swappiness=0的时候表示最大限度使用物理内存，然后才是 swap空间，
* swappiness＝100的时候表示积极的使用swap分区，并且把内存上的数据及时的搬运到swap空间里面。    

现在服务器的内存动不动就是上百G，所以我们可以把这个参数值设置的低一些，让操作系统尽可能的使用物理内存，降低系统对swap的使用，从而提高系统的性能。

### 如何修改swappiness参数？
####1.临时修改：

使用 sysctl 命令：
    
    sysctl vm.swappiness=10

但是这只是**临时性**的修改，在你重启系统后会恢复默认的60。

####2.永久修改：

要永久设置，还需要在 vim 中修改sysctl.conf：

    vi /etc/sysctl.conf

在这个文档的最后加上这样一行:

    vm.swappiness=10

输入`:wq`，保存退出 vim 。

### 管理SWAP相关命令

####1.释放Swap空间

假设我们的系统出现了性能问题，我们通过vmstat命令看到有大量的swap，而我们的物理内存又很充足，那么我们可以手工把swap 空间释放出来。让进程去使用物理内存，从而提高性能。

    vmstat 1 5

打印信息：

    root@vultr:~# vmstat 1 5
    procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
     r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
     0  0      0  61112  29756 324592    0    0    16    19   33   20  3  0 97  0
     0  0      0  61112  29756 324592    0    0     0     0  260  550  0  0 100  0
     0  0      0  61112  29756 324592    0    0     0     0  255  541  0  0 99  0
     0  0      0  61112  29756 324592    0    0     0     0  256  540  0  0 100  0
     0  0      0  61112  29760 324588    0    0     0    60  260  552  0  0 98  1

    free -m

打印信息：

                 total       used       free     shared    buffers     cached
    Mem:           755        696         59          0         29        316
    -/+ buffers/cache:        350        405
    Swap:         1023          0       1023


注意：free命令默认单位为`k`, `-m` 单位为`M`。 我们这里的swap使用了0M的空间，可能是因为刚刚设置的Swap。

####2.查看当前swap 的使用

查看swap使用：

    swapon -s

或：

    cat /proc/swaps

之前使用过，就不介绍了。

####3.启用swap
使用swapon命令： 格式`swapon [swap filename]`

    swapon /dev/sda2

####4.关闭swap

使用swapoff命令：格式`swapoff [swap filename]`

    swapoff /dev/sda2

之后可以使用`swapon -s`进行查看。

####5.查看开机启动

使用`cat /etc/fstab`：

    root@vultr:~# cat /etc/fstab
    # /etc/fstab: static file system information.
    #
    # Use 'blkid' to print the universally unique identifier for a
    # device; this may be used with UUID= as a more robust way to name devices
    # that works even if disks are added and removed. See fstab(5).
    #
    # <file system> <mount point>   <type>  <options>       <dump>  <pass>
    # / was on /dev/vda1 during installation
    UUID=448a3d02-379e-446b-a1c5-27172ef46473 /               ext4    errors=remount-ro 0       1
    /dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
    /dev/fd0        /media/floppy0  auto    rw,user,noauto  0       0
    /tmp/swapfile    swap     swap    defaults        0 0

说明：

（1）ext分区是否启用由`mount`及`umount`控制。

（2）swap分区是否启动，由`swapon`及`swapoff`控制。



    

    