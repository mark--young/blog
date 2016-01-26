本文介绍了，如何搭建shadowsocks实现翻墙/SS搭建教程/ss翻墙/翻墙教程，科学上网。浏览国外的知名网站google,facebook,youtube等等的方法。你还在使用Goagent苦苦找寻的IP吗？你还在使用不稳定的VPN（NydusVPN,GreenVPN）吗？你还在为不能上google学术懊恼吗？通过本文，你可以学习一种更加完全的方案，即安全速度也稳定。你还在为不能打美服游戏而抓狂吗？在这里，你可以找到解决方案。

**需要已经购买了VPS服务（下面介绍），一台windows系统电脑和一颗耐心（一步步按着教程来做）。**

	Shadowsocks → 简称SS
    DigitalOcean→ 简称DO

## 前言
世界这么大，你想不想去看看？Google.com全球最大的搜索引擎，facebook.com全球社交平台，还在看youku,tudou? 你看过youtube没？看看美国的“城里人”怎么玩？

打开浏览器，当你输入google.com回车时：

>404，您访问的页面不存在

打开浏览器，在搜索框中搜索XXX，百度提示：

>根据相关法律法规和政策，部分搜索结果未予显示。

你心里是不是有百万匹草泥马在奔腾？意气风发的说，“爷我又没有干什么伤天害理的事情，只是有一颗求知的心，怎么给我屏蔽了？”

遥想当年学习上机课，老师还照着教科书，告诉大家google.com是多么神奇的一个网站，你想知道的东西里面全都有。如今，却...

## Shadowsocks特点

**对于手机端APP：**

1. 省电；

2. 支持开机自启动，且断网无影响，无需手动重连，方便网络不稳定或者3G&Wi-Fi频繁切换的小伙伴；

3. 可使用自己的VPS服务器，安全和速度的保证；

4. 支持区分国内外流量，传统VPN在翻出墙外后访问国内站点会变慢；

5. （安卓）手机APP可对应用设置单独代理，5.0之后的系统无需root。

**对于电脑端：**

1. 支持开机启动。

2. 支持建立局域网连接，全家科学上网。

3. 支持全局模式，所有应用全都可以科学上网。 

>随机启动并24小时后台运行，占内存6.6MB以内，基本不怎么耗电，跟人直接置身墙外使用手机的感受差不多。

## VPS推荐与支付

Shadowsocks的正常使用需要服务端，其实所有的翻墙方式都需要服务端，搭建服务端需要你拥有一个属于自己的VPS。下面是我自己精挑细选出来的三家VPS供应商，如果你坚持认为我是在给这三家VPS打广告，你就不用往下看了。这三家我都在用，感觉不错，当然你也可以选择其他家的VPS产品。

支付宝购买搬瓦工VPS详细教程：点击上面的优惠链接后，在“Billing Cycle”的下拉菜单里选择“$19.99 USD Annually”或者“$49.99 USD Annually”，“Location”保持默认即可，然后点击“Add to Cart”按钮，网页跳转后，点击“Checkout”，在新页面的“Your Details”部分输入你的个人资料以及注册信息，除了“Address 2”和“Company Name”之外，其余均为必填项目，注意，尽量填写真实信息，如果你在中国大陆，“Country”请选择“China”，以免被系统判定为欺诈而注册失败。“Payment Method”里选择“Credit Card and AliPay (Stripe)”，勾选“I have read and agree to the Terms of Service”，最后点击“Complete Order”，页面跳转后，点击绿色的大按钮“Pay now”，然后会弹出一个小窗口，先在“电子邮箱”一栏里输入你的支付宝账号（此处必须为邮箱账号），然后点击下方的“支付宝”按钮，此时系统会自动给你支付宝账号绑定的手机号发送一条带验证码的短信，在“输入发送至xxxx的校验码”下方输入6位数的短信校验码，然后在最下面的“身份证号码后5位”框里输入你的身份证号后5位，最后点击蓝色的“Pay now $xx”按钮，完成支付。

注：搬瓦工域名在部分地区被墙，可能需要翻墙访问，但在上面购买的VPS不受影响。

Linode只能使用信用卡支付，官方会随机手工抽查，被抽查到的话需要上传信用卡正反面照片以及可能还需要身份证正反面照片，只要材料真实齐全，审核速度很快，一般一个小时之内就可以全部搞定。账户成功激活以后，就可以安心使用了。

DigitalOcean和搬瓦工两家的VPS都支持PayPal付款，DigitalOcean也可以选择在账单里绑定信用卡进行支付。

值得说明的是无论是注册这三家VPS还是注册PayPal，尽量填写真实信息，这样一旦遇到审核会更容易通过，注册的时候遇到国家地区一定要如实选择你所在的真实地区如“China”，以防被系统判定为欺诈。

2015年6月26日更新：搬瓦工目前已经支持使用支付宝付款，支付时请注意选择带AliPay字样的支付方式。

>关于支付的重要补充说明(必看！)：有小伙伴反映PayPal绑定银联借记卡之后无法支付搬瓦工的VPS，DigitalOcean没问题，经过我调查之后，发现有人很顺利的就用银联卡支付成功，有人则死活不行，所以问题的具体原因不好说。但这里给出有效的解决方案：绑定信用卡。

>没有信用卡的学生党可以[点击这里](http://dwz.cn/wbZHy) (此处为官方提供的短链接，网址安全，可放心使用，是一家在国内非常知名的虚拟信用卡服务商)申请一张虚拟信用卡，注册快速，可用普通网银充值，经楼主实测，可顺利支付搬瓦工。而且该卡还可以绑定Google Wallet，Google Play上面的软件和硬件可随便买（提醒：Google Wallet有一套很复杂的安全检测机制，无论你购买什么东西，如果不小心遇到订单被取消的情况，那都很正常，这是题外话了）。此外， DigitalOcean审核较为严格，尤其是对于选择了信用卡作为支付方式的用户，可能会要求你上传身份证明以及信用卡照片什么的，而且审核过程也需要等待，如果你怕麻烦，我建议你直接使用PayPal支付，方便快捷。通过审核开始正式使用后，一般就没什么问题了，多点耐心。

## VPS深入说明与选择
几家的对比：

(1)OpenVZ为不完全虚拟化技术，每个VPS账户共享母机内核，易受同一母机下其他VPS的影响，几乎不能单独修改内核。Xen和KVM为完全虚拟化技术，各VPS之间互相独立，基本互不影响，而且可以任意修改内核。

>这三种架构对我们搭建shadowsocks服务器来讲最直观的区别就是，Xen和KVM可通过系统内核修改来优化服务器，大幅度提升shadowsocks的连接速度，尤其体现在晚高峰的时候。

我在同一时间段用100MB的文件简单的在自己的三台VPS上面测试了一下shadowsocks的连接速度：

* Vultr的平均速度在200k/s左右，与Do可以说不相上下，但是DO最近被墙的厉害，再考虑到Vultr对日本节点的优化，并且在2015年的最后一天，Vultr把日本节点的400GB流量提升到了1TB，News:[https://www.vultr.com/news/Happy-New-Year-from-Vultr.com/](https://www.vultr.com/news/Happy-New-Year-from-Vultr.com/),那么对于DO来说，Vultr性价比完爆啊。

* DigitalOcean（5美元/月）的平均下载速度稳定维持在100k-200k/S左右，这个速度看一下youtube 480p应该还需要缓存一下。**而且，就像我在另一篇[博客](http://wayearn.com/2016/01/%E5%A6%82%E4%BD%95%E5%BB%BA%E7%AB%99/)里面写到那样，Do对于国人来说现在要求很多，限制也多，而且很多地方DO的css文件被墙了，你会发现，当你打开DO的管理面板，显示不正常。但是，DO也有它的好处，就是它的[社区](https://www.digitalocean.com/community/)还不错，里面的技术资料很全。**

* Linode（10美元/月）的上传下载速度均达到带宽满载，官方给出的数据是“40 Gbit Network In / 125 Mbit Network Out”，由于本人没有亲身试验，借我一个朋友传给我的截图，看720p的youtube，应该微缓存一下就行了，一分价钱一分货。Linode除了速度快之外，还有一个杀手锏就是提供日本节点，ping值70ms以内，有超低网络延迟需求的小伙伴可以重点考虑下。

* 搬瓦工（19.99美元/年）的平均下载速度在0-200KB/S之间，也就是说速度表现不是很稳定，YouTube 240p几乎看不了，**如果你只是翻墙了，为了看google，那你就别用bandwagon了，因为国人大批的涌入，bandwagon也很贼，搞了一个一键shadowsocks,全是界面操作，很快可以搭建ss，但是速度很不稳定（亲身试验）**。


**个人建议，对连接速度和稳定性尤其是网络延迟有极高要求的首选Linode（只有最快，没有更快），有较高要求的推荐Vultr（一分价钱一分货），对于普通用户来讲，搬瓦工就可以（基本满足要求）。**

## VPS节点的选择

1.Vultr测试：
[https://www.vultr.com/faq/#downloadspeedtests](https://www.vultr.com/faq/#downloadspeedtests)

2.DigitalOcean各节点测试域名：

* San Francisco： [speedtest-sfo1.digitalocean.com](speedtest-sfo1.digitalocean.com)
* 新加坡：        [speedtest-sgp1.digitalocean.com](speedtest-sgp1.digitalocean.com)
* New York：      [speedtest-ny1.digitalocean.com](speedtest-ny1.digitalocean.com)
* Amsterdam：     [speedtest-ams1.digitalocean.com](speedtest-ams1.digitalocean.com)
* 英国伦敦：      [speedtest-lon1.digitalocean.com](speedtest-lon1.digitalocean.com)

3.Linode各节点测试域名：
[https://www.linode.com/speedtest](https://www.linode.com/speedtest)

4.搬瓦工各节点测试IP：

* Los Angeles：   104.194.78.3
* Florida：       74.121.150.3
* Phoenix：       198.35.46.2（可在控制面板里切换到这个机房）


请在CMD下自行使用“ping IP -t”或“ping 域名 -t”命令来测试不同位置的机房与你的电脑之间的ping值以及丢包率（Ctrl+C退出测试）。

如果还是不知道该选择哪个节点的小伙伴,Vultr一般选择Japan&Los Angeles
节点，DigitalOcean一般选用Japan & San Francisco节点居多(都在美国西海岸)，而Linode一般选择“Tokyo,JP”(日本节点)或者“Fremont,CA”(美国西海岸)，由于Linode日本节点ping值很低(70ms左右)、销售火爆，可能会一时无货，如果遇到无货，等一会再试试(我也是刷新了一会就有了),搬瓦工一般选用Los Angeles节点居多。一开始节点选择的不理想也不要紧，以后还可以方便的切换机房。

### VPS的创建与使用

1.Vultr & DigitalOcean & Linode

无论在上面三家的哪一家，建一个虚拟机，因为都是傻瓜式的操作，一看就懂的。

（1）Vultr → Price → 选择自己需要的配置 → 点击“Deploy” → 依次选择：

* Server Type（默认）
* Location （Japan & Los Angeles）
* Operating System (建议Debian7 32bit)
* Server Size （建议5$/月的低配置）

（2）DigitalOcean主页上就有告诉你如何建立一个新的Droplet。**（Do把虚拟机称为Droplet）**

（3）Linode类似Vultr（不再赘述）

**注册完毕后，你已经获得了你VPS的IP，SSH端口（默认22），root密码。**

2.使用putty登陆到你的VPS
下载Putty.【[下载链接](http://www.putty.org/)进去后点击Download】

