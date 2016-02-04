##如何搭建网站？wordpress建站（二）

### 域名注册

现在的域名注册都很方便，主要说一点：**选择口碑好的服务商**

对比一下两家的现状：

1.价格对比

2016年万网与Godday服务价格对比

[table id=2 /]

**从价格上来看，万网完胜Godaddy。**

2.服务对比

万网：7X24小时售后电话咨询服务 400 600 8500（支持工单）

Goddaddy:中国: 11:00-17:00, 周一至周五(86) 400-842-8288

**从服务上来说，万网的优势是服务时间长，但是别忘记了中国人多啊！**

3.安全性对比

万网：在阿里云旗下的万网，遗传了阿里的安全特性：手机绑定，虚拟MFA，操作保护，密保问题。

Godaddy:短信两步验证

**从安全性上来讲，可以说阿里的安全性更好一些**

4.万网有什么不好的呢？

有人说，放在万网不放心，不小心ZF哪天就把你的站给毙了！你不做违法乱纪的事，你担心这干嘛？



### DNS选择与设置

一般来说域名服务商都会提供DNS解析服务，但是在国外买的域名的同学可能就会感受到网站经常打不开，因为可能域名服务商的DNS服务器被GFW（Great Firewall of China）给墙了。

所以，建议国内的站长考虑使用国内的DNS服务商，首推的是DNSpod。

官网：[http://dnspod.cn/](http://dnspod.cn/)

本站就是使用的DNSpod的服务。使用方法：


（1）首先，需要注册一个DNSPOD的账号。（如果有更高的安全需要，可以设置两步验证）

（2）在DNSPod面板中添加你的域名


（3）设置域名商的DNS（NameServers）为DNSPod的DNS地址
    
    免费DNS地址：f1g1ns1.dnspod.net/f1g1ns2.dnspod.net （对应 10 台服务器）；
    豪华DNS地址：ns1.dnsv2.com/ns2.dnsv2.com （对应 12 台服务器）；
    企业I DNS地址：ns1.dnsv3.com/ns2.dnsv3.com （对应 14 台服务器）；
    企业II DNS地址：ns1.dnsv4.com/ns2.dnsv4.com （对应 18 台服务器）；
    企业III DNS地址：ns1.dnsv5.com/ns2.dnsv5.com （对应 22 台服务器）；
    个人专业版DNS地址：ns3.dnsv2.com/ns4.dnsv2.com （对应 12 台服务器）；
    企业创业版DNS地址：ns3.dnsv3.com/ns4.dnsv3.com （对应 14 台服务器）；
    企业标准版 DNS地址：ns3.dnsv4.com/ns4.dnsv4.com （对应 18 台服务器）；
    企业旗舰版 DNS地址：ns3.dnsv5.com/ns4.dnsv5.com （对应 22 台服务器）。

（4）添加A记录到你的空间/VPS服务商提供的IP地址

（5）Ping/访问你的域名，看看解析是否正常。

资料参考：

DNSPod的DNS地址是什么？
 
万网注册的域名如何使用DNSPod解析？
新网注册的域名如何使用DNSPod解析？
Godaddy注册的域名如何使用DNSPod？
易名中国注册的域名如何使用DNSPod？
琥珀网注册的域名如何使用DNSPod？
新网互联注册的域名如何使用DNSPod？
EnomCentral注册的域名如何使用DNSPod？
NameCheap注册的域名如何使用DNSPod？
 


