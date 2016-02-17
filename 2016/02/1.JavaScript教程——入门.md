本教程是JavaScript教程，结合一些程序例子，让你从基础入门到掌握JavaScript(简称JS)。学习本教程，你需要有HTML与CSS基础知识，并在第一课之后，动手实践，才能更好的掌握JavaScript。

##JavaScript入门

### JavaScript简介

JavaScript是世界上最流行的脚本语言，因为你在电脑、手机、平板上浏览的所有的网页，以及无数基于HTML5的手机App，交互逻辑都是由JavaScript驱动的。

简单地说，**JavaScript是一种运行在浏览器中的解释型的编程语言。**

自己可以脑补：[解决型语言](http://baike.baidu.com/link?url=IdGBzXuI2g8KnK1tW8kHY1JHdRPl_Bcr-ZRdHkWx7C7OP97thwgBaW45DSPUZ4FLdJFIq6ZeZAHmh4RQ6Xqf5K)

> JavaScript诞生于1995年。当时，它的主要目的是处理以前由服务器端语言负责的一些输入验证操作。在JavaScript诞生之前，都需要把表单数据发送到服务器端才能确定用户是否有没有填错，或者是没有填。而当时的网络还是电话拨号上网的年代，传送一次数据可能需要几分钟，甚至几十分钟。JavaScript的诞生，就是人们在客户端上快速验证表单的需求下诞生的。

### 建立运行/调试环境

学习JS，你不用刻意的去下个什么类似eclipse，Zen Studio的运行环境。

JS存在于我们的花花网络世界中，它无处不在~~~~

现在一般的浏览器（Chrome，Firefox，IE等）都会有[调试窗口]，在调试窗口中运行JS代码。

**怎么打开？很简单按“F12”。**

你可以看到了一个“console”/“调试窗口”，会出现一个">"，在那里你就可以输入JS代码了，回车运行，So easy！

### 在HTML中使用JavaScript

1.JavaScript包含在一对<script></script>标签中

    <script>
    function a(){
        alert("hello world");
    }
    
    a();
    </script>

2.JavaScript代码可以直接嵌在网页的任何地方。

不过通常我们都把JavaScript代码放到<head>中：

    <html>
    <head>
      <script>
        alert('Hello, world');
      </script>
    </head>
    <body>
      ...
    </body>
    </html>

3.可以把JavaScript代码放到一个单独的`.js`文件，然后在HTML中通过script的`src`属性引入这个文件：

    <html>
    <head>
      <script src="../js/demo.js"></script>
    </head>
    <body>
      ...
    </body>
    </html>

把JS代码放到单独的文件，而不是放在head中，主要是这样更利于代码的维护，并且可以多个页面多次引用同一js代码文件。

4.在同一个页面中引入多个`.js`文件，还可以在页面中多次编写`<script> js代码... </script>`，浏览器按照顺序依次执行。

    5.`<noscript>`元素，当浏览器不支持JavaScript时，使用`<noscript>`元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本的情况下，浏览器不会显示`<noscript>`元素中的任何内容。

    <html>
    <head>
      <script src="../js/demo.js"></script>
    </head>
    <body>
      ...
      <noscript>
         <p>本页面需要浏览器支持（启用）JavaScript。</p>
      </noscript>
    </body>
    </html>


### 编辑器推荐

可以用任何文本编辑器来编写JavaScript代码。这里我们推荐以下几种文本编辑器：

Sublime Text

免费，但不注册会不定时弹出提示框。

Notepad++

免费

**注意：**不可以用Word或写字板来编写JavaScript或HTML，因为带格式的文本保存后不是纯文本文件，无法被浏览器正常读取。


