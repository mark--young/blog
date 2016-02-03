## 实现Bootstrap导航条可点击和鼠标悬停显示下拉菜单

使用Bootstrap导航条组件时，如果你的导航条带有下拉菜单，那么这个带下拉菜单的导航在点击时只会浮出下拉菜单，它本身的href属性会失效，也就是失去了超链接功能，这并不是我想要的，我希望导航条的链接可以正常打开它的链接，但又需要下拉菜单功能，开始折腾~

### 1.导航条可点击解决方法

首先解决带下拉菜单的导航条可以点击问题，下拉菜单效果是JS实现的，分析bootstrap.js文件发现，Bootstrap把下拉菜单写成了一个JQuery插件，在dropdown代码段中找到了关键的几句：

    // APPLY TO STANDARD DROPDOWN ELEMENTS
    // ===================================
    
      $(document)
        .on('click.bs.dropdown.data-api', clearMenus)
        .on('click.bs.dropdown.data-api', '.dropdown form', function (e) { e.stopPropagation() })
        .on('click.bs.dropdown.data-api'  , toggle, Dropdown.prototype.toggle)
        .on('keydown.bs.dropdown.data-api', toggle + ', [role=menu]' , Dropdown.prototype.keydown)

找到几句关键代码后，想到了解决办法，只要把其中click.bs.dropdown.data-api事件关闭就OK了，代码如下：

**解决办法：插入以下JS代码**

    $(document).ready(function(){
    	$(document).off('click.bs.dropdown.data-api');
    });

### 2.悬停显示下拉菜单

下面解决鼠标悬停弹下拉菜单问题，这个相对简单些。

用JQuery的鼠标事件就可实现，代码如下：

    $(document).ready(function(){
    	dropdownOpen();//调用
    });
    /**
     * 鼠标划过就展开子菜单，免得需要点击才能展开
     */
    function dropdownOpen() {
    
    	var $dropdownLi = $('li.dropdown');
    
    	$dropdownLi.mouseover(function() {
    		$(this).addClass('open');
    	}).mouseout(function() {
    		$(this).removeClass('open');
    	});
    }


### wordpress进阶用法

1.使用子主题来修改父主题

方法参见：

**Wordpress官方:**

Child theme:[https://codex.wordpress.org/zh-cn:%E5%AD%90%E4%B8%BB%E9%A2%98](https://codex.wordpress.org/zh-cn:%E5%AD%90%E4%B8%BB%E9%A2%98)

2.使用hooks挂钩wp_enqueue_scripts加载JS代码

举个例子：

把上面的两个代码的js加载到你的主题里面去：

**JS代码部分**

-----

    $(document).ready(function(){
    	$(document).off('click.bs.dropdown.data-api');
    	dropdownOpen();//调用
    });
    
    /**
     * 鼠标划过就展开子菜单，免得需要点击才能展开
     */
    function dropdownOpen() {
    
    	var $dropdownLi = $('li.dropdown');
    
    	$dropdownLi.mouseover(function() {
    		$(this).addClass('open');
    	}).mouseout(function() {
    		$(this).removeClass('open');
    	});
    }

**PHP部分**

-----

    function add_scripts(){
      wp_enqueue_script('bootstrap-dropdownjs', get_stylesheet_directory_uri().'/js/bootstrap-dropdown.min.js', array('jquery') );
    }
    add_action( 'wp_enqueue_scripts', 'add_scripts' );
     ?>


最终网页的效果：





引用：

* 使用 WordPress 的子主题（Child Themes）功能快速制作自己的主题[http://blog.wpjam.com/article/child-themes/](http://blog.wpjam.com/article/child-themes/)
* 使用WORDPRESS的子主题功能修改你的WORDPRESS主题[http://www.wpdaxue.com/wordpress-child-themes.html](http://www.wpdaxue.com/wordpress-child-themes.html)



### 3.关于wordpress常见的url

方法参见：wordpress各种获取路径和URl地址的函数总结 [http://wayearn.com/2016/02/wordpress-url-functions/](http://wayearn.com/2016/02/wordpress-url-functions/)


