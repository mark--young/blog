本教程是JavaScript教程，结合一些程序例子，让你从基础入门到掌握JavaScript(简称JS)。学习本教程，你需要有HTML与CSS基础知识，并在第一课之后，动手实践，才能更好的掌握JavaScript。


## 变量、作用域和内存问题

###基本类型和引用类型的值

1.动态的属性

定义基本类型值和引用类型值的方式是类似的：创建一个变量并为该变量赋值。但是，当这个值保存到变量中以后，对不同类型值可以执行的操作则大相径庭。

如下，添加属性和方法，也可以改变和删除其属性和方法：

    var person = new Object();
    person.name = "Liwei";  //为对你添加name属性
    alert(person.name); //Liwei

但是，不能给基本类型的值添加属性：

    var name = "LiWei"
    name.age = 27;
    alert(name.age); // undefined

2.复制

除了保存的方式不同之外，在从一个变量向另一个变量复制基本类型值和引用类型值时，也存在不同。

（1）如果从一个变量向另一个变量复制基本类型的值 ，会在变量对象上创建一个新值。

    var num1 = 10;
    var num2 = num1;
    alert(num2);//10
    num1 = num1 -2;
    alert(num2);//10

（2）当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份到为新变量分配的空间中。**不同的是，这个值的副本实际一个指针，而这个指针指向存储在堆中的一个对象。**

    var obj1 = new Object();
    var obj2 = obj1;
    obj1.name = "LiWei";
    alert(obj2.name); // "LiWei"

3.传递参数

基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同引用类型变量的复制一样。

    function addTen(num){
        num += 10;
        return num;
    }
    
    var count = 20;
    var result  = addTen(count);
    alert(count); //20
    alert(result); //30

另外一个例子：

    function setName(obj){
        obj.name = "Liwei";
    }

    var person = new Object();
    setName(person);
    alert(person.name);//Liwei

以上代码中创建一个对象，并将其保存了变量`person`中。然后，这个对象被传递到`setName()`函数中后被复制给了`obj`。`obj`和`person`引用的是同一个对象。

    function setName(obj){
        obj.name = "Liwei";
        obj = new Object();
        obj.name = "ZhangWei";
    }
    
    var person = new Object();
    setName(person);
    alert(person.name);//"Liwei"

这个例子与前一个例子的唯一区别，即使在函数内部修改了参数的值，但原始的引用仍然保持求变。实际上，当在函数内部重写`obj`时，这个变量就是一个局部对象。而这个局部对象会在函数执行完毕后立即被销毁。

4.检测类型

**虽然，typeof操作符是检测基本数据类型的方法，但是实际上，我们使用instanceof的更多。**

* typeof 操作符是确定一个变量是字符串、数值、布尔值，还是`undefined` 的最佳工具。如果变量的值是一个对象或`null`，则`typeof` 操作符会像下面例子中所示的那样返回"object"。

        var s = "Nicholas";
        var b = true;
        var i = 22;
        var u;
        var n = null;
        var o = new Object();
        alert(typeof s); //string
        alert(typeof i); //number
        alert(typeof b); //boolean
        alert(typeof u); //undefined
        alert(typeof n); //object
        alert(typeof o); //object

* `instanceof` 操作符是用来检测对象的类型

语法： `result = variable instanceof constructor`

    alert(person instanceof Object); // 变量person 是Object 吗？
    alert(colors instanceof Array); // 变量colors 是Array 吗？
    alert(pattern instanceof RegExp); // 变量pattern 是RegExp 吗？

根据规定，所有引用类型的值都是Object 的实例。因此，在检测一个引用类型值和Object 构造
函数时，`instanceof` 操作符始终会返回`true`。当然，如果使用`instanceof` 操作符检测基本类型的
值，则该操作符始终会返回`false`，因为基本类型不是对象。

> 使用typeof 操作符检测函数时，该操作符会返回"function"。在Safari 5 及
之前版本和Chrome 7 及之前版本中使用typeof 检测正则表达式时，由于规范的原
因，这个操作符也返回"function"。ECMA-262 规定任何在内部实现[[Call]]方法
的对象都应该在应用typeof 操作符时返回"function"。由于上述浏览器中的正则
表达式也实现了这个方法，因此对正则表达式应用typeof 会返回"function"。在
IE 和Firefox 中，对正则表达式应用typeof 会返回"object"。
    

### 执行环境及作用域

标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，
然后逐级地向后回溯，直至找到标识符为止（如果找不到标识符，通常会导致错误发生）。

我们看代码来讲：

    //如果一个页面有下面一段JS代码

    var color = "blue"; // 全局变量 color
    function changeColor(){
    if (color === "blue"){
        color = "red";
    } else {
        color = "blue";
    }
    }
    changeColor();
    alert("Color is now " + color);

在这个简单的例子中，函数`changeColor()`的作用域链包含两个对象：它自己的变量对象（其中
定义着`arguments` 对象）和全局环境的变量对象。可以在函数内部访问变量`color`，就是因为可以在
这个作用域链中找到它。

此外，在局部作用域中定义的变量可以在局部环境中与全局变量互换使用，如下面这个例子所示：

    var color = "blue";
    function changeColor(){
        var anotherColor = "red";
        function swapColors(){
            var tempColor = anotherColor;
            anotherColor = color;
            color = tempColor;

            // 这里可以访问color、anotherColor 和tempColor
        }

        // 这里可以访问color 和anotherColor，但不能访问tempColor
    swapColors();
    }
    // 这里只能访问color
    changeColor();

以上代码共涉及3 个执行环境：全局环境、`changeColor()`的局部环境和`swapColors()`的局部
环境。全局环境中有一个变量`color` 和一个函数`changeColor()`。`changeColor()`的局部环境中有
一个名为`anotherColor` 的变量和一个名为`swapColors()`的函数，但它也可以访问全局环境中的变
量`color`。`swapColors()`的局部环境中有一个变量`tempColor`，该变量只能在这个环境中访问到。
无论全局环境还是`changeColor()`的局部环境都无权访问`tempColor`。然而，在`swapColors()`内部
则可以访问其他两个环境中的所有变量，因为那两个环境是它的父执行环境

**JavaScript没有块级作用域**，这里是在一个`if` 语句中定义了变量`color`。如果是在C、C++或Java 中，`color` 会在if 语句执行完毕后被销毁。但在JavaScript 中，`if` 语句中的变量声明会将变量添加到当前的执行环境（在这里是全局环境）中：

    if (true) {
    var color = "blue";
    }
    alert(color); //"blue"

在使用`for` 语句时尤其要牢记这一差异，例如：

    for (var i=0; i < 10; i++){
    doSomething(i);
    }
    alert(i); //10

对于有块级作用域的语言来说，`for` 语句初始化变量的表达式所定义的变量，只会存在于循环的环
境之中。而对于JavaScript 来说，由`for` 语句创建的变量`i` 即使在`for` 循环执行结束后，也依旧会存在于循环外部的执行环境中。

**使用var 声明的变量会自动被添加到最接近的环境中。**在函数内部，最接近的环境就是函数的局部
环境；在`with `语句中，最接近的环境是函数环境。如果初始化变量时没有使用`var` 声明，该变量会自
动被添加到全局环境。如下所示：

    function add(num1, num2) {
        var sum = num1 + num2;
    return sum;
    }
    var result = add(10, 20); //30
    alert(sum); //由于sum 不是有效的变量，因此会导致错误

以上代码中的函数`add()`定义了一个名为`sum` 的局部变量，该变量包含加法操作的结果。虽然结
果值从函数中返回了，但变量`sum` 在函数外部是访问不到的。**如果省略这个例子中的var 关键字，那
么当add()执行完毕后，`sum `也将可以访问到**：

    function add(num1, num2) {
        sum = num1 + num2;
    return sum;
    }
    var result = add(10, 20); //30
    alert(sum); //30

这个例子中的变量`sum` 在被初始化赋值时没有使用`var` 关键字。于是，当调用完`add()`之后，添
加到全局环境中的变量`sum` 将继续存在；即使函数已经执行完毕，后面的代码依旧可以访问它。


### 垃圾收集

**JavaScript 具有自动垃圾收集机制**，也就是说，执行环境会负责管理代码执行过程中使用的内存。而在C 和C++之类的语言中，开发人员的一项基本任务就是手工跟踪内存的使用情况，这是造成许多问题的一个根源。

**JavaScript 中最常用的垃圾收集方式是标记清除（mark-and-sweep）。**到2008 年为止，IE、Firefox、Opera、Chrome 和Safari 的JavaScript 实现使用的都是标记清除式的
垃圾收集策略（或类似的策略），只不过垃圾收集的时间间隔互有不同。

> 在IE 中，调用window.CollectGarbage()方法会立即执行垃圾收集。在Opera 7 及更
高版本中，调用window.opera.collect()也会启动垃圾收集例程。

**确保占用最少的内存可以让页面获得更好的性能，而优化内存占用的最佳方式，就是为执行
中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为null 来释放其引用——这个
做法叫做解除引用（dereferencing）。**

    function createPerson(name){
        var localPerson = new Object();
        localPerson.name = name;
    return localPerson;
    }
    var globalPerson = createPerson("Nicholas");
    // 手工解除globalPerson 的引用
    globalPerson = null;

> 这种传递参数的模式最适合需要向函数传入大量可选参数的情形。一般来讲，命名参数虽然容易处理，但在有多个可选参数的情况下就会显示不够灵活。最好的做法是对那些必需值使用命名参数，而使用对象字面量来封装多个可选参数。



