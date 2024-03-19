## 1. TOP 子句

**TOP 子句**：指定要返回的记录的数目。

### 1.1 TOP 语法

```sql
SELECT _column_name(s)_
FROM _table_name_
LIMIT _number_
```

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|
|4|Obama|Barack|Pennsylvania Avenue|Washington|

### 1.2 TOP 实例 1：指定数值

选取前两条记录：

```sql
SELECT TOP 2 * FROM Persons
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

### 1.3 TOP 实例 2：指定百分比

选取前 50% 的记录：

```sql
SELECT TOP 50 PERCENT * FROM Persons
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

## 2. LIKE 操作符

**LIKE 操作符**：匹配指定的模式，作为 WHERE 子句的一部分。

### 2.1 LIKE 语法

```sql
SELECT _column_name(s)_
FROM _table_name_
WHERE _column_name_ LIKE _pattern_
```

其中`_pattern_`为通配符表达式。

SQL 中的通配符：

|通配符|描述|
|---|---|
|%|代表零个或多个字符|
|\_|仅替代一个字符|
|[charlist]|字符集中的任何单一字符|

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

### 2.2 % 实例

#### 实例 1：匹配前缀

选取居住在以 "N" 开头的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE 'N%'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|

#### 实例 2：匹配后缀

选取居住在以 "g" 结尾的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '%g'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

#### 实例 3：匹配子串

选取居住在包含 "lon" 的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '%lon%'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|

#### 实例 4：反选

选取居住在不包含 "lon" 的城市里的人：

```sql
SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

### 2.3 _ 实例

#### 实例 1：单字符留空

选取名字的第一个字符之后是 "eorge" 的人：

```sql
SELECT * FROM Persons
WHERE FirstName LIKE '_eorge'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|

#### 实例 2：多字符留空

选取姓氏以 "C" 开头，然后是一个任意字符，然后是 "r"，然后是一个任意字符，然后是 "er" 的人：

```sql
SELECT * FROM Persons
WHERE LastName LIKE 'C_r_er'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

### 2.3 \[charlist] 实例

#### 实例 1：结合 %

选取居住的城市以 "A" 或 "L" 或 "N" 开头的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '[ALN]%'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

#### 实例 2：反选

选取居住的城市_不以_ "A" 或 "L" 或 "N" 开头的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '[!ALN]%'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

## 4. IN 操作符

**IN 操作符**：指定多个筛选条件值，作为 WHERE 子句的一部分。

### 4.1 IN 语法

```sql
SELECT _column_name(s)_
FROM _table_name_
WHERE _column_name_ IN (_value1_,_value2_,...)
```

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

### 4.2 IN 实例

选取姓氏为 "Adams" 或 "Carter" 的人：

```sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|3|Carter|Thomas|Changan Street|Beijing|

## 5. BETWEEN 操作符

**BETWEEN ... AND 操作符**：选取介于两个值（数值、文本或者日期）之间的数据。

### 5.1 BETWEEN 语法

```sql
SELECT _column_name(s)_
FROM _table_name_
WHERE _column_name_
BETWEEN _value1_ AND _value2_
```

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|
|4|Gates|Bill|Xuanwumen 10|Beijing|

### 5.2 BETWEEN 实例

选取姓氏介于 "Adams" 和 "Carter" 之间的人：

```sql
SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

注意：不同的数据库对 BETWEEN ... AND 操作符的开闭性有差异。

选取姓氏不在 "Adams" 和 "Carter" 之间的人：

```sql
SELECT * FROM Persons
WHERE LastName
NOT BETWEEN 'Adams' AND 'Carter'
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|
|4|Gates|Bill|Xuanwumen 10|Beijing|

## 6. Alias 别名

**Alias 别名**：为表或列定义别名，以便使用。

### 6.1 Alias 语法

```sql
--表语法
SELECT _column_name(s)_
FROM _table_name_
AS _alias_name_

--列语法
SELECT _column_name_ AS _alias_name_
FROM _table_name_
```

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

### 6.2 Alias 实例 1：表名别名

有两个表 "Persons" 和 "Product_Orders"，分别为它们定义别名 "p" 和 "po"，然后选取 "John Adams" 的所有定单：

```sql
SELECT po.OrderID，p.LastName，p.FirstName
FROM Persons AS p Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
```

### 6.3 Alias 实例 2：列名别名

对 "Persons" 表中的 "LastName" 和 "FirstName" 列定义别名并选取。

```sql
SELECT LastName AS Family FirstName AS Name
FROM Persons
```

结果：

|Family|Name|
|---|---|
|Adams|John|
|Bush|George|
|Carter|Thomas|

## 7. JOIN 关键词

**JOIN 关键词**：从两个或更多的表中获取结果。

数据库中的表可通过键将彼此联系起来。**主键**(Primary Key)是一个列，在这个列中的每一行的值都是唯一的。

"Persons" 表：

|Id_P|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

"Orders" 表：

|Id_O|OrderNo|Id_P|
|---|---|---|
|1|77895|3|
|2|44678|3|
|3|22456|1|
|4|24562|1|
|5|34764|65|

### 7.1 JOIN 实例

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
ORDER BY Persons.LastName
```

结果：

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|

## 8. 更多 JOIN

除了JOIN(INNER JOIN)，还有：

- JOIN：如果表中有至少一个匹配，则返回行。
- LEFT JOIN：即使右表中没有匹配，也从左表返回所有的行。
- RIGHT JOIN：即使左表中没有匹配，也从右表返回所有的行。
- FULL JOIN：只要其中一个表中存在匹配，就返回行。

### 8.1 INNER JOIN 关键词

**INNER JOIN 关键词**：在表中存在至少一个匹配时返回行。

\[7. JOIN] 即为 INNER JOIN。

#### 8.1.1 INNER JOIN 语法

```sql
SELECT _column_name(s)_
FROM _table_name1_
INNER JOIN _table_name2_
ON _table_name1.column_name_=_table_name2.column_name_
```

"Persons" 表：

|Id_P|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

"Orders" 表：

|Id_O|OrderNo|Id_P|
|---|---|---|
|1|77895|3|
|2|44678|3|
|3|22456|1|
|4|24562|1|
|5|34764|65|

#### 8.1.2 INNER JOIN 实例

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

结果：

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|

INNER JOIN 关键词在表中存在至少一个匹配时返回行。如果 "Persons" 中的行在 "Orders" 中没有匹配，就不会列出这些行。

### 8.2 LEFT JOIN 关键词

**LEFT JOIN 关键词**：从左表中返回所有的行，即使在右表中没有匹配的行。

#### 8.2.1 LEFT JOIN 语法

```sql
SELECT _column_name(s)_
FROM _table_name1_
LEFT JOIN _table_name2_
ON _table_name1.column_name_=_table_name2.column_name_
```

注意：在某些数据库中，LEFT JOIN 称为 LEFT OUTER JOIN。

"Persons" 表：

|Id_P|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

"Orders" 表：

|Id_O|OrderNo|Id_P|
|---|---|---|
|1|77895|3|
|2|44678|3|
|3|22456|1|
|4|24562|1|
|5|34764|65|

#### 8.2.2 LEFT JOIN 实例

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
LEFT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

结果：

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|
|Bush|George||

LEFT JOIN 从 "Persons" 表中返回所有的行，即使在 "Orders" 表中没有匹配的行。

### 8.3 RIGHT JOIN 关键词

RIGHT JOIN 关键词：从右表中返回所有的行，即使在左表中没有匹配的行。

#### 8.3.1 RIGHT JOIN 语法

```sql
SELECT _column_name(s)_
FROM _table_name1_
RIGHT JOIN _table_name2_
ON _table_name1.column_name_=_table_name2.column_name_
```

注意：在某些数据库中，RIGHT JOIN 称为 RIGHT OUTER JOIN。

"Persons" 表：

|Id_P|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

"Orders" 表：

|Id_O|OrderNo|Id_P|
|---|---|---|
|1|77895|3|
|2|44678|3|
|3|22456|1|
|4|24562|1|
|5|34764|65|

#### 8.3.2 RIGHT JOIN 实例

选取所有订单号及订购者：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
RIGHT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

结果：

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|
|||34764|

RIGHT JOIN 关键词会从右表 (Orders) 那里返回所有的行，即使在左表 (Persons) 中没有匹配的行。

### 8.4 FULL JOIN 关键词

**FULL JOIN 关键词**：返回在任何表中存在匹配的行。

#### 8.4.1 FULL JOIN 语法

```sql
SELECT _column_name(s)_
FROM _table_name1_
FULL JOIN _table_name2_
ON _table_name1.column_name_=_table_name2.column_name_
```

注意：在某些数据库中，FULL JOIN 称为 FULL OUTER JOIN。

"Persons" 表：

|Id_P|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

"Orders" 表：

|Id_O|OrderNo|Id_P|
|---|---|---|
|1|77895|3|
|2|44678|3|
|3|22456|1|
|4|24562|1|
|5|34764|65|

#### 8.4.2 FULL JOIN 实例

列出所有的人（即使没有订单）以及所有的定单（即使没有订购者）：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
FULL JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

结果：

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|
|Bush|George||
|||34764|

## 9. UNION 操作符

**UNION 操作符**：合并多个 SELECT 语句的结果。

UNION 内部的 SELECT 语句必须拥有相同数量的列，列也必须拥有相似的数据类型，并且每条 SELECT 语句中的列的顺序必须相同。

### 9.1 UNION 语法

```sql
SELECT _column_name(s)_ FROM _table_name1_
UNION
SELECT _column_name(s)_ FROM _table_name2_
```

UNION 操作符将摒弃重复值。

```sql
SELECT _column_name(s)_ FROM _table_name1_
UNION ALL
SELECT _column_name(s)_ FROM _table_name2_
```

UNION ALL 允许重复值。

UNION 结果中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

"Employees_China" 表：

|E_ID|E_Name|
|---|---|
|01|Zhang，Hua|
|02|Wang，Wei|
|03|Carter，Thomas|
|04|Yang，Ming|

"Employees_USA" 表：

|E_ID|E_Name|
|---|---|
|01|Adams，John|
|02|Bush，George|
|03|Carter，Thomas|
|04|Gates，Bill|

### 9.2 UNION 实例

选取所有在中国和美国的不同雇员：

```sql
SELECT E_Name FROM Employees_China
UNION
SELECT E_Name FROM Employees_USA
```

结果：

|E_Name| |
|---|---|
|Zhang，Hua||
|Wang，Wei||
|Carter，Thomas||
|Yang，Ming||
|Adams，John||
|Bush，George||
|Gates，Bill||

### 9.3 UNION ALL 实例

选取在中国和美国的所有雇员：

```sql
SELECT E_Name FROM Employees_China
UNION ALL
SELECT E_Name FROM Employees_USA
```

结果：

|E_Name| |
|---|---|
|Zhang，Hua||
|Wang，Wei||
|Carter，Thomas||
|Yang，Ming||
|Adams，John||
|Bush，George||
|Carter，Thomas||
|Gates，Bill||

## 10. SELECT INTO 语句

**SQL SELECT INTO 语句**：创建表的备份。

SELECT INTO 语句从一个表中选取数据，将其插入另一个表中。

### 10.1 SELECT INTO 语法

```sql
--将所有的列插入新表
SELECT *
INTO _new_table_name_ [IN _externaldatabase_] 
FROM _old_tablename_

--把指定的列插入新表
SELECT column_name(s)
INTO _new_table_name_ [IN _externaldatabase_]
FROM _old_tablename_
```

### 10.2 SELECT INTO 实例 1：备份

备份 "Persons" 表：

```sql
SELECT *
INTO Persons_backup
FROM Persons
```

IN 子句可用于向另一个数据库中拷贝表：

```sql
SELECT *
INTO Persons IN 'Backup.mdb'
FROM Persons
```

如需拷贝某些域，可以在 SELECT 语句后列出：

```sql
SELECT LastName,FirstName
INTO Persons_backup
FROM Persons
```

### 10.3 SELECT INTO 实例 2：结合 WHERE

从 "Persons" 表中提取居住在 "Beijing" 的人，创建一个带有两个列的名为 "Persons_backup" 的新表：

```sql
SELECT LastName,Firstname
INTO Persons_backup
FROM Persons
WHERE City='Beijing'
```

### 10.4 SELECT INTO 实例 3：被连接的表

创建一个名为 "Persons_Order_Backup" 的新表，其中包含 "Persons" 和 "Orders" 两个表中的信息：

```sql
SELECT Persons.LastName,Orders.OrderNo
INTO Persons_Order_Backup
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P
```