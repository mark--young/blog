## 使用WampServer搭建本地PHP环境，绑定域名，配置伪静态
wordpress 除了首页其他页面打不开 htaccess问题 伪静态问题 WAMP配置
### WampServer 简介

Wamp就是Windows Apache Mysql PHP集成安装环境，即在window下的apache、php和mysql的服务器软件。也是一件安装，不需要你进行环境的配置。

### WampServer 下载安装
官方地址：[http://www.wampserver.com/](http://www.wampserver.com/) （支持32位和64位系统，根据自己的系统选择版本）

1.下载后，直接运行安装，安装过程可能会要你设置默认浏览器，过程略过。

2.运行 WampServer ，在右下角的任务栏出现图标，在图标上右键，选择语言为简体中文。

3.在图标上单击左键，出现 WampServer 的快捷管理菜单，包括各种服务的快捷入口和服务设置：

> Localhost：默认的网站首页，如果打开显示 403 Forbidden，你可以手动输入 http://127.0.0.1 进行访问
> 
> 或者打开 c:\windows\system32\drivers\etc   修改hosts文件，添加一条记录
> 
> 127.0.0.1      localhost
> 
> 保存即可。

**注：如果提示你无法保存hosts文件，可能是你目前的系统用户没有修改权限，请自己搜索解决办法；或者是某些安全软件限制了修改，暂时退出安全软件。**

www目录：存放网站文件的根目录

### WampServer 绑定域名，添加虚拟主机
1.将你要绑定的域名，使用A记录绑定到 127.0.0.1

2.启动wampserver服务，左键单击右下角wampserver图标，打开Apache菜单下“httpd.conf”文件； 找到“# Include conf/extra/httpd-vhosts.conf” ，把这句前面的#号去掉，启用了虚拟主机配置文件 httpd-vhosts.conf 的引用。

３.在Apache安装目录的confextra目录下，比如我的是 D:\wamp\bin\apache\apache2.2.22\conf\extra，用记事本打开httpd-vhosts.conf，最最底部你会看到２个虚拟主机样例，将其中一个修改为类型下面的，删除多余的样例：

    <VirtualHost *:80>
        ServerAdmin admin@xxx.com
        DocumentRoot "D:/wamp/www/xxx.com"
        ServerName www.xxx.com
        ErrorLog "logs/www.xxx.com-error.log"
        CustomLog "logs/www.xxx.com-access.log" common
    </VirtualHost>

4.在托盘中左键单击wampserver，重启所有服务；

5.用记事本打开 c:\windows\system32\drivers\etc    目录下hosts文件，在最下面添加一行：

127.0.0.1      www.xxx.com

6.在浏览器下输入www.xxx.com,可以看到通过http已经访问到本机下 d:\wamp\www\xxx.com 目录，以后你只要将这个网站的文件放在这个目录即可。

7.如果你要添加多个虚拟主机，重复上面的操作即可。

### WampServer 配置伪静态
默认情况下，WampServer不支持伪静态，我们需要进行一些配置

1.启动wampserver服务，左键单击右下角wampserver图标，打开Apache菜单下“httpd.conf”文件；

2.搜索找到“LoadModule rewrite_module modules/mod_rewrite.so”这一行，去掉前面的“#”；

3.找到“AllowOverride None”改为“AllowOverride All”；

4.重启wampserver的所有服务

5.新建.haccess文件，放在当前网站根目录下，在.haccess文件中添加伪静态规则，比如添加WordPress伪静态规则

    # BEGIN WordPress
    <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    </IfModule>
    # END WordPress

**注：每个建站程序的伪静态规则不一样，请根据自己的需要添加。**