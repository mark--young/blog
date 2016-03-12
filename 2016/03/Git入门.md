##Git入门教程

###Git简介

Git是目前世界上最先进的分布式版本控制系统。

Git起源：[http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137402760310626208b4f695940a49e5348b689d095fc000](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137402760310626208b4f695940a49e5348b689d095fc000)

**1.版本控制**

典型代表Word文件的编辑，你的文件夹中是不是有这样的情况：

	word20160301.doc
	word备份的.doc
	word(小王).doc
	word-03.doc
	word.doc

而某一天，你可能需要以前修改过的版本（因为，经常会遇到这种抽风的上司或者客户）

而由版本控制给你带来的是：

	版本	用户	说明	日期
	1   张三	删除了软件服务条款5	7/12 10:38
	2	张三	增加了License人数限制	7/12 18:09
	3	李四	财务部门调整了合同金额	7/13 9:51
	4	张三	延长了免费升级周期	7/14 15:17

而且，你想退回到哪里，就可以退回到哪里！

记住第一个关键词：（无尽的）**后悔药**
	

**2.分布式 VS 集中式**

集中式，典型的代表就是SVN，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

分布式，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

Git不单是不必联网这么简单，Git更强大的是分支管理。后面讲到~~~~

关于更多SVN&Git的区别可以参见：

[蒋鑫：为什么 Git 比 SVN 好](http://blog.jobbole.com/20069/)

[SVN 和 Git 在日常使用中的明显差异](https://github.com/xirong/my-git/blob/master/why-git.md)

[GIT和SVN之间的五个基本区别](http://www.vaikan.com/5-fundamental-differences-between-git-svn/)

记住第二个关键词：**分布式**

###Git环境搭建

**1.安装Git**

* 在Linux(Debian)上安装Git:

	apt-get install git

* Mac OS X上安装Git：

    一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：[http://brew.sh/](http://brew.sh/)。

    第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

* 在Windows上安装Git

	从这里[https://git-for-windows.github.io/](https://git-for-windows.github.io/)下载，双击安装

	安装完成后，可以在右键菜单/开始菜单中找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

**2.全局变量设置**

就像Java需要设置Path一样，Git需要设置一些全局变量。

	“Git”->“Git Bash”

	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"

设置用户与Email，相当于自报家门，让版本库有一个记录。注意：`git config`命令的`--global`是全局设置的意思。

> 任何一个命令或者参考：`git [命令] --help`来查看帮助，或者登陆官方来学习命令[http://git-scm.com/doc](http://git-scm.com/doc)

参考资料：[Git 内部原理 - 环境变量](http://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)

**3.创建版本库**

非常简单。

* windows下，需要建立的版本库的地方，右键git bash->

		$ git init

	瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

	PS:如果你没有看到.git目录，那是因为这个目录默认是隐藏的

* Linux中：

如果，需要在learngit目录下建立一个Git仓库，可以如下操作

	$ mkdir learngit
	$ cd learngit
	$ git init

你也可以这样:

	$ git init learngit

试一试吧！

**4.基本操作**

* Git工作区和暂存区:

	我们看到目录为工作区(/learngit)；需要进行提交到版本库的文件放在暂存区（看不到，需要使用`git status`来查看）。

	`git status`命令：可以让我们时刻掌握仓库当前的状态。

	`git diff`命令：让我们查看文件与版本库中的区别。

* 获取远程仓库代码（前题是`init`之后）

	（1）克隆仓库：
	
		$ git clone [url]
		
	（2）或者添加远程仓库：
	
	使用`git remote add`命令，添加一个远程仓库的链接，命令格式：`git remote add [远程仓库别名] [远程仓库地址]`
	
		$ git remote add origin git@github.com:michaelliao/learngit.git
	
	使用`git pull`拉取远程代码的`HEAD`头标记，即最新的代码。

	命令格式：`$ git pull <远程主机名> <远程分支名>:<本地分支名>`
	
		$ git pull 
	
	PS:使用`git remote`来查看远程库别名，使用`git remote -v`来查看更加详细的远程库的信息。
	
		$ git remote
		origin
	
		home@home-PC MINGW64 /f/learngit (master)
		$ git remote -v
		origin  https://github.com/wayearn/learngit.git (fetch)
		origin  https://github.com/wayearn/learngit.git (push)


* 提交代码


	把所有的文件更改提交到暂存区：

		$ git add -a

	为所有暂存区的代码写入日志并提交到本地仓库：

		$ git commit -m "(something)"

	把所有本地仓库的提交，更新到远程仓库：

		$ git push


* Git时光机
	
	（1）`git log`命令：查看每次修改的日志文件。

     [git log与git reflog的区别](http://stackoverflow.com/questions/17857723/whats-the-difference-between-git-reflog-and-log)，记得几点：git log是顺着当前分支往前去查找提交记录，而git reflog并不像git log去遍历提交历史，它都不是仓库的一部分，它不包含推送、更新或者克隆，而是作为本地提交记录的清单。简单理解：**本地后悔药**。

	（2）`git reset`命令：回退命令。
	
	首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

		$ git reset --hard HEAD^
		HEAD is now at ea34578 add distributed

	回退add命令提交到缓存区的文件，并不会把文件恢复缓存区，需要区别（3）`git checkout`命令：

		$ git reset HEAD <file>


	（3）`git checkout -- <file>`命令:丢弃缓存区文件的修改，把文件恢复到`git add`之前的状态。

	（4）`git diff HEAD -- <file>`命令可以查看<file>工作区和版本库里面最新版本的区别


	（5）`git rm`删除文件。

###分支管理

还记得《星际穿越》中的平行空间吗？两个独立的空间互不干扰，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。在某一个时间点，两个平行的时空合并了，结果，你既学会了Git又学会了SVN！

**分支在实际中有什么用呢？**假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

**分支的独立性：**现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

**git分支的高效：**其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

**1.理解`HEAD`头指针**

一开始的时候，`HEAD`头指针指向的是主分支，即`master`分支。而`HEAD`指向的是当前分支，`master`指向的是提交。

如果，在`master`分支上新建了一个分支`dev`，此时`HEAD`指向了`dev`，Git建立分支的过程很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变。

**2.创建`dev`分支**

创建分支使用`git branch`命令，命令格式：`git branch [分支别名]`

	$ git branch dev

可以使用`$ git branch`来查看所有本地分支，`$ git branch -a`查看所有分支（包括远程分支）。

使用`git checkout [分支名]`切换到对应的分支，如：

	$ git checkout dev 

此时，`HEAD`头指针会指向dev，如果在`dev`上提交，`dev`指针会往前移，而其他分支不变。（`master`分支及指针不变）

当使用`git checkout master`时，`HEAD`头指针会重新指向`master`，此时再提交，`master`指针会往前移。

这个过程，需要自己亲身的试验才能体会到它们的作用和变化。

	$gitk

使用Git自带的图形界面，可以很好的来管理分支。

**3.冲突解决**

冲突产生：当两个分支中修改的相同的文件并提交（add->commit），合并(merge)这两个分支的时候，会产生冲突。

如下例：

	$ git checkout -b feature1

在新的`feature1`分支下修改了readme.txt：

	vi readme.txt
	//修改，添加Creating a new branch is quick AND simple.
	$ git add readme.txt 
	$ git commit -m "AND simple"
	
切换到`master`分支：

	$ git checkout master

	vi readme.txt
	//在`master`分支上把readme.txt文件的最后一行改为：Creating a new branch is quick & simple
	$ git add readme.txt 
	$ git commit -m "& simple"
	
试图合并`master`与`feature1`：

	$ git merge feature1
	Auto-merging readme.txt
	CONFLICT (content): Merge conflict in readme.txt
	Automatic merge failed; fix conflicts and then commit the result.

（1）使用：`$ git status`来查看冲突文件：

	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 2 commits.
	#
	# Unmerged paths:
	#   (use "git add/rm <file>..." as appropriate to mark resolution)
	#
	#       both modified:      readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")

（2）直接查看readme.txt文件内容：

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	<<<<<<< HEAD
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick AND simple.
	>>>>>>> feature1

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

	Creating a new branch is quick and simple.
再提交：
	
	$ git add readme.txt 
	$ git commit -m "conflict fixed"
	[master 59bc1cb] conflict fixed

PS: 用带参数的`git log`也可以看到分支的合并情况：

	$ git log --graph --pretty=oneline --abbrev-commit
	*   59bc1cb conflict fixed
	|\
	| * 75a857c AND simple
	* | 400b400 & simple
	|/
	* fec145a branch test
	...

最后，删除`feature1`分支：
	
	$ git branch -d feature1
	Deleted branch feature1 (was 75a857c).

**4.分支管理策略**

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-f`f方式的`git merge`：

首先，仍然创建并切换`dev`分支：
	
	$ git checkout -b dev
	Switched to a new branch 'dev'

修改readme.txt文件，并提交一个新的`commit`：

	$ git add readme.txt 
	$ git commit -m "add merge"
	[dev 6224937] add merge
	 1 file changed, 1 insertion(+)

现在，我们切换回`master`：

	$ git checkout master
	Switched to branch 'master

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：
	
	$ git merge --no-ff -m "merge with no-ff" dev
	Merge made by the 'recursive' strategy.
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)

分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如`1.0`版本发布时，再把dev分支合并到m`aster`上，在`master`分支发布`1.0`版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

**5.Bug分支**

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

	$ git status
	# On branch dev
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   hello.py
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

	$ git stash
	Saved working directory and index state WIP on dev: 6224937 add merge
	HEAD is now at 6224937 add merge

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：
	
	$ git checkout master
	$ git checkout -b issue-101

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：
	
	$ git add readme.txt 
	$ git commit -m "fix bug 101"

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

	$ git checkout master
	$ git merge --no-ff -m "merged bug fix 101" issue-101
	$ git branch -d issue-101

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！
	
	$ git checkout dev
	Switched to branch 'dev'
	$ git status
	# On branch dev
	nothing to commit (working directory clean)

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：
	
	$ git stash list
	stash@{0}: WIP on dev: 6224937 add merge

工作现场还在，Git把`stash`内容存在某个地方了，但是**需要恢复**一下，有两个办法：

**一种方式：**用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

**另一种方式：**是用`git stash pop`，恢复的同时把`stash`内容也删了：

	$ git stash pop
	# On branch dev
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   hello.py
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	Dropped refs/stash@{0} (f624f8e5f082f2df2bed8a4e09c12fd2943bdd40)

再用`git stash list`查看，就看不到任何stash内容了：

	$ git stash list

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：
	
	$ git stash apply stash@{0}

**6.删除分支**

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

还记得吗？ 

建立新的分支:`git checkout -b feature-new`

工作提交：`git add --a`，`git commit -m "something..."`

回到`dev`开发分支：`git checkout dev`

合并分支：`git merge --no-ff feature-new`

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是，就在此时，接到上级命令，因经费不足，新功能必须取消！虽然白干了，但是这个分支还是必须就地销毁：

（1）如果没有合并之前，可以简单的使用`git branch -d [分支名]`来删除分支（使用`-D`命令，强制删除分支）

（2）如果已经合并，除了上面的需要删除以外，还需要使用前面讲到的`git reset --hard HEAD^`来退回到上一个版本。

PS:分支的删除，不会影响到其他分支上已经合并的分支内容。


**7.多人协作**

多人协作的工作模式通常是这样：

首先，可以试图用`git push origin branch-name`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name。`

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

参考资料：

[Git 少用 Pull 多用 Fetch 和 Merge](http://www.oschina.net/translate/git-fetch-and-merge)

[真正理解 git fetch, git pull 以及 FETCH_HEAD](https://ruby-china.org/topics/4768)

[git pull 和 git fetch 有什么区别？](https://ruby-china.org/topics/15729)


###标签管理

发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

**1.创建标签（快照）**

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

	$ git branch
	* dev
	  master
	$ git checkout master
	Switched to branch 'master'

然后，敲命令`git tag <name>`就可以打一个新标签：
	
	$ git tag v1.0

可以用命令`git tag`查看所有标签：

	$ git tag
	v1.0

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

	$ git log --pretty=oneline --abbrev-commit
	6a5819e merged bug fix 101
	cc17032 fix bug 101
	7825a50 merge with no-ff
	6224937 add merge
	59bc1cb conflict fixed
	400b400 & simple
	75a857c AND simple
	fec145a branch test
	d17efd8 remove test.txt

比方说要对add merge这次提交打标签，它对应的commit id是`6224937`，敲入命令：

	$ git tag v0.9 6224937

再用命令`git tag`查看标签：

	$ git tag
	v0.9
	v1.0

> 注意，**标签不是按时间顺序列出，而是按字母排序的。**

可以用`git show <tagname>`查看标签信息：
	
	$ git show v0.9
	commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
	Author: Li Wei <i@wayearn.com>
	Date:   Thu Aug 22 11:22:08 2013 +0800
	
	    add merge
	...

可以看到，v0.9确实打在add merge这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：
	
	$ git tag -a v0.1 -m "version 0.1 released" 3628164

用命令`git show <tagname>`可以看到说明文字：

	$ git show v0.1
	tag v0.1
	Tagger: Li Wei <i@wayearn.com>
	Date:   Mon Aug 26 07:28:11 2013 +0800
	
	version 0.1 released
	
	commit 3628164fb26d48395383f8f31179f24e0882e1e0
	Author: Li Wei <i@wayearn.com>
	Date:   Tue Aug 20 15:11:49 2013 +0800
	
    append GPL

还可以通过`-s`用私钥签名一个标签：
	
	$ git tag -s v0.2 -m "signed version 0.2 released" fec145a

参考资料：

[GPG入门教程](http://www.ruanyifeng.com/blog/2013/07/gpg.html)

[带GPG签名的Git tag](http://airk000.github.io/git/2013/09/30/git-tag-with-gpg-key)

[git使用GPG进行签名](http://blog.csdn.net/chenjh213/article/details/47978199)

**2.标签操作（删除，推送）**

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	$ git tag -d v0.9
	Deleted tag 'v0.9' (was 6224937)

然后，从远程删除。删除命令也是push，但是格式如下：

	$ git push origin :refs/tags/v0.9
	To git@github.com:michaelliao/learngit.git
	 - [deleted]         v0.9

###使用.gitignore忽略文件

有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...，有强迫症的童鞋心里肯定不爽。

好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)

忽略文件的原则是：

* 忽略操作系统自动生成的文件，比如缩略图等；
* 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
* 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有Desktop.ini文件，因此你需要忽略Windows自动生成的垃圾文件：
	
	# Windows:
	Thumbs.db
	ehthumbs.db
	Desktop.ini

然后，继续忽略Python编译产生的`.pyc`、`.pyo`、`dist`等文件或目录：

	# Python:
	*.py[cod]
	*.so
	*.egg
	*.egg-info
	dist
	build

加上你自己定义的文件，最终得到一个完整的`.gitignore`文件，内容如下：

	# Windows:
	Thumbs.db
	ehthumbs.db
	Desktop.ini
	
	# Python:
	*.py[cod]
	*.so
	*.egg
	*.egg-info
	dist
	build
	
	# My configurations:
	db.ini
	deploy_key_rsa

最后一步就是把.gitignore也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说working directory clean。

使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

或者可以使用以下方法，在git bash中输入以下命令：

	$ touch .gitignore
	$ vi .gitignore

**3.配置命令别名**

有没有经常敲错命令？比如`git status`？status这个单词真心不好记。

如果敲`git st`就表示`git status`那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉`Git`，以后`st`就表示`status`：

	$ git config --global alias.st status

好了，现在敲`git st`看看效果。

当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch

以后提交就可以简写成：

	$ git ci -m "bala bala bala..."

`--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

在撤销修改一节中，我们知道，命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个`unstage`操作，就可以配置一个`unstage`别名：

	$ git config --global alias.unstage 'reset HEAD'

当你敲入命令：

	$ git unstage test.py

实际上Git执行的是：

	$ git reset HEAD test.py

配置一个`git last`，让其显示最后一次提交信息：

	$ git config --global alias.last 'log -1'

这样，用`git last`就能显示最近一次的提交：

	$ git last
	commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
	Merge: bd6ae48 291bea8
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py

甚至还有人丧心病狂地把`lg`配置成了：

	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

来看看git lg的效果：

为什么不早点告诉我？别激动，咱不是为了多记几个英文单词嘛！

**配置文件**

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

	$ cat .git/config 
	[core]
	    repositoryformatversion = 0
	    filemode = true
	    bare = false
	    logallrefupdates = true
	    ignorecase = true
	    precomposeunicode = true
	[remote "origin"]
	    url = git@github.com:michaelliao/learngit.git
	    fetch = +refs/heads/*:refs/remotes/origin/*
	[branch "master"]
	    remote = origin
	    merge = refs/heads/master
	[alias]
	    last = log -1

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.`gitconfig`中：

	$ cat .gitconfig
	[alias]
	    co = checkout
	    ci = commit
	    br = branch
	    st = status
	[user]
	    name = Your Name
	    email = your@email.com

