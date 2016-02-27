##PHP类与对象学习

PHP 5 is very very flexible in accessing member variables and member functions. These access methods maybe look unusual and unnecessary at first glance; but they are very useful sometimes; specially when you work with SimpleXML classes and objects. I have posted a similar comment in SimpleXML function reference section, but this one is more comprehensive. 

PHP 5是非常灵活的去使用成员变量与成员函数的。这些方法第一眼看去可能看起来很不常用和很不必要；但是，它们非常有用有的时候；特别是，当你在使用SimpleXML类和对象的时候。这里有一些例子：

Demo1:

	<?php
	
	$obj = (object) array('foo' => 'bar', 'property' => 'value');
	
	echo $obj->foo; // prints 'bar'
	echo $obj->property; // prints 'value'
	
	?>

Demo2:

	<?php 
	class Foo { 
	    public $aMemberVar = 'aMemberVar Member Variable'; 
	    public $aFuncName = 'aMemberFunc'; 
	    
	    
	    function aMemberFunc() { 
	        print 'Inside `aMemberFunc()`'; 
	    } 
	} 
	
	$foo = new Foo; 
	?> 

	//第一种访问成员变量的方式：使用其它变量作为访问名
	<?php 
	$element = 'aMemberVar'; 
	print $foo->$element; // prints "aMemberVar Member Variable" 
	?> 
	
	//或者使用函数
	<?php 
	function getVarName() 
	{ return 'aMemberVar'; } 
	
	print $foo->{getVarName()}; // prints "aMemberVar Member Variable" 
	?> 


> 特别注意：这个地方的`getVarName();`必需使用一对括号 `{` `}`，否则的话PHP会认为这个函数是对象`foo`的成员函数。

	// 使用常量
	<?php 
	define(MY_CONSTANT, 'aMemberVar'); 
	print $foo->{MY_CONSTANT}; // Prints "aMemberVar Member Variable" 
	print $foo->{'aMemberVar'}; // Prints "aMemberVar Member Variable" 
	?> 

	//使用其它对象的成员也可以
	<?php 
	print $foo->{$otherObj->var}; 
	print $foo->{$otherObj->func()}; 
	?> 

	//下面的方式去获取成员函数
	<?php 
	print $foo->{'aMemberFunc'}(); // Prints "Inside `aMemberFunc()`" 
	print $foo->{$foo->aFuncName}(); // Prints "Inside `aMemberFunc()`" 
	?>