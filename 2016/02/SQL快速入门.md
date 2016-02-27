## SQL快速入门

SQL 是用于访问和处理数据库的标准的计算机语言。在本教程中，您将学到如何使用 SQL 访问和处理数据系统中的数据，这类数据库包括：MySQL、SQL Server、Access、Oracle、Sybase、DB2 等等。本篇博客告诉大家如果快速入门，学习SQL的增(Insert)、删(Delete)、改(Update)、查(Select)。

###SQL配置学习环境

我的策略是：WAMP中的MySQL+ Navicat

如何配置：

可以先参考：[使用WampServer搭建本地PHP环境，绑定域名，配置伪静态](http://wayearn.com/2016/01/wamp/)先自己搭建一个WAMP环境，

然后，安装Navicat软件，网上一大堆。

打开 -> 选择"连接" -> "MySQL" -> 

	连接名：MyStudy
	主机名或IP地址：localhost
	端口：3306
	用户名：root
	密码：（如果设置了，请输入）

-> 连接测试 "OK" -> "确定"

-> 选择表 -> 点击"查询" -> "新建查询" ->就可以开始学习了。

###简介

SQL指结构化查询语言，全称是（Structured Query Language）是用于访问和处理数据库的标准的计算机语言。SQL 让您可以访问和处理数据库。SQL 是一种 ANSI（American National Standards Institute 美国国家标准化组织）标准的计算机语言。

###语法
数据表：

[table id=18 /]

上面的表包含五条记录（每一条对应一个客户）和七个列（CustomerID、CustomerName、ContactName、Address、City、PostalCode 和 Country）。

下面的 SQL 语句从 "Customers" 表中选取所有记录：

	SELECT * FROM Customers;

> * SQL 对大小写不敏感：`SELECT` 与 `select` 是相同的。
> * 某些数据库系统要求在每条 SQL 语句的末端使用分号。
> * 分号是在数据库系统中分隔每条 SQL 语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的 SQL 语句。

重要的SQL命令一览：

* SELECT - 从数据库中提取数据
* UPDATE - 更新数据库中的数据
* DELETE - 从数据库中删除数据
* INSERT INTO - 向数据库中插入新数据
* CREATE DATABASE - 创建新数据库
* ALTER DATABASE - 修改数据库
* CREATE TABLE - 创建新表
* ALTER TABLE - 变更（改变）数据库表
* DROP TABLE - 删除表
* CREATE INDEX - 创建索引（搜索键）
* DROP INDEX - 删除索引

###SELECT语句

语法：`SELECT column_name,column_name FROM table_name;`

下面的 SQL 语句从 "Customers" 表中选取 "CustomerName" 和 "City" 列：
	
	SELECT CustomerName,City FROM Customers;

如果需要选中所有的列，使用`*`：

	SELECT * FROM Customers;

###SELECT DISTINCT语句

语法：`SELECT DISTINCT column_name,column_name FROM table_name;`

例如：
	SELECT DISTINCT City FROM Customers;

需要说明的是：`DISTINCT`与`GROUP BY`的区别：

`GROUP BY`可以使用一些其它的聚合函数：`AVG`, `MAX`, `MIN`, `SUM` 和 `COUNT`。而`DISTINCT`只是简单的移除重复项。

###WHERE子句

子句，是作为SELECT语句来说的。

语法：`SELECT column_name,column_name FROM table_name WHERE column_name operator value;`

例子:

	SELECT * FROM Customers WHERE Country='Mexico';

取得Customers表中，Country为Mexico的所有列。
> SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。
如果是数值字段，请不要使用引号。

例子：

	SELECT * FROM Customers WHERE CustomerID=1;


下面的运算符可以在 WHERE 子句中使用：

[table id=19 /]

###AND & OR运算符
如果第一个条件和第二个条件都成立，则 `AND` 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 `OR` 运算符显示一条记录。

`AND`例子：

	SELECT * FROM Customers
	WHERE Country='Germany'
	AND City='Berlin';

`OR`例子：

	SELECT * FROM Customers
	WHERE City='Berlin'
	OR City='München';

结合 `AND` & `OR`：

	SELECT * FROM Customers
	WHERE Country='Germany'
	AND (City='Berlin' OR City='München');



###INSERT INTO语句

语法1：
    INSERT INTO table_name
	VALUES (value1,value2,value3,...);

语法2：

	INSERT INTO table_name (column1,column2,column3,...)
	VALUES (value1,value2,value3,...);

比如插入新的一行：

	INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
	VALUES ('Cardinal','Tom B. Erichsen','Skagen 21','Stavanger','4006','Norway');

在指定的列插入数据：

	INSERT INTO Customers (CustomerName, City, Country)
	VALUES ('Cardinal', 'Stavanger', 'Norway');

###UPDATE语句

`UPDATE` 语句用于更新表中已存在的记录。

语法：`UPDATE table_name SET column1=value1,column2=value2,...WHERE some_column=some_value;`

实例：

	UPDATE Customers
	SET ContactName='Alfred Schmidt', City='Hamburg'
	WHERE CustomerName='Alfreds Futterkiste';

> 一定要注意，不能省略`WHERE`子句，否则数据库所有列都会被重写。

###DELETE语句

语法：`DELETE FROM table_name WHERE some_column=some_value;`

实例：

	DELETE FROM Customers
	WHERE CustomerName='Alfreds Futterkiste' AND ContactName='Maria Anders';

删除所有数据：

	DELETE FROM table_name;
	
	or
	
	DELETE * FROM table_name;