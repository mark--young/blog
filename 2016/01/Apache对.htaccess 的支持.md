### Ubuntu/Debian下启动Apache对.htaccess 的支持步骤:

1.终端运行
    
    sudo a2enmod

程序提示可供激活的模块名称，输入：
    
    rewrite


2.修改指向站点的配置文件(该链接)
    
    vi /etc/apache2/sites-enabled/000-default 

把（默认的www目录、或者需要应用.htaccess的目录）下的AllowOverride 属性改为All，保存。

3.重新加载apache
    
    sudo /etc/init.d/apache2 restart14514686.28797

### 本地环境WAMP下，修改apache的配置文件httpd.conf

参考：[使用WampServer搭建本地PHP环境，绑定域名，配置伪静态](http://wayearn.com/2016/01/wamp/)

1.启动wampserver服务，左键单击右下角wampserver图标，打开Apache菜单下“httpd.conf”文件；

2.搜索找到“LoadModule rewritemodule modules/modrewrite.so”这一行，去掉前面的“#”；

3.找到“AllowOverride None”改为“AllowOverride All”；

4.重启wampserver的所有服务。


