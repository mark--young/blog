##SQL进阶

###SELECT TOP子句

语法：

	SELECT TOP number|percent column_name(s)
	FROM table_name;

下面的 SQL 语句从 "Customers" 表中选取头两条记录，实例：

	SELECT TOP 2 * FROM Customers;

###LIKE操作符

语法：

	SELECT column_name(s)
	FROM table_name
	WHERE column_name LIKE pattern;

下面的 SQL 语句选取 City 以字母 "L" 开始的所有客户，例子：
	
	SELECT * FROM Customers
	WHERE City LIKE 'L%';

下面的 SQL 语句选取 Country 包含模式 "me" 的所有客户：

	SELECT * FROM Customers
	WHERE Country LIKE '%me%';

通过使用 `NOT` 关键字，您可以选取不匹配模式的记录。
下面的 SQL 语句选取 Country 不包含模式 "me" 的所有客户：

	SELECT * FROM Customers
	WHERE Country NOT LIKE '%me%';

###SQL通配符

[table id=20 /]

主要讲[charlist]通配符，下面的 SQL 语句选取 City 以 "b"、"s" 或 "p" 开始的所有客户：
	
	SELECT * FROM Customers
	WHERE City LIKE '[bsp]%';

下面的 SQL 语句选取 City 以 "a"、"b" 或 "c" 开始的所有客户：

	SELECT * FROM Customers
	WHERE City LIKE '[a-c]%';

下面的 SQL 语句选取 City 不以 "b"、"s" 或 "p" 开始的所有客户：

	SELECT * FROM Customers
	WHERE City LIKE '[!bsp]%';

可以在线测试一下：[http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_wildcard_charlist&ss=-1](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_wildcard_charlist&ss=-1)

###IN操作符

IN操作符主要是在WHERE子句中去设定多个值
	
	SELECT column_name(s)
	FROM table_name
	WHERE column_name IN (value1,value2,...);

下面的 SQL 语句选取 City 为 "Paris" 或 "London" 的所有客户，例如：
	
	SELECT * FROM Customers
	WHERE City IN ('Paris','London');

与下面的句子等价：

	SELECT * FROM Customers
	WHERE City = 'Paris' OR City='London';

###BETWEEN操作符

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。

语法：

	SELECT column_name(s)
	FROM table_name
	WHERE column_name BETWEEN value1 AND value2;

下面的 SQL 语句选取CustomerID介于 1 和 3 之间的所有用户：

	SELECT * FROM customers
	WHERE CustomerID BETWEEN 1 AND 3;

与下面的语句等价：

	SELECT * FROM customers
	WHERE CustomerID >=1 AND CustomerID<=3;

**1.NOT BETWEEN 操作符**

同样对于BETWEEN，我们可以使用NOT非操作符：

	SELECT * FROM customers
	WHERE CustomerID NOT BETWEEN 1 AND 3;

**2.带有 IN 的 BETWEEN 操作符**

下面的 SQL 语句选取CustomerID为1到3之间，并且CITY为Berlin的客户：

	SELECT * FROM customers
	WHERE CustomerID BETWEEN 1 AND 3
	AND CITY IN ('Berlin');

**3.带有文本值的 BETWEEN 操作符**
下面的SQL语句选取了customers中CustomerName介于'A'和'B'之间字母开始的所有用户：

	SELECT * FROM customers
	WHERE CustomerName BETWEEN 'A' AND 'B';

> 请注意，在不同的数据库中，BETWEEN 操作符会产生不同的结果！

> 在某些数据库中，BETWEEN 选取介于两个值之间但不包括两个测试值的字段。

> 在某些数据库中，BETWEEN 选取介于两个值之间且包括两个测试值的字段。

> 在某些数据库中，BETWEEN 选取介于两个值之间且包括第一个测试值但不包括最后一个测试值的字段。

> 因此，请检查您的数据库是如何处理 BETWEEN 操作符！