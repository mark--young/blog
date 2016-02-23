##PHP调试环境配置：PhpStorm + Xdebug + WAMP

工欲善其事，必先利其器，本次给大家介绍PHP本地调试环境的配置，PhpStorm + Xdebug + WAMP。

**运行环境：**

* JetBrains PhpStorm 10.0.3
* PHP 5.5.12
* Xdebug：C:\wamp\bin\php\php5.5.12\zend_ext\php_xdebug-2.2.5-5.5-vc11-x86_64.dll

> Tips: PHP版本和xdebug版本一定要相对应，而且注意32位与64位。

Zend Studio相关下载地址：
[http://www.zend.com/en/products/studio/download](http://www.zend.com/en/products/studio/download)

###1.PHP配置xdebug扩展

php.ini的配置，下面的配置仅供参考，路径要换成自己的！

    [xdebug]
    zend_extension = "c:/wamp/bin/php/php5.5.12/zend_ext/php_xdebug-2.2.5-5.5-vc11-x86_64.dll"
    xdebug.remote_enable = On
    xdebug.remote_handler = dbgp
    xdebug.remote_host= localhost
    xdebug.remote_port = 9000
    xdebug.idekey = PHPSTORM

ps :  remote_handler 、 remote_host、 remote_port 这些都有默认值，但还是建议设置下，至少知道要设置这些参数~

###2.浏览器配置
* 下载插件Firebug和Xdebug
* 设置Xdebug的IDE key为PHPSTORM

###3.PHPSTORM配置

* 配置全局：File -> Settings -> Languages & Frameworks -> PHP -> Debug -> Xdebug
* 配置调试：Run -> Edit Configurations -> PHP Web Application -> 点击"+"
* 配置服务：点击Server后面的"..." -> 点击"+" -> 输入服务器相关信息 -> 选择Debugger为"Xdebug" -> OK
* 完善配置：Run -> Start Listening for PHP Debug Connections -> Debug
