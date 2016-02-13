本教程是JavaScript教程，结合一些程序例子，让你从基础入门到掌握JavaScript(简称JS)。学习本教程，你需要有HTML与CSS基础知识，并在第一课之后，动手实践，才能更好的掌握JavaScript。

## 引用类型

在ECMAScript 中，引用类型是一种数据结构，用于将数据和功能组织在一起。它也常被称为类，但这种称呼并不妥当。尽管ECMAScript从技术上讲是一门面向对象的语言，但它不具备传统的面向对象语言所支持的类和接口等基本结构。引用类型有时候也被称为对象定义，因为它们描述的是一类对象所具有的属性和方法。

> 虽然引用类型与类看起来相似，但它们并不是相同的概念。

如前所述，对象是某个特定引用类型的实例。新对象是使用new 操作符后跟一个构造函数来创建的。构造函数本身就是一个函数，只不过该函数是出于创建新对象的目的而定义的。请看下面这行代码：
    
    var person = new Object();

这行代码创建了`Object` 引用类型的一个新实例，然后把该实例保存在了变量`person` 中。使用的构造函数是`Object`，它只为新对象定义了默认的属性和方法。ECMAScript 提供了很多原生引用类型（例如`Object`），以便开发人员用以实现常见的计算任务。

### Object 类型

创建Object 实例的方式有两种。第一种是使用new 操作符后跟Object 构造函数，如下所示：

    var person = new Object();
    person.name = "Nicholas";
    person.age = 29;

另一种方式是使用对象字面量表示法：

    var person = {
    name : "Nicholas",
    age : 29
    };

> 在age 属性的值29 的后面不能添加逗号，因为age 是这个对象的最后一个属性。在最后一个属性后面添加逗号，会在IE7 及更早版本和Opera 中导致错误。

在使用对象字面量语法时，属性名也可以使用字符串，如下面这个例子所示。

    var person = {
        "name" : "Nicholas",
        "age" : 29,
        5 : true //person["5"]来访问
    };

这个例子会创建一个对象，包含三个属性：name、age 和5。但这里的数值属性名会自动转换为字符串。另外，使用对象字面量语法时，如果留空其花括号，则可以定义只包含默认属性和方法的对象，如下所示：

    var person = {}; //与new Object()相同
    person.name = "Nicholas";
    person.age = 29;

> 在通过对象字面量定义对象时，实际上不会调用Object 构造函数（Firefox 2 及更早版本会调用Object 构造函数；但Firefox 3 之后就不会了）。

**作用：对象字面量也是向函数传递大量可选参数的首选方式**，例如：

    function displayInfo(args) {
        var output = "";
        if (typeof args.name == "string"){
            output += "Name: " + args.name + "\n";
        }
        if (typeof args.age == "number") {
            output += "Age: " + args.age + "\n";
        }   

        alert(output);
    }

    displayInfo({
        name: "Nicholas",
        age: 29
    });

    displayInfo({
        name: "Greg"
    });

在这个例子中，函数`displayInfo()`接受一个名为`args` 的参数。这个参数可能带有一个名为`name`或`age` 的属性，也可能这两个属性都有或者都没有。在这个函数内部，我们通过`typeof` 操作符来检测每个属性是否存在，然后再基于相应的属性来构建一条要显示的消息。然后，我们调用了两次这个函数，每次都使用一个对象字面量来指定不同的数据。这两次调用传递的参数虽然不同，但函数都能正常执行。

> 这种传递参数的模式最适合需要向函数传入大量可选参数的情形。一般来讲，命名参数虽然容易处理，但在有多个可选参数的情况下就会显示不够灵活。最好的做法是对那些必需值使用命名参数，而使用对象字面量来封装多个可选参数。

**object属性使用方法**

一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言中通用的语法。不过，在JavaScript 也可以使用方括号表示法来访问对象的属性。在使用方括号语法时，应该将要访问的属性以字符串的形式放在方括号中，如下面的例子所示。
    
    alert(person["name"]); //"Nicholas"
    alert(person.name); //"Nicholas"

从功能上看，这两种访问对象属性的方法没有任何区别。但方括号语法的主要优点是可以通过变量来访问属性，例如：

    var propertyName = "name";
    alert(person[propertyName]); //"Nicholas"
如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以使用方括号表示法。例如：
    
    person["first name"] = "Nicholas";

由于"first name"中包含一个空格，所以不能使用点表示法来访问它。然而，属性名中是可以包含非字母非数字的，这时候就可以使用方括号表示法来访问它们。通常，除非必须使用变量来访问属性，否则我们建议使用点表示法。

