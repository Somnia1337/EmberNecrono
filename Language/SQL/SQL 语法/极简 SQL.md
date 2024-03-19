# 1 SQL 基础

## 1. SELECT 语句

**SELECT 语句**：从表中选取数据，结果存储在 **结果集** 中。

```sql
-- 选取指定列
SELECT 列名称 FROM 表名称

-- 选取所有列
SELECT * FROM 表名称
```

"Persons" 表:

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

---

选取 "LastName" 与 "FirstName" 列：

```sql
SELECT LastName，FirstName FROM Persons
```

|LastName|FirstName|
|---|---|
|Adams|John|
|Bush|George|
|Carter|Thomas|

---

选取所有列：

```sql
SELECT * FROM Persons
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

## 2. DISTINCT 关键词

**DISTINCT 关键词**：选取所有不同的值。

```sql
SELECT DISTINCT 列名称 FROM 表名称
```

"Orders" 表：

|Company|OrderNumber|
|---|---|
|IBM|3532|
|W3School|2356|
|Apple|4698|
|W3School|6953|

---

选取所有不同的值：

```sql
SELECT DISTINCT Company FROM Orders
```

|Company| |
|---|---|
|IBM||
|W3School||
|Apple||

## 3. WHERE 子句

**WHERE 子句**：按指定条件筛选，作为 SELECT 的子句。

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

可用的运算符：

|运算符|描述|
|---|---|
|=|等于|
|<>|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|在某个范围内|
|LIKE|搜索某种模式|

"Persons" 表：

|LastName|FirstName|Address|City|Year|
|---|---|---|---|---|
|Adams|John|Oxford Street|London|1970|
|Bush|George|Fifth Avenue|New York|1975|
|Carter|Thomas|Changan Street|Beijing|1980|
|Gates|Bill|Xuanwumen 10|Beijing|1985|

---

选取城市为 "Beijing" 的人：

```sql
SELECT * FROM Persons WHERE City='Beijing'
```

|LastName|FirstName|Address|City|Year|
|---|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|1980|
|Gates|Bill|Xuanwumen 10|Beijing|1985|

## 4. 引号

使用单/双引号包括文本值：

```sql
--正确
SELECT * FROM Persons WHERE FirstName='Bush'

--错误
SELECT * FROM Persons WHERE FirstName=Bush
```

不能使用引号包括数值：

```sql
--正确
SELECT * FROM Persons WHERE Year>1965

--错误
SELECT * FROM Persons WHERE Year>'1965'
```

## 5. AND / OR 运算符

**运算符AND / OR**：连接多个条件。

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Adams|John|Oxford Street|London|
|Bush|George|Fifth Avenue|New York|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

---

选取所有姓为 "Carter" 并且名为 "Thomas" 的人：

```sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|

---

选取所有姓为 "Carter" 或者名为 "Thomas" 的人：

```sql
SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

---

选取所有姓为 "Carter"、并且名为 "Thomas" 或 "William" 的人：

```sql
SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William')
AND LastName='Carter'
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

## 6. ORDER BY 子句

**ORDER BY 子句**：根据指定的列对结果集排序，默认按升序排序，作为 SELECT 的子句。

**DESC 关键词**：指定倒序排序。

"Orders" 表：

|Company|OrderNumber|
|---|---|
|IBM|3532|
|W3School|2356|
|Apple|4698|
|W3School|6953|

---

以字母顺序排序公司名称：

```sql
SELECT Company，OrderNumber FROM Orders ORDER BY Company
```

|Company|OrderNumber|
|---|---|
|Apple|4698|
|IBM|3532|
|W3School|6953|
|W3School|2356|

---

以字母顺序排序公司名称，并以数字顺序排序订单号：

```sql
SELECT Company，OrderNumber FROM Orders ORDER BY Company，OrderNumber
```

|Company|OrderNumber|
|---|---|
|Apple|4698|
|IBM|3532|
|W3School|2356|
|W3School|6953|

---

以字母顺序倒序排序公司名称：

```sql
SELECT Company，OrderNumber FROM Orders ORDER BY Company DESC
```

|Company|OrderNumber|
|---|---|
|W3School|6953|
|W3School|2356|
|IBM|3532|
|Apple|4698|

---

以字母顺序倒序排序公司名称，并以数字顺序排序订单号：

```sql
SELECT Company，OrderNumber FROM Orders ORDER BY Company DESC，OrderNumber ASC
```

|Company|OrderNumber|
|---|---|
|W3School|2356|
|W3School|6953|
|IBM|3532|
|Apple|4698|

## 7. INSERT INTO 语句

**INSERT INTO 语句**：插入新行。

```sql
INSERT INTO 表名称 VALUES (值1，值2,....)

--可以指定所要插入数据的列
INSERT INTO table_name (列1，列2,...) VALUES (值1，值2,....)
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|

---

插入新行：

```sql
INSERT INTO Persons VALUES ('Gates'，'Bill'，'Xuanwumen 10'，'Beijing')
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Gates|Bill|Xuanwumen 10|Beijing|

---

插入新行并在指定列插入新值：

```sql
INSERT INTO Persons (LastName，Address) VALUES ('Wilson'，'Champs-Elysees')
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Wilson||Champs-Elysees||

## 8. UPDATE 语句

**Update 语句**：修改数据。

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson||Champs-Elysees||

---

为姓氏为 "Wilson" 的人添加名：

```sql
UPDATE Persons SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Champs-Elysees||

---

为姓氏为 "Wilson" 的人修改地址并添加城市：

```sql
UPDATE Persons SET Address = 'Zhongshan 23'，City = 'Nanjing' WHERE LastName = 'Wilson'
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Zhongshan 23|Nanjing|

## 9. DELETE 语句

**DELETE 语句**：删除行。

```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Zhongshan 23|Nanjing|

---

删除 "Wilson" 所在行：

```sql
DELETE FROM Person WHERE LastName = 'Wilson'
```

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|

---

在不删除表（保留表的完整结构）的情况下删除所有行：

```sql
DELETE FROM table_name
--或者
DELETE * FROM table_name
```

# 2 SQL 高级

## 1. TOP 子句

**TOP 子句**：指定要返回的记录的数目。

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

---

选取前两条记录：

```sql
SELECT TOP 2 * FROM Persons
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

---

选取前 50% 的记录：

```sql
SELECT TOP 50 PERCENT * FROM Persons
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

## 2. LIKE 操作符

**LIKE 操作符**：匹配指定的模式，作为 WHERE 子句的一部分。

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

---

选取居住在以 "N" 开头的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE 'N%'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|

---

选取居住在以 "g" 结尾的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '%g'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

---

选取居住在包含 "lon" 的城市里的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '%lon%'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|

---

选取居住在不包含 "lon" 的城市里的人：

```sql
SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

---

选取名字的第一个字符之后是 "eorge" 的人：

```sql
SELECT * FROM Persons
WHERE FirstName LIKE '_eorge'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|2|Bush|George|Fifth Avenue|New York|

---

选取姓氏以 "C" 开头，然后是一个任意字符，然后是 "r"，然后是一个任意字符，然后是 "er" 的人：

```sql
SELECT * FROM Persons
WHERE LastName LIKE 'C_r_er'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

---

选取居住的城市以 "A" 或 "L" 或 "N" 开头的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '[ALN]%'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

---

选取居住的城市_不以_ "A" 或 "L" 或 "N" 开头的人：

```sql
SELECT * FROM Persons
WHERE City LIKE '[!ALN]%'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|

## 4. IN 操作符

**IN 操作符**：指定多个筛选条件值，作为 WHERE 子句的一部分。

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

---

选取姓氏为 "Adams" 或 "Carter" 的人：

```sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|3|Carter|Thomas|Changan Street|Beijing|

## 5. BETWEEN 操作符

**BETWEEN ... AND 操作符**：选取介于两个值（数值、文本或者日期）之间的数据。

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

---

选取姓氏介于 "Adams" 和 "Carter" 之间的人：

```sql
SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|

注意：不同的数据库对 BETWEEN ... AND 操作符的开闭性有差异。

---

选取姓氏不在 "Adams" 和 "Carter" 之间的人：

```sql
SELECT * FROM Persons
WHERE LastName
NOT BETWEEN 'Adams' AND 'Carter'
```

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|3|Carter|Thomas|Changan Street|Beijing|
|4|Gates|Bill|Xuanwumen 10|Beijing|

## 6. Alias 别名

**Alias 别名**：为表或列定义别名，以便使用。

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

---

有两个表 "Persons" 和 "Product_Orders"，分别为它们定义别名 "p" 和 "po"，然后选取 "John Adams" 的所有定单：

```sql
SELECT po.OrderID，p.LastName，p.FirstName
FROM Persons AS p Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
```

---

对 "Persons" 表中的 "LastName" 和 "FirstName" 列定义别名并选取。

```sql
SELECT LastName AS Family FirstName AS Name
FROM Persons
```

|Family|Name|
|---|---|
|Adams|John|
|Bush|George|
|Carter|Thomas|

## 7. JOIN 关键词

**JOIN 关键词**：从两个或更多的表中获取结果。

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

---

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P = Orders.Id_P
ORDER BY Persons.LastName
```

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

---

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

|LastName|FirstName|OrderNo|
|---|---|---|
|Adams|John|22456|
|Adams|John|24562|
|Carter|Thomas|77895|
|Carter|Thomas|44678|

INNER JOIN 关键词在表中存在至少一个匹配时返回行。如果 "Persons" 中的行在 "Orders" 中没有匹配，就不会列出这些行。

### 8.2 LEFT JOIN 关键词

**LEFT JOIN 关键词**：从左表中返回所有的行，即使在右表中没有匹配的行。

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

---

选取所有人的订单号：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
LEFT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

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

---

选取所有订单号及订购者：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
RIGHT JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

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

---

列出所有的人（即使没有订单）以及所有的定单（即使没有订购者）：

```sql
SELECT Persons.LastName，Persons.FirstName，Orders.OrderNo
FROM Persons
FULL JOIN Orders
ON Persons.Id_P=Orders.Id_P
ORDER BY Persons.LastName
```

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

---

选取所有在中国和美国的不同雇员：

```sql
SELECT E_Name FROM Employees_China
UNION
SELECT E_Name FROM Employees_USA
```

|E_Name| |
|---|---|
|Zhang，Hua||
|Wang，Wei||
|Carter，Thomas||
|Yang，Ming||
|Adams，John||
|Bush，George||
|Gates，Bill||

---

选取在中国和美国的所有雇员：

```sql
SELECT E_Name FROM Employees_China
UNION ALL
SELECT E_Name FROM Employees_USA
```

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

---

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

---

从 "Persons" 表中提取居住在 "Beijing" 的人，创建一个带有两个列的名为 "Persons_backup" 的新表：

```sql
SELECT LastName,Firstname
INTO Persons_backup
FROM Persons
WHERE City='Beijing'
```

---

创建一个名为 "Persons_Order_Backup" 的新表，其中包含 "Persons" 和 "Orders" 两个表中的信息：

```sql
SELECT Persons.LastName,Orders.OrderNo
INTO Persons_Order_Backup
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P
```

# 3 SQL 创建结构

## 1. CREATE DATABASE 语句

**CREATE DATABASE 语句**：创建数据库。

```sql
CREATE DATABASE _database_name_
```

---

创建一个名为 "my_db" 的数据库：

```sql
CREATE DATABASE my_db
```

## 2. CREATE TABLE 语句

**CREATE TABLE 语句**：创建表。

```sql
CREATE TABLE _table_name_
(
_column_1_ _datatype_,
_column_2_ _datatype_,
_column_3_ _datatype_,
...
)
```

**数据类型**(datatype)：指定列可容纳的数据类型。

SQL 的数据类型：

|数据类型|描述|
|---|---|
|integer(size)<br>int(size)<br>smallint(size)<br>tinyint(size)|整数，括号内规定数字的最大位数|
|decimal(size,d)<br>numeric(size,d)|小数<br>"size"规定数字的最大位数<br>"d"规定小数点右侧的最大位数|
|char(size)|固定长度的字符串<br>括号内规定字符串的长度|
|varchar(size)|可变长度的字符串<br>括号内规定字符串的最大长度|
|date(yyyymmdd)|日期|

---

创建名为 "Person" 的表，该表包含 "Id_P"、"LastName"、"FirstName"、"Address" 、 "City" 5 个列：

```sql
CREATE TABLE Persons
(
Id_P int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

## 3. SQL 约束

**约束**(constraint)：限制加入表的数据类型。

**CREATE TABLE 语句**：在表创建时规定约束。

**ALTER TABLE 语句**：在表创建后规定约束。

SQL 约束：

- NOT NULL 约束。
- UNIQUE 约束。
- PRIMARY KEY 约束。
- FOREIGN KEY 约束。
- CHECK 约束。
- DEFAULT 约束。

### 3.1 NOT NULL 约束

**NOT NULL 约束**：强制列不接受 NULL 值。

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

在 "Persons" 表创建时定义 NOT NULL 约束：

```sql
--为两个列定义 NOT NULL 约束
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

### 3.2 UNIQUE 约束

**UNIQUE 约束**：唯一标识数据库表中的每条记录。

在 "Persons" 表创建时定义 UNIQUE 约束：

```sql
--为一个列定义 UNIQUE 约束
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
UNIQUE (Id_P)
)

--命名 UNIQUE 约束并为多个列定义
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
)
```

在 "Persons" 表创建后定义 UNIQUE 约束：

```sql
--为一个列定义 UNIQUE 约束
ALTER TABLE Persons
ADD UNIQUE (Id_P)

--命名 UNIQUE 约束并为多个列定义
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (Id_P,LastName)
```

撤销 UNIQUE 约束：

```sql
ALTER TABLE Persons
DROP INDEX uc_PersonID
```

### 3.3 PRIMARY KEY 约束

**PRIMARY KEY 约束**：唯一标识数据库表中的每条记录。

UNIQUE 和 PRIMARY KEY 约束均提供唯一性保证，PRIMARY KEY 拥有自动定义的 UNIQUE 约束。

一个表可以有多个 UNIQUE 约束，但是只能有一个 PRIMARY KEY 约束。

在 "Persons" 表创建时定义 PRIMARY KEY 约束：

```sql
--为一个列定义 PRIMARY KEY 约束
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (Id_P)
)

--命名 PRIMARY KEY 约束并为多个列定义
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
)
```

在 "Persons" 表创建后定义 PRIMARY KEY 约束：

```sql
--为一个列定义 PRIMARY KEY 约束
ALTER TABLE Persons
ADD PRIMARY KEY (Id_P)

--命名 PRIMARY KEY 约束并为多个列定义
ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)

--撤销 PRIMARY KEY 约束
ALTER TABLE Persons
DROP PRIMARY KEY
```

### 3.4 FOREIGN KEY 约束

一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。

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

"Orders" 中的 "Id_P" 列指向 "Persons" 表中的 "Id_P" 列。

"Persons" 表中的 "Id_P" 列为其 PRIMARY KEY。

"Orders" 表中的 "Id_P" 列为其 FOREIGN KEY。

在 "Orders" 表创建时定义 FOREIGN KEY：

```sql
--为一个列定义 FOREIGN KEY 约束
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
)

--命名 FOREIGN KEY 约束并为多个列定义
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
)
```

在 "Orders" 表创建后定义 FOREIGN KEY：

```sql
--为一个列定义 FOREIGN KEY 约束
ALTER TABLE Orders
ADD FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)

--命名 FOREIGN KEY 约束并为多个列定义
ALTER TABLE Orders
ADD CONSTRAINT fk_PerOrders
FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)

--撤销 FOREIGN KEY 约束
ALTER TABLE Orders
DROP FOREIGN KEY fk_PerOrders
```

### 3.5 CHECK 约束

**CHECK 约束**：限制列中的值的范围。

如果对单个列定义 CHECK 约束，那么该列只允许特定的值。

如果对一个表定义 CHECK 约束，那么此约束会在特定的列中对值进行限制。

在 "Persons" 表创建时定义 CHECK 约束：

```sql
--为一个列定义 CHECK 约束
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CHECK (Id_P>0)
)

--命名 CHECK 约束并为多个列定义
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')
)
```

在 "Persons" 表创建后定义 CHECK 约束：

```sql
--为一个列定义 CHECK 约束
ALTER TABLE Persons
ADD CHECK (Id_P>0)

--命名 CHECK 约束并为多个列定义
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (Id_P>0 AND City='Sandnes')

--撤销 CHECK 约束
ALTER TABLE Persons
DROP CHECK chk_Person
```

### 3.6 DEFAULT 约束

**DEFAULT 约束**：向列中插入默认值。

如果没有规定其他的值，那么会将默认值添加到所有的新记录。

在 "Persons" 表创建时定义 DEFAULT 约束：

```sql
--为一个列定义 DEFAULT 约束
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes'
)
```

通过使用类似 GETDATE 等函数，DEFAULT 约束也可以用于插入系统值：

```sql
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
OrderDate date DEFAULT GETDATE()
)
```

在 "Persons" 表创建后定义 DEFAULT 约束：

```sql
--为一个列定义 DEFAULT 约束
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES'

--撤销 DEFAULT 约束
ALTER TABLE Persons
ALTER City DROP DEFAULT
```