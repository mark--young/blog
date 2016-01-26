## Debian上安装LAMP环境（Linux+Apache+MySQL+PHP）

###1.更新:
    
    apt-get update

###2.安装Apache：

    apt-get install apache2

安装完之后，你就可以打开自己的IP了，如：http://104.238.148.88/

会看到apache安装成功并运行了。

###3.安装MySQL:

    apt-get install mysql-server

之后：运行安装脚本

    mysql_secure_installation

提示：

    Enter current password for root (enter for none): 

输入root用户的MySQL密码，如：1234567890，回车；

    OK, successfully used password, moving on...

    By default, a MySQL installation has an anonymous user, allowing anyone
    to log into MySQL without having to have a user account created for
    them.  This is intended only for testing, and to make the installation
    go a bit smoother.  You should remove them before moving into a
    production environment.
    
    Remove anonymous users? [Y/n] y                                            
     ... Success!
    
    Normally, root should only be allowed to connect from 'localhost'.  This
    ensures that someone cannot guess at the root password from the network.
    
    Disallow root login remotely? [Y/n] y
    ... Success!
    
    By default, MySQL comes with a database named 'test' that anyone can
    access.  This is also intended only for testing, and should be removed
    before moving into a production environment.
    
    Remove test database and access to it? [Y/n] y
     - Dropping test database...
     ... Success!
     - Removing privileges on test database...
     ... Success!
    
    Reloading the privilege tables will ensure that all changes made so far
    will take effect immediately.
    
    Reload privilege tables now? [Y/n] y
     ... Success!
    
    Cleaning up...

**记住一路y+回车下来！！**

###4.安装PHP：

    apt-get install php5 php-pear php5-mysql 

完成之后：

    service apache2 restart

重启apache2服务。

或者使用：

     /etc/init.d/apache2 restart

------

下面来测试下：

    vi /var/www/info.php

按i之后，输入：

    <?php
    phpinfo();
    ?>

esc，:wq保存退出。

打开VPS IP/info.php，例：http://104.238.148.88/info.php
