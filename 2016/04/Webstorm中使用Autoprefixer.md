##Webstorm中使用Autoprefixer

最近玩了一下SASS，感觉不错，不过CSS3在不同平台兼容性代码一直是个头痛的问题，手写处理费时费力又容易出错。
曾经一直用sublime text写html和css，这些问题都有相应的插件。用Webstorm写js，但是来回切换编辑器也比较麻烦。
虽然Webstorm内置了css3自动补全功能，当输入user-select时,Webstorm会自动补全：
	
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

但是很多情况下，这种自动补全并不令人满意，比如当我输入display:flex;时，Webstorm并不会自动补全为：

	display:-webkit-box;
	display:-webkit-flex;
	display:-ms-flexbox;
	display:flex;

###关于Autoprefixer

Autoprefixer是一个后处理程序，不象Sass以及Stylus之类的预处理器。它适用于普通的CSS，可以实现css3代码自动补全。也可以轻松跟Sass，LESS及Stylus集成，在CSS编译前或编译后运行。详情见，[https://github.com/postcss/autoprefixer](https://github.com/postcss/autoprefixer)

当Autoprefixer添加前缀到你的CSS，还不会忘记修复语法差异。这种方式，CSS是基于最新W3C规范产生：

	a {
     background : linear-gradient(to top, black, white);
     display : flex
	}
	::placeholder {
	     color : #ccc
	}

编译成：

	a {
    background : -webkit-linear-gradient(bottom, black, white);
    background : linear-gradient(to top, black, white);
    display : -webkit-box;
    display : -webkit-flex;
    display : -ms-flexbox;
    display : flex
	}
	::-webkit-input-placeholder {
	    color : #ccc
	}
	::-moz-placeholder {
	    color : #ccc
	}
	:-ms-input-placeholder {
	    color : #ccc
	}
	::placeholder {
	    color : #ccc
	}

Autoprefixer 同样会清理过期的前缀，因此下面的代码：

	a {
    -webkit-border-radius : 5px;
    border-radius : 5px
	}

编译成：

	a {
	    border-radius : 5px
	}

因为经过Autoprefixer处理，CSS将仅包含实际的浏览器前缀。

###具体安装和配置：

所以尝试在Webstorm下搜索autoprefixer插件，无果。那就自己手动配置了一个。首先我考虑配置File Watchers，但是不习惯，原来在sublime text下用autoprefixer都是手动触发的，所以后面我配置了External Tools。

####1.首先当然是安装node.js;

直接在官网上[https://nodejs.org/en/download/](https://nodejs.org/en/download/)可以下载安装（windows）。

####2.安装Autoprefixer：

见[https://github.com/postcss/autoprefixer](https://github.com/postcss/autoprefixer)：
	
	sudo npm install autoprefixer -g

要不要加`sudo`，或者是不是全局安装（`-g`）那就看你自己的环境了。

npm太慢，我是用淘宝的 NPM 镜像的[https://npm.taobao.org/](https://npm.taobao.org/)

####3.安装postcss-cli

Autoprefixer其实是postcss的插件，见[https://github.com/code42day/postcss-cli](https://github.com/code42day/postcss-cli)
	
	sudo npm install postcss-cli -g

####4.配置External Tools

打开Webstorm设置，Preferences -> Tools -> External Tools ;点击新增按钮，如图：


填写具体配置，例如我的配置，如图：

* Program:填入你的postcss-cli 的PATH；
* Parameters: -u autoprefixer -o $FileDir$/$FileName$  $FileDir$/$FileName$ ，你可以根据你自己的需要配置，具体参见[https://github.com/code42day/postcss-cli](https://github.com/code42day/postcss-cli)
* Working directory :$ProjectFileDir$

配置好后，你可以在css，或sass文件中右键，就可以在右键菜单中看到External Tools – autoprefixer，点击搞定，嘎嘎。

####5.设置快捷键
右键太麻烦的话，可以设置个快捷键，打开Webstorm设置，Preferences -> Keymap ， 搜索External Tools ， 配置 autoprefixer即可。 不要和原来的冲突就可以了。


