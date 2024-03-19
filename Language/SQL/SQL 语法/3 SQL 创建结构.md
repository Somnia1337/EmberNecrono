## 1. CREATE DATABASE 语句

**CREATE DATABASE 语句**：创建数据库。

### 1.1 SQL CREATE DATABASE 语法

```sql
CREATE DATABASE _database_name_
```

### 1.2 SQL CREATE DATABASE 实例

创建一个名为 "my_db" 的数据库：

```sql
CREATE DATABASE my_db
```

## 2. CREATE TABLE 语句

**CREATE TABLE 语句**：创建表。

### 2.1 CREATE TABLE 语法

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

### 2.2 CREATE TABLE 实例

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

FOREIGN KEY 约束用于防止破坏表之间连接的动作，也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

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