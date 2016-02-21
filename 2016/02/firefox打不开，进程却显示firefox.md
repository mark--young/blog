本文介绍了在使用firefox时，突然打不开，却进程中显示有firefox的情况，解决方案有两种：1.重装。2.新建firefox配置文件。

### 解决方案1：

试着重装firefox。

### 解决方案2：

（1）打开命令工具：

开始 -> 运行 -> cmd.exe -> 鼠标右键 -> “以管理员身份运行”

（2）寻找firefox安装位置：

右键firefox -> 属性

    "C:\Program Files (x86)\Mozilla Firefox\firefox.exe"

（3）在命令工具中，输入以下命令：

    cd C:\Program Files (x86)\Mozilla Firefox\

    firefox.exe -p -no-remote

一行一行的输入，后回车。

（4）选择“创建配置文件” -> 下一步 -> 命名"myset" -> 完成 -> 启动 Firefox

