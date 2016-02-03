我们来做一个WP子主题：

1.copy父主题的文件

2.保留style.css和functions.php

3.修改style.css并清空functions.php

style.css文件修改成如下文件：

    /*
    Theme Name:     Twenty Ten Child
    Theme URI:      http: //example.com/
    Description:    Child theme for the Twenty Ten theme 
    Author:         Your name here
    Author URI:     http: //example.com/about/
    Template:       twentyten
    Version:        0.1.0
    */

    Theme Name. (必需) 子主题的名称。
    Theme URI. (可选) 子主题的主页。
    Description. (可选) 子主题的描述。比如：我的第一个子主题，真棒！
    Author URI. (可选) 作者主页。
    Author. (optional) 作者的名字。
    Template. (必需) 父主题的目录名，区别大小写。 注意： 当你更改子主题名字时，要先换成别的主题。
    Version. (可选) 子主题的版本。比如：0.1，1.0，等。