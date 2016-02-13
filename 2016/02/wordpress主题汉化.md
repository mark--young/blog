##wordpress主题汉化

本教程介绍了如何汉化wordpress主题，使wordpress主题支持语言本地化，主要流程为：让主题开启语言本地化功能，然后使用符合WordPress API 规范的写法来撰写需要翻译的内容，接着使用 [poedit](https://poedit.net/) 生成语言包。

###如何导入语言包？

在主题的 functions.php 文件添加下面的代码：

    add_action('after_setup_theme', 'my_theme_setup');
    function my_theme_setup(){
        load_theme_textdomain('wayearn', get_template_directory() . '/languages');
    }

* 使用add_action添加钩子，`after_setup_theme`钩子，请参考：[http://codex.wordpress.org.cn/Plugin_API/Action_Reference/after_setup_theme](http://codex.wordpress.org.cn/Plugin_API/Action_Reference/after_setup_theme)，当主题安装之后，在每个页面中都会调用函数`my_theme_setup()`。
*  `load_theme_textdomain()` 函数来定义主题的语言路径，其中包含了两个参数，第一个“`wayearn`”是`textdomain` ，第二个“`get_template_directory() . '/languages'` ”则指明主题的语言存放路径为 当前主题的 languages 目录。只要将语言包存放在这个目录，就可以生效。

###指定需要翻译的内容

要让你的主题的文本内容支持自动翻译，需要你做好规范符合 WordPress API 的书写。WordPress常用下面几个函数来包裹需要翻译的内容：

* `__()`
* `_e()`
* `_x()`
* `_ex()`
* `_n()`


WORDPRESS翻译中 `__()`、`_E()`、`_X`、`_EX` 和 `_N` 的用法及区别：

以上所列的函数是用来包含所需翻译的字符串的，根据字符串的不同参数和输出类型，需要使用不同的函数。

**1.`__()` 和 `_e()`**

`__()` 和 `_e() `都是用来返回对应当前语言的字符串内容。请看下面的例子：

使用 `__()`：

    <?php  
    if( is_single() ) { //如果这是一篇“文章”  
        echo __( 'This is a post.' );  
        _e('this is my first post','wayearn'); // 指定PO本地翻译包，即textdomain()函数的第一个参数
    }  
    ?>

上面两组代码的最终输出内容都是一样的。请自己对比一下这两组代码，使用了 `echo` 函数的，就用 `__()`，直接返回内容的就用 `_e()` 。由此，我们可以简单理解为：

如果字符串是返回给其他函数调用，不打印出来，就用 `__()` ；直接打印输出到 html 中的字符串，就用 `_e()` 。

再看下面的例子：

    <?php  
    the_content( __( 'Click here to read more' ) );  
    ?>

`'Click here to read more'` 是被函数 `the_content()` 直接包裹的，所以这里使用` __()`；如果你换成 `_e()` ，它就会直接输出“`Click here to read more`”，函数 `the_content()` 就不起作用了。

**2.`_x()` 和 `_ex()`**

如果要翻译的字符串是根据上下文来决定的，就需要用到 `_x()`。比如“Post”根据上下文的不同既可以指 “a post(名词)” 也可以指 “to post (动词)”。你需要明确表达同一个词的不同含义，方便翻译者识别。 `_x()` 主要是用在单个具有多重用法的词语，而且它比其他的编译函数多了一个额外的参数，这个参数就是用来根据不同上下文显示的不同内容。

比如，你在主题的两个地方用到了“post”这个词，但是这两个地方的要表达的意思是不一样的。这时候，你可以这样操作：

在第一处的post使用下面的代码：
    
    <?php echo _x('post','A post.') ?>

在第二处的post使用下面的代码：
    
    <?php echo _x('post','To post.') ?>

那么，翻译者就可以理解两处post的不同，将第一处翻译为“一篇文章”，第二处翻译为“发布”。

注：导出的 .pot 语言包文件，使用 POEdit 软件打开后，你会发现有两条翻译条目分别为：post[A post] 和 post[To post]，这样就可以分别翻译这两个意思。

`_ex() `区别于 `_x()` 的地方，和 `_e()` 区别于`__()` 是一样的。前者是用于直接打印输出到html的字符串，后者用于返回字符串以供其他函数调用，不打印输出。

**3.`_n()`**

`_n()` 是用来进行单复数编译的。比较常见于 WordPress 的评论功能模块。例如下面的两组代码输出的内容都是一样的：

代码块1：

    <?php  
    if(get_comments_number() == 1) {  
        _e( 'There is a comment' );  
    } else {  
        _e( 'There are comments' );  
    }  
    ?>

代码块2：

    <?php  
    echo _n( 'There is a comment' , 'There are comments' , get_comments_number() );  
    ?>

第一组代码是通过 `if` 来判断，如果评论数量是 `1` ，就输出“There is a comment ”，否则输出“There are comments”，由于是直接输出的，所以使用 `_e()` ；第二组是使用`echo` 函数输出，然后使用 `_n()` 来区分1条评论和多条评论的显示内容。

`_n()` 有 3 个参数。第一个是单数形式的字符串，第二个是复数形式的字符串，第三个是引用的数字。在这个例子中，`get_comments_number() `是用来获取 评论 的条数，提供给 `_n()` 使用。

4.含有变量的翻译

如果在翻译的字符串中包含一个额外的函数或变量，我们应该如何处理呢？比如下面的例子：

    <?php  
    $color = the_color();  
    _e( "You have chosen the $color theme" );  
    ?>

如果你通过 POEdit 制作 .pot 文件，这时会报错。因为在翻译的字符串中包含了 $color 这个变量。那么，如何规避这个问题呢？下面有2种方法：

* 拆分内容，使用`.`运算符

如下代码：

    <?php  
    $color = the_color();  
    echo __( 'You have chosen the ' ) . $color . __( ' theme.' );  
    ?>

仔细看第 3 行代码，使用了 echo 函数，这样就能保证 $color 变量的输出，同时将变量两端的内容分别进行编译，中间使用点号“.”相连。这样一来，只需翻译两端的内容即可。

* 使用 `printf()` 或 `sprintf()`

上面说的“拆分内容”法虽然可以实现我们要的效果，但是它将内容分成几段，需要我们分别翻译，多少有些繁琐。其实我们可以使用 `printf()` 或 `sprintf()` 来解决这个问题。看下面的代码：

    <?php  
    //使用 sprintf() 的写法
    echo sprintf( __( 'You have chosen the %s theme.' ) , the_color() );
     
    //使用 printf() 的写法 
    printf( __( 'You have chosen the %s theme.' ) , the_color() );  
    ?>

`printf()` 与 `sprintf()` 用法的区别，和 `_e()` 与` __()` 的区别是一样的。

**在实际使用中，我们还需要注意以上函数的末位参数 。也就是在上一步中，我们通过 `load_theme_textdomain() `函数定义了第一个参数 `$domain` 为“`wayearn`”，这个参数都需要添加到 `__()` 等函数中作为末位参数，它是用来检索被翻译字符串的唯一标识符。**

### 使用 POEdit 制作语言包

POEdit 是一款非常有用的语言包制作软件，你可以在 [POEdit](https://poedit.net/) 官方下载，安装好以后，就是中文界面了。下面就简单演示一下操作过程。

####新建编目
点击 文件 > 新建编目，会出现如下界面：

1.在”翻译属性”中按照下面的范例添加信息：

需要注意的是，“语言”就是输出语言的简码，比如简体中文为 zh_CN （注意大小写），然后字符集一般选择 UTF-8，复数形式一般默认或者填 nplurals=2; plural=(n!=1); [了解复数形式](http://poedit.net/trac/wiki/Doc/PluralForms)即可。

2.切换到“源路径”，由于前面我们创建的语言包路径为当前主题的 languages 目录，所以这里我们添加相对路径 ../ 即可，如下图：

3.添加“源关键字”，这个关键字就是要识别上面的几个翻译函数。

需要注意的是，_n、_x 和 _ex 这三个函数要添加对应的参数才能实现其功能，建议对应的写法为

* `__`
* `_e`
* `_n`
* `_n:1,2`
* `_x:1,2c`
* `_ex:1,2c`
* `esc_attr_e`
* `esc_attr__`
* `esc_attr_x`
* `_nx:4c,1,2`

更多知识，请点击：[了解gettext关键字](http://poedit.net/trac/wiki/Doc/Keywords)

填写完以后，点击“确认”，就创建好了编目。

####导入需要翻译的字符串

点击“更新”按钮，如果你前面的步骤没有出错的话，就会自动搜索主题文件中需要翻译的条目，如下图：

#### 翻译字符串


翻译完以后，将语言包保存到主题的语言目录 languages 中，这里特别要注意语言包的命名。它是使用 `Gettext` 代码来命名的，比如中文的 `Gettext` 语言代码为 `zh` ，国别代码为 `CN`，所以最终保存的简体中文语言包为 `zh_CN.po`，POEdit 会自动生成一个名为` zh_CN.mo` 的文件。

po 和 mo 的最直接的区别在于：po文件是给人看的，也是可以直接通过 POEdit 编辑的，mo 文件则是给服务器识别的，是用来显示翻译内容所必需的。也就是说，你的主题语言目录中，可以没有po文件，但是必须有mo文件，否则服务器就没办法加载翻译！

你可以通过下面的链接了解更多 Gettext 代码：

* [Gettext 语言代码](http://www.gnu.org/software/gettext/manual/gettext.html#Language-Codes)
* [Gettext 国别代码](http://www.gnu.org/software/gettext/manual/gettext.html#Country-Codes)

###让WordPress识别语言包

通过上面的步骤，我们已经创建好了语言包，那么WordPress如何才能识别语言包？打开WordPress根目录下的 `wp-config.php `文件，找到 `WPLANG`，如果这里填入的是 `zh_CN` ，说明你使用的是简体中文版本的 WordPress，那么主题也会自动调用简体中文语言包` zh_CN.mo` 。

    /**
     * WordPress 语言设置，中文版本默认为中文。
     *
     * 本项设定能够让 WordPress 显示您需要的语言。
     * wp-content/languages 内应放置同名的 .mo 语言文件。
     * 要使用 WordPress 简体中文界面，只需填入 zh_CN。
     */
    define('WPLANG', 'zh_CN');