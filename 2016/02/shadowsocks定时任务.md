## Shadowsocks定时任务

### 前言
小伙伴有没有在使用一段时间shadowsocks之后，就无法使用了？进程被kill了，需要在Linux中进行restart。

### 寻找解决方案

有之前[mySQL的定时任务](http://wayearn.com/2016/02/%E7%9B%91%E6%8E%A7mysql%E8%BF%90%E8%A1%8C%E7%8A%B6%E6%80%81%EF%BC%8C%E5%81%9C%E6%AD%A2%E5%88%99%E9%87%8D%E5%90%AF/)的经验之后，打算也给shadowsocks弄一个相应的定时任务。

一直比较关注teddysun.com，所以借鉴一下他的脚本：[Shadowsocks定时任务脚本](https://shadowsocks.be/6.html)

    cat /var/log/shadowsock-crond.log

发现log中，如下问题：

    2016-02-24 01:15:01 Shadowsocks start failed

what?重启失败，打开脚本发现：

    # check Shadowsocks status
    /etc/init.d/shadowsocks status &>/dev/null

我自己执行了一下，没有/etc/init.d/shadowsocks这个文件，what?大神也会出错？

于是通读了一下teddysun写的一键脚本：[Shadowsocks Python版一键安装脚本](https://shadowsocks.be/1.html)的脚本[https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh](https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh)

发现其中有一段关于Debian的代码：

     if ! wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-debian -O /etc/init.d/shadowsocks; then
            echo "Failed to download shadowsocks chkconfig file!"

原来问题在这里，下面附上脚本代码：

### Shadowsocks控制脚本

用于实现start/stop/status/restart功能。

1.新建文件：

    cd ~
    
    touch myss.sh

2.按`i`后，把下面代码考进去

    #!/bin/bash
    ### BEGIN INIT INFO
    # Provides:          shadowsocks
    # Required-Start:    $network $local_fs $remote_fs
    # Required-Stop:     $network $local_fs $remote_fs
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: Fast tunnel proxy that helps you bypass firewalls
    # Description:       A secure socks5 proxy, designed to protect your Internet traffic.
    ### END INIT INFO
    
    # Author: Teddysun <i@teddysun.com>
    
    name=shadowsocks
    BIN=/usr/local/bin/ssserver
    conf=/etc/shadowsocks.json
    
    start(){
        $BIN -c $conf -d start
        RETVAL=$?
        if [ "$RETVAL" = "0" ]; then
            echo "$name start success"
        else
            echo "$name start failed"
        fi
    }
    
    stop(){
        pid=`ps -ef | grep -v grep | grep -i "${BIN}" | awk '{print $2}'`
        if [[ ! -z $pid ]]; then
            $BIN -c $conf -d stop
            RETVAL=$?
            if [ "$RETVAL" = "0" ]; then
                echo "$name stop success"
            else
                echo "$name stop failed"
            fi
        else
            echo "$name is not running"
            RETVAL=1
        fi
    }
    
    status(){
        pid=`ps -ef | grep -v grep | grep -i "${BIN}" | awk '{print $2}'`
        if [[ -z $pid ]]; then
            echo "$name is not running"
            RETVAL=1
        else
            echo "$name is running with PID $pid"
            RETVAL=0
        fi
    }
    
    case "$1" in
    'start')
        start
        ;;
    'stop')
        stop
        ;;
    'status')
        status
        ;;
    'restart')
        stop
        start
        RETVAL=$?
        ;;
    *)
        echo "Usage: $0 { start | stop | restart | status }"
        RETVAL=1
        ;;
    esac
    exit $RETVAL

保存退出。

3.使用`chmod+x`给予文件执行权限

    chmod+x myss.sh

4.试着执行一下脚本

    ./myss.sh status
    ./myss.sh start
    ...

成功。


### Shadowsocks定时任务脚本

我们下面继续，上面只是完成了shadowsocks控制的脚本文件成功执行，但是，定时任务脚本，还需要进行修改一下：

我们来看看原teddysun的脚本：

    #!/bin/bash
    # Check Shadowsocks Server is running or not
    # Author: Teddysun <i@teddysun.com>
    # Visit: https://shadowsocks.be/6.html
    
    # name
    name=Shadowsocks
    # log path
    path=/var/log
    # check log path
    [[ ! -d $path ]] && mkdir -p $path
    # log file
    log=$path/shadowsocks-crond.log
    # shadowsocks-python path(centos)
    bin1=/usr/bin/ssserver
    # shadowsocks-python path(debian)
    bin2=/usr/local/bin/ssserver
    # shadowsocks-go path
    bin3=/usr/bin/shadowsocks-server
    # shadowsocks-libev path
    bin4=/usr/local/bin/ss-server
    # shadowsocksR path
    bin5=/usr/local/shadowsocks/server.py
    # default pid value
    pid=""
    
    # check Shadowsocks status
    /etc/init.d/shadowsocks status &>/dev/null
    if [ $? -eq 0 ]; then
        pid=`ps -ef | grep -v grep | grep -v ps | grep -i "${bin1}" | awk '{print $2}'` 
        if [ -z $pid ]; then
            pid=`ps -ef | grep -v grep | grep -v ps | grep -i "${bin2}" | awk '{print $2}'` 
            if [ -z $pid ]; then
                pid=`ps -ef | grep -v grep | grep -v ps | grep -i "${bin3}" | awk '{print $2}'` 
                if [ -z $pid ]; then
                    pid=`ps -ef | grep -v grep | grep -v ps | grep -i "${bin4}" | awk '{print $2}'` 
                    if [ -z $pid ]; then
                        pid=`ps -ef | grep -v grep | grep -v ps | grep -i "${bin5}" | awk '{print $2}'` 
                    fi
                fi
            fi
        fi
    fi
    
    # check status & auto start
    if [[ -z $pid ]]; then
        echo "`date +"%Y-%m-%d %H:%M:%S"` $name is not running" >> $log
        echo "`date +"%Y-%m-%d %H:%M:%S"` Starting $name" >> $log
        /etc/init.d/shadowsocks start &>/dev/null
        if [ $? -eq 0 ]; then
            echo "`date +"%Y-%m-%d %H:%M:%S"` $name start success" >> $log
        else
            echo "`date +"%Y-%m-%d %H:%M:%S"` $name start failed" >> $log
        fi
    else
        echo "`date +"%Y-%m-%d %H:%M:%S"` $name is running with pid $pid" >> $log
    fi

把其中的：

    # check Shadowsocks status
    /etc/init.d/shadowsocks status &>/dev/null

和

    /etc/init.d/shadowsocks start &>/dev/null

修改成：

    /root/myss.sh status &>/dev/null

和

    /root/myss.sh start &>/dev/null

问题解决。

试着执行了一下脚本：

    cd ~
    ./shadowsocks-crond.sh

看一下log文件：

    2016-02-24 02:03:29 Shadowsocks is running with pid 28885

正常。

### shadowsocks定时任务脚本添加到cron中

之前就介绍过，这里简单说一下：

    crontab -e

按`i`后在文件的最后添加：

    */5 * * * * /root/shadowsocks-crond.sh

然后按`esc`退出，`:wq`保存。

    service cron restart

然后过个几分钟，去看一下log文件：

    cat /var/log/shadowsock-crond.log

输出：


    2016-02-24 12:55:01 Shadowsocks is running with pid 29264
    2016-02-24 13:00:01 Shadowsocks is running with pid 29264
    2016-02-24 13:05:01 Shadowsocks is running with pid 29264
    2016-02-24 13:10:02 Shadowsocks is running with pid 29264
    2016-02-24 13:15:01 Shadowsocks is running with pid 29264
    2016-02-24 13:20:01 Shadowsocks is running with pid 29264
    2016-02-24 13:25:01 Shadowsocks is running with pid 29264
    2016-02-24 13:30:01 Shadowsocks is running with pid 29264
    2016-02-24 13:35:01 Shadowsocks is running with pid 29264
    2016-02-24 13:40:01 Shadowsocks is running with pid 29264
    2016-02-24 13:45:01 Shadowsocks is running with pid 29264
    2016-02-24 13:50:01 Shadowsocks is running with pid 29264
    2016-02-24 13:55:01 Shadowsocks is running with pid 29264
    2016-02-24 14:00:01 Shadowsocks is running with pid 29264
    2016-02-24 14:05:01 Shadowsocks is running with pid 29264
    2016-02-24 14:10:01 Shadowsocks is running with pid 29264
    2016-02-24 14:15:01 Shadowsocks is running with pid 29264
    root@vultr:~#


