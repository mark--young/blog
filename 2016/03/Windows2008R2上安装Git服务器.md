##Windows2008R2上安装Git服务器

本文介绍了在Windows2008R2上搭建Git服务器（Git Server），记录搭建过程中的一些问题。采用的策略是服务器端：CopSSH++Git，客户端：TortoiseGit。图文教程+视频教程！让你也能搭一台属于自己的windows下的Git服务器。

关键词：Git服务器，Git Server，搭建git服务器。

###资源下载
1.CopSSH 

[百度云下载地址](http://pan.baidu.com/s/1bckWxw)

2.Git

[官方下载地址](https://git-scm.com/downloads)

[百度云下载地址 64bit](http://pan.baidu.com/s/1i4iIrB3)

3.TortoiseGit

[官方下载地址](https://tortoisegit.org/download/)

[百度云下载地址](http://pan.baidu.com/s/1pKfOnjH)

###Windows2008上的配置

####1.安装CopSSH
注意：**建议使用默认的安装方式安装，修改一下安装目录为c:\ICWa或者C：\SSH之类的，简单的。**

问题1：安装CopSSH无法使用密钥，出现：public key permission denied错误。

建议，重新安装CopSSH，卸载之后，把之前设置的用户在“控制面板”中删除，删除安装目录。

问题2：无法连接SSH服务器，telnet失败。无法启动CopSSH服务。

建议：安装的时候，不要设置CopSSH的密码与用户。密码的强度在win2008R2下是有要求的。可以看看控制面板中是否已经建立了SSH账户。如果不行，请参考1的方法，删除用户，卸载，删除文件，重装，使用默认的方式。


####2.安装Git

请使用默认的选项进行安装。

####3.设置环境变量

	HOME
	C:\ICW
	
	GIT_HOME
	C:\git
	
	Path:
	%GIT_HOME%\bin;%GIT_HOME%\mingw64\libexec\git-core


####4.生成密钥

请参考博文：
[git使用ssh密钥进行push&pull等操作](http://wayearn.com/2016/01/gitssh/)


####5.初始化仓库
请参考博文：
[Git入门教程](http://wayearn.com/2016/03/git%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B/)


