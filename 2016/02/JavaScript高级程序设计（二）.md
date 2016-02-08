本教程是JavaScript教程，结合一些程序例子，让你从基础入门到掌握JavaScript(简称JS)。学习本教程，你需要有HTML与CSS基础知识，并在第一课之后，动手实践，才能更好的掌握JavaScript。

##基本概念

###语法

JavaScript的语法大量借鉴了C及其他类C语言（Java和Perl）的语法。因为，如果你是熟悉这些语言的开发人员，那么下面的内容你可以轻松的掌握。

1.区分大小写

变量名`test`与`Test`分别表示两个不同的变量。

2.标识符

所谓**标识符**，就是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照下列格式规则组合起来的一个或多个字符：

（1）第一个字符必须是一个字母、下划线（_）或一个美元符号（$）;

（2）其他字符可以是字母、下划线、美元符号或者数字。

> 按照惯例，标识符采用驼峰大小写格式，也就是第一个字母小写，剩下的每个单词的首字母大写，例如：
> 
> firstSecond

> myJavaScript
> 
> doMyTestDemo

3.注释

使用C风格注释，包括单行与多行。

    //单行注释

    /*
     * 多行注释
     * （块级）注释  
     *   
     */

4.严格模式

    "use strict";

在JS代码的顶部添加如上代码。

设立"严格模式"的目的，主要有以下几个：

- 消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为;
- 消除代码运行的一些不安全之处，保证代码运行的安全；
- 提高编译器效率，增加运行速度；
- 为未来新版本的Javascript做好铺垫。

另一方面，同样的代码，在"严格模式"中，可能会有不一样的运行结果；一些在"正常模式"下可以运行的语句，在"严格模式"下将不能运行。掌握这些内容，有助于更细致深入地理解Javascript，让你变成一个更好的程序员。

这一块，可以参考：[Javascript 严格模式详解-阮一峰](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)

5.语句

    var sum = a + b //即使没有分号也是有效的语句——不推荐
    var diff = a - b; //有效的语句——推荐

> 虽然语句的结尾的分号不是必需的，但我们建议任何时候都不要省略它。因为加上这个分号可以避免很多错误（例如不完整的输入），开发人员可以放心的删除多余的空格来压缩JS代码（如果没有“;”在结尾可能导致压缩错误）。加入分号也会提高代码运行性能，因为程序不必去花时间推测应该在哪里插入分号了。

其他例子：

使用C风格的语法的，马多条语句组合到一个代码块中。

    if(test){
        test = false;
        alert(test);
    } //推荐使用

    if(test)
        alert(test); //有效，但容易出错，不要使用

    if(test){
        alert(test); //推荐使用
    }

###关键字和保留字

**关键字**

[table id=5 /]

带“*”号上标的是第5版新增的关键字。

**保留字**

[table id=6 /]


第5版在非严格模式下运行的保留字缩减为下列这些：

[table id=7 /]

严格模式下，第5版以下保留字加以了限制：

[table id=8 /]


###变量

JS的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。换句话说，每个变量仅仅是一个用于保存值的占位符而已。定义变量是使用var操作符。

    var message;
    var message = "hi";
    message = 100; //有效，但不推荐

局部变量：

    function test(){
        var message = "hi"; //局部变量
    }
    test();
    alert(message); //错误！

message是在函数中使用var定义的。

如果是：

    function test(){
        message = "hi"; //全局变量 但是不推荐
    }
    test();
    alert(message); //"hi"
    

###数据类型

1.typeof操作符

typeof是用来检测给定变量的数据类型：

* undefined——如果这个值未定义
* boolean——如果这个值是布尔值
* string——如果这个值是字符串
* number——如果这个值是数值
* object——如果这个值是对象或者Null;
* function——如果这个值是函数

举几个例子：

    var message = "some string";
    alert(typeof message);  // "string"
    alert(typeof(message)); //"string"
    alert(typeof 95); // "number"

2.undefined类型

在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined，例如：

    var message;
    alert(message ==  undefined);//true

包含undefined值的变量与尚未定义的变量还是不一样的：

    var message;
    // var age;
    alert(message); //"undefined"
    alert(age); // 产生错误

3.Null类型

Null类型是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null值表示一个空对象指针，而这也正是使用typeof操作符检测null值时会返回"object"的原因，如下面的例子所示：

    var car = null;
    alert(typeof car); // "object"

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化化null而不是其他值。这样一来，只要直接检查null值就可以知道相应的变量是否已经保存了一个对象的引用，如下面的例子所示：

    if(car != null){
        //对car对象执行某些操作
    }

> 实际上，undefined值是派生自null值的，因此 ECMA-262 规定对它们的相等性测试要返回true.

    alert(null == undefined);  //true

4.Boolean类型

说明一点：true不一定等于1，而false也不一定等于0；

    var found = true;
    var lost = false;

另一个例子：

    var message = "hello javascript";
    //undefined
    var messageBoolean = Boolean(message);
    //undefined
    messageBoolean;
    //true

> 需要注意的是，Boolen类型的字面值true和false是区分大小写的。也就是说，True和False（以及其他的混合大小写形式）都不是Boolean值，只是标识符。

[table id=9 /]

理解上表，如下有个例子：

    var message = "Hello JavaScript";
    if(message){
        alert("Value is true!");
    }

5.Number类型

    var intNum = 55; //整数
    var octalNum1 = 070; //八进制的56
    var octalNum2 = 079; //无效的八进制数值——解析为79
    var octalNum3 = 08; //无效的八进制数值——解析为8
    var hexNum1 = 0xA; //十六进制——10
    var hexNum2 = 0x1f; //十六进制——31

在进行算术计算时，所有八进制、十六进制表示的数值都会被转换成十进制数值。

（1）浮点数值

所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。

    var float1 = 1.1;
    var float2 = 0.1;
    var float3 = .1; //有效，但不推荐；

> 保存浮点数值需要的内存空间是保存整数值的两位，因此ECMAScript会不失时机地将浮点数值转换为整数值。

    var float1 = 2.; //小数点后面没有数字——解析为2
    var float2 = 8.0; //整数——解析为8

用大写或者小写的字母E来表示数值乘以10的指数次幂。

    var float1 = 4.125e7；  //等于41250000；

同样可以用e表示法表示极小的数值，如`0.00000000000000007`，这个数值可以使用更简洁的`7e-17`表示。**ECMAScript会将那些小数点后面带有6个零以上的浮点数值转换为以e表示法表示的数值。浮点数值的最高精度是17位小数。但是计算时的精确度远远不如整数。**

> 例如：`0.1`加`0.2`的结果不是`0.3`，而是`0.30000000000000004`。这个小小的舍入误差会导致无法测试特定的浮点数值。

（2）数值范围

由于内存的限制，ECMAScript并不能保存世界上所有的数值。

最小值保存在`Number.MIN_VALUE`中，在大多数浏览器中，这个值是`5e-324`。

最大值保存在`Number.MAX_VALUE`中，在大多数浏览器中，这个值是`1.7976931348623157e+308`。

如果一个值超出JS的数值范围，这个数值将会自动转换成特殊的Infinity值。

在数值范围内的数，可以使用`isFinite()`来进行判断。如：

    var someNum = Number.MAX_VALUE + Number.MAX_VALUE;
    alert(isFinite(someNum)); // false

（3）NaN

NaN，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况。**任何数值除以NaN都会返回NaN。其次，NaN与任何值都不相等，包括NaN本身。**

针对NaN的特点，ECMAScript定义了`isNaN()`函数。

    alert(isNaN(NaN));//true
    alert(isNaN(8));//false (8是一个数值)
    alert(isNaN("8"));//false（可以被转换成数值8）
    alert(isNaN("red"));//true (不能转换成数值)
    alert(isNaN(true));//false（可以被转换成数值1）

（4）数值转换

有3个函数可以把非数值转换为数值：`Number()`,`parseInt()`和`parseFloat()`。第一函数，即转型函数`Number()`可以用于任何数据类型，而另两个函数则专门用于把字符串转换成数值。


-----

`Number()`函数的转换规则如下。

* 如果是Boolean值，true和false将分别转换为1和0。
* 如果是数字值，只是简单的传入和返回。
* 如果是null值，返回0。
* 如果是undefined，返回NaN。
* 如果是字符串，遵循以下规则：
    * 如果字符串中只包含数字（包括前面带正负号的情况），则将其转换为十进制数值，即"2"会变成2，"011"会变成11（注意：前面的零被忽略了）；
    * 如果是有效的浮点格式，如"0.10"，则将其转换为对应的浮点数值0.1(同样会忽略0)
    * 空字符串""转换成了0；
    * 十六进制格式，如"0xf"，则转换为相同大小的十进制数值。
    * 如果字符串中包含除上述格式之处的字符，则将其转换为NaN。
* 如果是对象，则调用对象的`valueOf()`方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的`toString()`的方法，然后再次依照前面的规则转换返回的字符串。

例子：

    var num1 = Number("Hello JavaScript!"); //NaN
    var num2 = Number(""); //0
    var num3 = Number("0000011"); //11
    var num4 = Number(true); //1

-----

`parseInt()`函数的转换规则，下面给出一些例子：

    var num1 = parseInt("123blue"); //123
    var num2 = parseInt("");  //NaN
    var num3 = parseInt("0xA"); //10 （十六进制转换为十进制）
    var num4 = parseInt(22.5);//22
    var num5 = parseInt("070"); //56(八进制转换为十进制)
    var num6 = parseInt("70"); //70
    var num7 = parseInt("0xf"); //15 (十六进制转换为十进制)

> 在使用`parseInt()`解析八进制字面量的字符串时，ECMAScript 3和5存在分歧。例如： ECMAScript 3 认为是56， ECMAScript 5认为是0；
> 所以为了避免如上问题，可以使用`parseInt()`函数的第二个参数，如下例子：

    var num = parseInt("0xAF",16); //175
    var num1 = parseInt("AF",16); //175
    var num2 = parseInt("AF"); //NaN
    var num3 = parseInt("10",2); //2(按二进制解析)
    var num4 = parseInt("10",8); //8(按八进制解析)
    var num5 = parseInt("10",10); //10(按十进制解析)
    var num6 = parseInt("10",16);//16(按十六进制解析)

-----

`parseFloat()`的几个典型例子：

    var num1 = parseFloat("123blue"); //13
    var num2 = parseFloat("0xA"); //0
    var num3 = parseFloat("22.5"); //22.5
    var num4 = parseFloat("22.34.5"); //22.34
    var num5 = parseFloat("0908.5"); //908.5
    var num6 = parseFloat("3.125e7"); //31250000

6.String类型

String类型用于表示由0或多个16位的Unicode字符组成的字符序列，即字符串。字符串可以由双引号（""）或单引号（''）表示，因此下面两种字符串的写法都是有效的：

    var firstName = "Wei";
    var lastName = "Li";
    var firstName = 'LiWei"; //语法错误

* 字符字面量

String数据类型包含一些特殊的字符字面量，也叫转义充列，用于表示非打印字符，或者具有其他用途的字符。

[table id=10 /]



    var text ="This is the letter sigma: \u03a3.";
    alert(text.length);//输出28

* 字符串的特点

ECMAScript中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就可能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量，例如：

    var lang = "Java";
    lang = lang + "Script";

* 转换为字符串

要把一个值转换为字符串，使用几乎每个值都有的toString()方法（后面讨论）。

> 要把某个值转换为字符串，可以使用加号操作符把它与一个字符串（""）加在一起，如：`var i = 13+"";//"13"`

下面来看一些例子：

    var age = 21;
    var ageAsString = age.toString(); // "21"
    var found = true;
    var foundAsString = found.toString(); //"true"

多数情况下，调用`toString()`方法不必传递参数。但是，在调用数值的`toString()`方法时，可以传递一个参数，`toString()`可以输出二进制、八进制、十六进制，及至其他任意有效进制格式表示的字符串值。

    var num = 10;
    alert(num.toString(1)); //"10"
    alert(num.toString(2)); //"1010"
    alert(num.toString(8)); //"12"
    alert(num.toString(10)); //"10"
    alert(num.toString(16)); //"a"

> 在不知道要转换的值是不是`null`或者`undefined`的情况下，还可以使用转型函数`String()`，这个函数能够将任何类型的值转换为字符串。`String()`函数遵循下列转换规则：

    var value1 = 10;
    var value2 = true;
    var value3 = null;
    var value4;

    alert(String(value1));//"10"
    alert(String(value2));//"true"
    alert(String(value3));//"null"
    alert(String(value4));//"undefined"

这里选择转换了4个值：数值、布尔值、null和undefined。


7.Object类型（重点理解）

JS中的对象其实是一组数据和功能的集合。对象可以通过执行`new`操作符后跟要创建的对象类型的名称来创建。而创建`Object`类型的实例并为其添加属性和（或）方法，就可以创建自定义对象，如下所示：

    var o = new Object();
    var o1 = new Object; //有效，但不推荐省略圆括号

Object的每个实例都具有下列属性的方法。

* Constructor:保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）就是`Object()`;
* hasOwnProperty(propertyName):用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如：o.hasOwnProperty("name")）。
* isPrototypeOf(object):用于检查传入的对象是否是另一个对象的原型。
* propertyIsEnumerable(propertyName):用于检查给定的属性是否能够使用for-in语句来枚举。与hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定。
* toLocaleString():返回对象的字符串表示，该字符串与执行环境的地区对应。
* toString():返回对象的字符串表示。
* valueOf():返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同。


###操作符
1.一元操作符

* 递增和递减操作符 `++` `--`

    var age =29;
    
    age++;

* 一元加和减操作符 `+` `-`

    var s1 = "01";
    var s2 = "1.1";
    var s3 = "z";
    var b = false;
    var f = 1.1;
    var o ={
        valueOf:functions(){
            return -1;
        }
    }

    s1 = +s1; // 1
    s2 = +s2; //1.1
    s3 = +s3; // NaN
    b = +b; //0
    f = +f; //1.1
    f = -f; //-1.1
    o = +o; //-1

2.位操作符

* 按位非（NOT） `~`
* 按位与（AND） `&`
* 按位或（OR） `|`
* 按位异或（XOR） `^`
* 左移  `<<`
* 有符号的右移  `>>`
* 无符号的右移  `>>>`

3.布尔操作符

* 逻辑非 `！`
* 逻辑与 `&&`
* 逻辑或 `||`

4.乘性、加性操作符

    * / + -

6.关系操作符

* 大于`>`小于`<`
* 大于等于`>=` 小于等于`<=`

7.相等操作符

* 相等`==`与不相等`!=`
* 全等`===`与不全等`!==`

8.条件操作符 `?`

    var max = (num1 > num2)?num1 : num2;

条件操作符应该算是JS中最灵活的一种操作符了，而且它遵循与Java中的条件操作符相同的语法形式。

9.赋值操作符

* 乘/赋值`*=`
* 除/赋值`/=`
* 模/赋值`%=`
* 加/赋值`+=`
* 减/赋值`-=`
* 左移/赋值`<<=`
* 有符号右移/赋值`>>=`
* 无符号右移/赋值`>>>=`
    

10.逗号操作符

    var num1=1,num2=2,num3=3;

###语句

1.if语句

    if (condition)
      {
     当条件为 true 时执行的代码
      }

    if (condition)
      {
      当条件为 true 时执行的代码
      }
    else
      {
      当条件不为 true 时执行的代码
      }

2.do-while语句

    do
      {
      需要执行的代码
      }
    while (条件);

3.while语句

    while (条件)
      {
      需要执行的代码
      }

4.for语句

    for (语句 1; 语句 2; 语句 3)
      {
      被执行的代码块
      }

C语言里面我们学习过，我们来举个例子：

    for(var i=0; i <10; i++)
    {
        console.log(i);//在console中输出0~9
    }

5.for-in语句

    for(property in expression){
        被执行的代码块
    }

例子：
    var group = ["hello","this","is","my","test","group"];

    for(var i in group){
        alert(group[i]);
    }

6.label语句
    
使用label语句可以在代码中添加标签，以便将来使用。

    label: statement

例子：

    start:for(var i=0; i< count; i++){
        alert(i);
    }

这个例子中定义的start标签可以在将来由break或者continue语句引用。加标签的语句一般都要与for语句等循环语句配合使用。

7.break和continue语句

break和continue语句用于在循环中精确地控制代码的执行。其中，break语句会立即退出循环，强制继续执行循环后面的语句。而continue语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。请看下面的例子：

    var num = 0;

    for(var i=1; i<10; i++){
        if(i%5==0){
            break;
        }
        num++;
    }
    
    alert(num);//4

如果把break替换成continue的话：

    var num = 0;

    for(var i=1; i<10; i++){
        if(i%5==0){
            continue;
        }
        num++;
    }
    
    alert(num);//8

break和continue语句都可以与label语句联合使用：

    var num = 0;
    
    outermost:
    for(var i=1; i<10; i++){
        for(var j=0; j<10; j++){
            if(i==5 && j==5){
                break outermost;
            }
            num++;
        }
    }
    
    alert(num);//45

===============

    var num = 0;
    
    outermost:
    for(var i=1; i<10; i++){
        for(var j=0; j<10; j++){
            if(i==5 && j==5){
                continue outermost;
            }
            num++;
        }
    }
    
    alert(num);//85    


8.with语句

with语句的作用是将代码的作用域设置到一个特定的对象中。

    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;

都包含location对象。如果使用with语句，可以把代码改写成如下所示：

    with(location){
        var qs = search.substring(1);
        var hostName = hostname;
        var url = href;
    }

> 由于大量使用with语句会导致性能下降，同时也会给高度代码造成困难，因此在开发大型应用程序时，不建议使用with语句。

9.switch语句

switch语句也是普遍使用的一种控制语句。

    switch(expression){
        case value1: statement1
            break;
        case value2: statement2
            break;
        case value3: statement2
            break;
        default:statement4
    }

例子：
    
    switch(i){
        case 25: //i等于25时
            alert("25");
            break;
        case 35://i等于35时
            alert("35");
            break;
        case 45://i等于45时
            alert("45");
            break;
        default:
            alert("Other");
    }

> 如果没有`break`，当条件成立时，会顺序执行后面的语句(包括default)，直到碰到`break`;

###函数

函数就是封装起来的功能，以后可以重复的进行调用。

函数可以带参数，也可以不带参数。

    function a(){
        //some code here 
    }
        
    function b(m,n){
        var x=m;
        var y=n;
        //some code here
    }

1.理解参数

ECMAScript函数的参数与大多数其他语言中函数的参数有所不同。ECMAScript函数不介意传递进来多少个参数，也不在乎传进来参数是什么数据类型。

> ECMAScript中的所有参数传递的都是值，不可能通过引用传递参数。

实际上，在函数体内可以通过`arguments`对象来访问这个参数数组，从而获取传递给函数的每一个参数。

其实，`arguments`对象只是与数组类似（它并不`Array`的实例），因为可以使用方括号语法访问它的第一个元素（即第一个元素是`arguments[0]`，第二个元素是arguments[1]，以此类推），使用length属性来确定传递进来多个参数。

    function a(){
        alert(arguments.length);
    }
    a("string",45);//2
    a();//0
    a(12);//1

由此可见，开发人员可以利用这一点让函数能够接收任意个参数并分别实现适当的功能。
    
    function doAdd(){
        if(arguments.length == 1){
            alert(arguments[0]+10);
        }else if(arguments.length == 2){
            alert(arguments[0] + arguments[1]);
        }
    }
    
    doAdd(10);//20
    doAdd(32,22);//54

虽然，这个特性不算上完美的重载，但也足够弥补JS的这一缺憾了。

2.没有重载

ECMAScript函数不能像传统意义上那样实现重载。而其他语言中（如Java中），可以为一个函数编写两个定义，只要两个定义的签名（接受的参数的类型和数量）不同即可。如前所述，ECMAScript函数没有签名，因为其参数是由包含零或多个值的数组来表示的。而没有函数签名，真正的重载是不可能做到的。

    function a(num){
        return num+10;
    }

    function a(num){
        return num+20;
    }

    var result = a(200); 
    alert(result);//220

在JS中定义了两个名字相同的函数，则该名字只属于后定义的函数。