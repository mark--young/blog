##监控mysql运行状态，停止则重启

本文介绍了使用简单的shell脚本来监控mysql的运行情况，如果停止则重启。

###前言

最近发现MySQL服务隔三差五就会挂掉，导致我的网站无法正常运作，WordPress提示“建立数据库连接时出错”。

虽然我自己有[网站监控](http://www.SiteUptime.com/?aff=158876)和邮件通知，但是好多时候还是需要我来手动连接我的服务器重新启动一下我的MySQL，这样简直太不友好了.

所以，需要一个脚本，定时监控它，如果发现mySQL挂掉了就重启它。

好了，闲言碎语不多讲，开始我们的配置之旅。

**运行环境：**

    Distributor ID: Debian
    Description:    Debian GNU/Linux 7.9 (wheezy)
    Release:        7.9
    Codename:       wheezy

###1.新建脚本：

首先，我们要编写一个shell脚本，脚本主要执行的逻辑如下：

显示mysqld进程状态，如果判断进程未在运行，那么输出日志到文件，然后启动mysql服务，如果进程在运行，那么不执行任何操作，可以选择性输出监测结果。

可能大家对于shell脚本比较陌生，在这里推荐官方的shell脚本文档来参考一下

shell脚本的后缀为sh，在任何位置新建一个脚本文件，我选择在 /etc/mysql 目录下新建一个 listen.sh 文件。

执行如下命令：

    cd /usr/local/sbin/
    touch check_mysql.sh
    vi check_mysql.sh

按`i`添加如下内容：

    #!/bin/bash
    pgrep mysqld &> /dev/null
    if [ $? -gt 0 ]
    then
    echo "`date` mysql is stop" >>/var/log/check_mysql.log
    service mysql start
    else
    echo "`date` mysql running" >>/var/log/check_mysql.log
    fi

其中 `pgrep mysqld` 是监测mysqld服务的运行状态，`&> /dev/null` 是将其结果输出到空文件，也就是不保存输出信息

`$?` 是拿到上一条命令的运行结果，`-gt 0` 是判断是否大于0，后面则是输出时间到日志文件，然后启动`mysql`，否则不启动`mysql`。

> Tips:写完之后可以在脚本所在目录下使用`./check_mysql.sh`执行脚本，看看log中是否有打印信息，判断脚本执行情况。

###2.赋予脚本权限

+x 是代表可执行权限：

    chmod +x check_mysql.sh

###3.设计定时任务

加入crontab，让系统五分钟检测一次mysql状态
    
    #crontab -e

按`i`，调整光标，在文件的最后添加：
    
    */5 * * * * /usr/local/sbin/check_mysql.sh > /dev/null 2>&1

crontab命令相关基础知识，[请点击](http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/crontab.html)

###4.查看定时任务效果：

    cat /var/log/check_mysql.log

输出效果：

    Tue Feb 23 03:25:01 UTC 2016 mysql running
    Tue Feb 23 03:30:01 UTC 2016 mysql running
    Tue Feb 23 03:35:01 UTC 2016 mysql running
    Tue Feb 23 03:40:01 UTC 2016 mysql running
    Tue Feb 23 03:45:01 UTC 2016 mysql running
    Tue Feb 23 03:50:01 UTC 2016 mysql running
    Tue Feb 23 03:55:01 UTC 2016 mysql running
    Tue Feb 23 04:00:01 UTC 2016 mysql running
    Tue Feb 23 04:05:01 UTC 2016 mysql running
    Tue Feb 23 04:10:01 UTC 2016 mysql running
    Tue Feb 23 04:15:01 UTC 2016 mysql running
    Tue Feb 23 04:20:01 UTC 2016 mysql running
    Tue Feb 23 04:25:01 UTC 2016 mysql running
    Tue Feb 23 04:27:21 UTC 2016 mysql running
    Tue Feb 23 04:28:00 UTC 2016 mysql running
    Tue Feb 23 04:28:06 UTC 2016 mysql running
    Tue Feb 23 04:28:53 UTC 2016 mysql running
    Tue Feb 23 04:30:01 UTC 2016 mysql running
