##在新VPS上安装Wordpress


###1.MySQL操作，新增用户，及数据库。
进入数据库：

    >mysql -u root -p
    >（密码，隐藏不见）

新建数据库：

    >mysql>create database testDB;

新建一下用户test@localhost，数据库操作密码123456,给testDB,授予所有权限：

    >mysql>grant all privileges on testDB.* to test@localhost identified by '123456';

>**重要：记住数据库名testDB，用户名test，及用户密码123456，后面建立wordpress的wp-config.php文件时，会使用到。**

刷新权限：

    mysql>flush privileges;

Ctrl+Z退出。

###2.下载wordpress，并解压。

有几种方法：

1.使用WinScp软件把下载好的安装包上传，使用解压命令进行解压（这个很简单，配置一下winscp，图形界面的操作方法，不作介绍了）。

2.使用wget命令进行网络下载，并解压。（推荐）

    cd /var/www/

    wget https://wordpress.org/latest.zip

    unzip latest.zip -d /var/www/

>卧槽，提示没有unzip命令。所以没有的包在Debian这，只需要apt-get install xxx，Debian就是这么方便。

    apt-get installl unzip

可能会用到移动文件命令：mv
    
    mv /var/www/wordpress/* /var/www/

显示文件目录下的文件：

    dir

显示文件权限与组：

    ls -l

###3.安装wordpress。

通过http://服务器IP/，访问进行安装。

提示：

    没有修改wp-config.php文件的权限。

**解决方法：**

修改apache默认用户的写权限：

    chmod u+w /var/www/

修改apache默认目录的所有者为www-data:

    chown -R www-data: /var/www/

