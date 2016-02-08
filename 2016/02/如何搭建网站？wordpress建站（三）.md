## WORDPRESS入门

### 什么是WordPress？

> WordPress is web software you can use to create a beautiful website, blog, or app. We like to say that WordPress is both free and priceless at the same time.

译文：Wordpress是你可以用来创建炫酷网页、博客或者APP的web软件。我们想说，wordpress不仅是免费的，而且是非常有趣的。

更多wordpress相关：[https://wordpress.org/about/](https://wordpress.org/about/),a敬请浏览wordpress官网。

**说了这么说，wordpress是到底是什么玩意？**

Wordpress是使用PHP语言开发的博客平台，支持在PHP和MySQL的服务器上架设自己的博客。

WordPress最大的“谎言”就是，它让那些没有任何基础的人(隔壁下岗工人张大妈，送快递初中没毕业的陈小蛋,楼下卖早餐的李大爷，餐厅里洗盘子的刘小花)天真地认为只要双击解压安装包，并且点击下一步，下一步，10分钟能就能搭建自己的第一个给力的网站。

### 选择wordpress理由

1.wordpress最大的特点就是开源，这使它有成千上万的人愿意贡献自己的力量使它变得更好。

2.wordpress拥有众多插件和主题，安装和使用都非常方便，及时你不动代码，你也可以很方便地使用它搭建出漂亮且强大的网站。

* 主题。网站主题一键更换，而且除了wordpress官网上会有很多免费的提供者，更有一些收费主题也挺不错。大家百度一下，就一大堆。
* 插件。网站插件一键添加，给你的网站添加不同的特色的功能。

3.前WordPress已不在是一个简单的Blog程序，你不仅可以使用它来搭建个人博客，还可以搭建其他常见类型的网站，比如门户、下载站、淘宝客、论坛、多博客等等。

4.使用WordPress，你不会再孤军奋战，不管你遇到什么问题，只要你百度或者Google一下，你就可以找到解决的办法。

### 搭建wordpress站点的条件

1.需要网站空间/VPS服务器。就像在[如何搭建网站？wordpress建站（一）](http://wayearn.com/2016/01/build-website-1/)中所述的那样。

> 市面上常见的主机空间有 Windows主机 和 Linux主机：

> Windows主机，顾名思义，是在服务器上安装了服务器版本的Windows系统，比如windows2003。这种主机，一般是使用自带的IIS来配置网站运行的环境。windows主机，市面上常称之为全能主机，支持 ASP、PHP 等多种语言编写的建站程序。当然，一般也安装了MySQL数据库环境。

> Linux主机，即安装了Linux核心系统的主机。这种主机，一般独立安装 Apache, MySQL, PHP三大组件来搭建网站运行的环境。Linux主机不支持ASP等语言，通常都只支持PHP语言的程序。
> 
> PS:在[网站维护](http://wayearn.com/category/wordpress/weihu/)分类下，我们有详细的介绍如何在Linux上安装Wordpress的运行环境LAMP。

**选择Linux主机而不选择Windows主机的主要原因**是：如果你使用windows主机，你会发现，运行Wordpress感觉比较慢，而且通常不能完美支持伪静态，而且网址中有中文的话，就会出现404错误，有时候还没办法使用某些插件……虽然有些问题可以通过修改配置勉强实现，但是对于一个新手来说，几乎是没办法折腾的！

2.需要MySQL数据库，并学习使用类似phpmyadmin/Webmin的配置软件或者简单的Linux命令配置方法。

3.Vps/网站空间需要多大空间与数据库？

通常，wordpress博客而言，最低配置的就行了。

那区别的地方有几点：

* 服务商的口碑：价格，性能。
* 操作后台面板的难易层度。
* 具体个人建站的需求：影视站、购物站、广告站、论坛、下载站等。

确定了如上的需求之后，就可以选择：

* CPU核数
* 硬盘大小/SSD块数
* 内存多少
* 流量及带宽的多少

一般而言，主机空间有三种选择：虚拟主机->VPS->服务器，对于新手或个人博客而言，博主推荐先购买VPS，因为建站初期（一年内）你网站的流量都不会很大，一个200M左右的虚拟主机虽然就完全够一个普通博客使用一年以上了，但是，**虚拟主机/网站空间/共享空间的服务器一般都很渣（siteuptime网站在线时间不足99%，时不时宕机），而且特别是国外的（便宜的）服务商，经常每台服务器被挂上了N多网站，网站的流量及访问速度得不到保证，安全性更得不到保证。**

说了这么多，那这样的一个VPS服务器需要多少钱。拿[Vultr](http://www.vultr.com/?ref=6862890)最低配置的768M 1核 15GB硬盘 1T流量的来说，一个月5刀，一年60刀，大概340元。加上一个域名一年55元（[万网](wanwang.aliyun.com)，推荐码[6a7gwj]），两项费用一共400元左右。

如果你目前只是先学习一下WordPress，不打算建站那么快，那你完全可以在自己的电脑里安装PHP环境，本地搭建WordPress，这样你就没必要花钱那么快啦！如何搭建？请阅读[使用WampServer搭建本地PHP环境，绑定域名，配置伪静态](http://wayearn.com/2016/01/wamp/)

### 如何安装wordpress?

so simple, so easy.

Linux主机用户请看视频：
[WordPress安装视频教程](http://wayearn.com/2016/01/wordpress%E5%AE%89%E8%A3%85%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B/)

步骤：

1.下载最新的wordpress，[下载地址](https://wordpress.org/latest.zip/)

2.使用phpmyadmin或者MySQL命令新建一个用户admin，并建立对应的数据库adminDB，给予该用户权限(All privilege)。

    比如这里我的域名为 demo.wayearn.com ，新建的数据库信息如下：
    数据库名：wesql
    数据库用户名：wesql
    数据库密码：123456
    主机：localhost （没有特殊说明，一般都是localhost）

3.访问VPS供应商提供的IP（如果已经通过域名解析了，那就可以直接访问域名，参考[如何搭建网站？wordpress建站（二）](http://wayearn.com/2016/02/%E5%A6%82%E4%BD%95%E6%90%AD%E5%BB%BA%E7%BD%91%E7%AB%99%EF%BC%9Fwordpress%E5%BB%BA%E7%AB%99%EF%BC%88%E4%BA%8C%EF%BC%89/)）

4.出现安装向导，点击“创建配置文件”->“现在开始”->如下填表->提交->进行安装

    数据库名 wesql
    用户名 wesql
    密码 123456
    数据库主机 localhost
    表前缀 we_

5.填写网站的基本信息

    站点标题 wayearn
    用户名 demo
    输入两次密码 （使用密码器生成）
    您的电子邮件 i@wayearn.com

6.安装成功。



