## 1. SQL 简介

### 1.1 什么是 SQL？

- SQL 指结构化查询语言
- SQL 使我们有能力访问数据库
- SQL 是一种 ANSI 的标准计算机语言

### 1.2 SQL 能做什么？

- SQL 面向数据库执行查询
- SQL 可从数据库取回数据
- SQL 可在数据库中插入新的记录
- SQL 可更新数据库中的数据
- SQL 可从数据库删除记录
- SQL 可创建新数据库
- SQL 可在数据库中创建新表
- SQL 可在数据库中创建存储过程
- SQL 可在数据库中创建视图
- SQL 可以设置表、存储过程和视图的权限
### 1.3 RDBMS

RDBMS 指的是关系型数据库管理系统。

RDBMS 是 SQL 的基础，同样也是所有现代数据库系统的基础，比如 MS SQL Server, IBM DB2, Oracle, MySQL 以及 Microsoft Access。

RDBMS 中的数据存储在被称为表(table)的数据库对象中。

表是相关的数据项的集合，它由列和行组成。

## 2. SQL 语法

### 2.1 数据库表

一个数据库通常包含一个或多个表。

表由一个名字标识（例如“客户”或者“订单”），它包含带有数据的记录（行）。

"Persons" 表：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

上面的表包含三条记录（每一条对应一个人）和五个列（Id、姓、名、地址和城市）。

### 2.2 SQL 语句

选取 "LastName" 列的数据：

```sql
SELECT LastName FROM Persons
```

结果：

|LastName| |
|---|---|
|Adams||
|Bush||
|Carter||

SQL 对大小写不敏感。

### 2.4 DML 与 DDL

可以把 SQL 分为两个部分：**数据操作语言**(DML)和**数据定义语言**(DDL)。

DML 包含查询与更新：

- `SELECT` - 从数据库表中获取数据
- `UPDATE` - 更新数据库表中的数据
- `DELETE` - 从数据库表中删除数据
- `INSERT INTO` - 向数据库表中插入数据

DDL 包含创建、删除与修改：

- `CREATE DATABASE` - 创建新数据库
- `ALTER DATABASE` - 修改数据库
- `CREATE TABLE` - 创建新表
- `ALTER TABLE` - 变更（改变）数据库表
- `DROP TABLE` - 删除表
- `CREATE INDEX` - 创建索引（搜索键）
- `DROP INDEX` - 删除索引

## 3. SELECT 语句

**SELECT 语句**：从表中选取数据，结果存储在**结果集**中。

### 3.1 SELECT 语法

```sql
--选取指定列
SELECT 列名称 FROM 表名称

--选取所有列
SELECT * FROM 表名称
```

"Persons" 表:

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

### 3.2 SELECT 实例 1：选取指定列

选取 "LastName" 与 "FirstName" 列：

```sql
SELECT LastName, FirstName FROM Persons
```

结果：

|LastName|FirstName|
|---|---|
|Adams|John|
|Bush|George|
|Carter|Thomas|

### 3.3 SELECT 实例 2：选取所有列

选取所有列：

```sql
SELECT * FROM Persons
```

结果：

|Id|LastName|FirstName|Address|City|
|---|---|---|---|---|
|1|Adams|John|Oxford Street|London|
|2|Bush|George|Fifth Avenue|New York|
|3|Carter|Thomas|Changan Street|Beijing|

## 4. DISTINCT 关键词

**DISTINCT 关键词**：选取所有不同的值。

### 4.1 DISTINCT 语法

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

### 4.2 DISTINCT 实例

选取所有不同的值：

```sql
SELECT DISTINCT Company FROM Orders
```

结果：

|Company| |
|---|---|
|IBM||
|W3School||
|Apple||

## 5. WHERE 子句

**WHERE 子句**：按指定条件筛选，作为 SELECT 的子句。

### 5.1 WHERE 语法

```sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

可用的运算符：

|操作符|描述|
|---|---|
|=|等于|
|<>|不等于|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
|BETWEEN|在某个范围内|
|LIKE|搜索某种模式|

### 5.2 WHERE 实例

"Persons" 表：

|LastName|FirstName|Address|City|Year|
|---|---|---|---|---|
|Adams|John|Oxford Street|London|1970|
|Bush|George|Fifth Avenue|New York|1975|
|Carter|Thomas|Changan Street|Beijing|1980|
|Gates|Bill|Xuanwumen 10|Beijing|1985|

选取城市为 "Beijing" 的人：

```sql
SELECT * FROM Persons WHERE City='Beijing'
```

结果：

|LastName|FirstName|Address|City|Year|
|---|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|1980|
|Gates|Bill|Xuanwumen 10|Beijing|1985|

### 5.3 引号

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

## 6. AND / OR 运算符

**运算符AND / OR**：连接多个条件。

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Adams|John|Oxford Street|London|
|Bush|George|Fifth Avenue|New York|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

### 6.1 AND 实例

选取所有姓为 "Carter" 并且名为 "Thomas" 的人：

```sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|

### 6.2 OR 实例

选取所有姓为 "Carter" 或者名为 "Thomas" 的人：

```sql
SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

### 6.3 AND 结合 OR 实例

选取所有姓为 "Carter"、并且名为 "Thomas" 或 "William" 的人：

```sql
SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William')
AND LastName='Carter'
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Carter|William|Xuanwumen 10|Beijing|

## 7. ORDER BY 子句

**ORDER BY 子句**：根据指定的列对结果集排序，默认按升序排序，作为 SELECT 的子句。

**DESC 关键词**：指定倒序排序。

"Orders" 表：

|Company|OrderNumber|
|---|---|
|IBM|3532|
|W3School|2356|
|Apple|4698|
|W3School|6953|

### 7.1 ORDER BY 实例 1

以字母顺序排序公司名称：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company
```

结果：

|Company|OrderNumber|
|---|---|
|Apple|4698|
|IBM|3532|
|W3School|6953|
|W3School|2356|

### 7.2 ORDER BY 实例 2

以字母顺序排序公司名称，并以数字顺序排序订单号：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
```

结果：

|Company|OrderNumber|
|---|---|
|Apple|4698|
|IBM|3532|
|W3School|2356|
|W3School|6953|

### 7.3 ORDER BY 实例 3

以字母顺序倒序排序公司名称：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC
```

结果：

|Company|OrderNumber|
|---|---|
|W3School|6953|
|W3School|2356|
|IBM|3532|
|Apple|4698|

### 7.4 ORDER BY 实例 4

以字母顺序倒序排序公司名称，并以数字顺序排序订单号：

```sql
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```

结果：

|Company|OrderNumber|
|---|---|
|W3School|2356|
|W3School|6953|
|IBM|3532|
|Apple|4698|

## 8. INSERT INTO 语句

**INSERT INTO 语句**：插入新行。

### 8.1 INSERT INTO 语法

```sql
INSERT INTO 表名称 VALUES (值1, 值2,....)

--可以指定所要插入数据的列
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|

### 8.2 INSERT INTO 实例 1：插入新行

插入新行：

```sql
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Gates|Bill|Xuanwumen 10|Beijing|

### 8.3 INSERT INTO 实例 2：插入新行并在指定列插入新值

插入新行并在指定列插入新值：

```sql
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Carter|Thomas|Changan Street|Beijing|
|Wilson||Champs-Elysees||

## 9. UPDATE 语句

**Update 语句**：修改数据。

### 9.1 UPDATE 语法

```sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson||Champs-Elysees||

### 9.2 UPDATE 实例 1：修改指定行、指定列

为姓氏为 "Wilson" 的人添加名：

```sql
UPDATE Persons SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Champs-Elysees||

### 9.3 UPDATE 实例 2：更新某一行中的若干列

为姓氏为 "Wilson" 的人修改地址并添加城市：

```sql
UPDATE Persons SET Address = 'Zhongshan 23', City = 'Nanjing' WHERE LastName = 'Wilson'
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Zhongshan 23|Nanjing|

## 10. DELETE 语句

**DELETE 语句**：删除行。

### 10.1 DELETE 语法

```sql
DELETE FROM 表名称 WHERE 列名称 = 值
```

"Persons" 表：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|
|Wilson|Fred|Zhongshan 23|Nanjing|

### 10.2 DELETE 实例 1：删除指定行

删除 "Wilson" 所在行：

```sql
DELETE FROM Person WHERE LastName = 'Wilson'
```

结果：

|LastName|FirstName|Address|City|
|---|---|---|---|
|Gates|Bill|Xuanwumen 10|Beijing|

### 10.3 DELETE 实例 2：删除所有行

在不删除表（保留表的完整结构）的情况下删除所有行：

```sql
DELETE FROM table_name
--或者
DELETE * FROM table_name
```