## Debian下更换默认编辑器为vim & 解决编辑出现ABCD的问题

有些同学在debian系统下输入VI或VIM的命令编辑文本，确发现按键盘的上下左右方向键，变成显示ABCD字符了，退格键也失灵。恩，没错这是有些debian系统默认的编辑器并不是VIM而是nano的缘故。下面有几个方法可以解决问题。

    There is only one alternative in link group editor: /usr/bin/vim.tiny Nothing to configure.


运行如下命令：

    update-alternatives --config editor
    
出现如下界面：

    There are 3 alternatives which provide `editor'.
    
    Selection    Alternative
    
    -----------------------------------------------
    
             1    /bin/ed
    
    +        2    /bin/nano
    
    *        3    /usr/bin/vim.tiny

    
    Press enter to keep the default[*], or type selection number:

选择3重启一下就可以了.提示，这里只是vim.tiny，而我们经常用的是vim.basic。

    
    apt-get remove nano   

    vi /etc/apt/sources.list


按i，添加源：

    
    deb http://ftp.debian.org/debian squeeze main contrib non-free
    deb http://security.debian.org squeeze/updates main contrib non-free


    :wq #保存配置，退出
    
    
    apt-get update #更新源
    
    apt-get upgrade #更新系统


重新安装VIM编辑器：
    
    apt-get install vim

再次使用：

    update-alternatives --config editor

使用vim.basic。

这样就OK了.