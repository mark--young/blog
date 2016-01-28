## git代码仓库转移

如果你想从别的 Git 托管服务那里复制一份源代码到新的 Git 托管服务器上的话，可以通过以下步骤来操作。

1). 从原地址克隆一份裸版本库，比如原本托管于 GitHub。
    
    git clone --bare git@/github.com:username/project.git

**一定要记住clone的是bare仓库。**

2). 然后到新的 Git 服务器上创建一个新项目，比如 GitCafe上建立一个项目newproject.git。

3). 以镜像推送的方式上传代码到 GitCafe 服务器上。
    
    cd project.git
    
    git push --mirror git@gitcafe.com:username/newproject.git

4). 删除本地代码

    cd ..
    
    rm -rf project.git

5). 到新服务器 GitCafe 上找到 Clone 地址，直接 Clone 到本地就可以了。
    
    git clone git@gitcafe.com:username/newproject.git

这种方式可以保留原版本库中的所有内容。


我们来测试一下

从gitcafe clone出来的仓库看得到，我们github上的项目的所有commmit记录都得以了保存。