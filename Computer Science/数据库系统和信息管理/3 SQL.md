👈 [[2 关系模型介绍]]

### 创建

创建两张表 `Course` 和 `Section`：

```sql
CREATE TABLE
    Course
(
    course_id VARCHAR(10),
    title     VARCHAR(50),
    dept_name VARCHAR(20),
    credits   NUMERIC(2, 0),
    PRIMARY KEY (course_id) -- 指定主码
);

CREATE TABLE
    Section
(
    course_id   VARCHAR(8),
    sec_id      VARCHAR(8) NOT NULL,                      -- 不允许空值
    semester    VARCHAR(6),
    room_number VARCHAR(7),
    PRIMARY KEY (course_id, sec_id, semester),
    FOREIGN KEY (course_id) REFERENCES Course (course_id) -- 指定外码
);
```

### 修改

#### 插入

向 `Course` 插入一个元组：

```sql
INSERT INTO Course
VALUES ('201018030', '概率统计', 5);
```

为 `Section` 新增一个属性：

```sql
ALTER TABLE Section
    ADD building VARCHAR(20);
```

#### 删除

删除 `Section` 的属性 `room_number`：

```sql
ALTER TABLE Section
DROP
room_number;
```

删除 `Course` 的所有元组：

```sql
DELETE
FROM Course;
```

删除整个表 `Section`：

```sql
DROP TABLE Section;
```

### 查询

查询一个属性：

```sql
SELECT course_id
FROM Course;
```

去除重复项：

```sql
SELECT DISTINCT course_id
FROM Course;
```

查询的同时修改（临时）：

```sql
SELECT course_id,
       credits * 2
FROM Course;
```

指定条件：

```sql
SELECT course_id,
       credits
FROM Course
WHERE credits >= 3
  AND credits <= 4;
```

用 `BETWEEN` 简化：

```sql
SELECT course_id,
       credits
FROM Course
WHERE credits BETWEEN 3 AND 4;
```

从多个表中查询：

```sql
SELECT title,
       semester
FROM Course,
     Section
WHERE Course.course_id = Section.course_id;
```

用 `NATRUAL JOIN` 简化：

```sql
SELECT title,
       semester
FROM Course
         NATURAL JOIN Section;
```

### 字符串

用一对单引号 `'` 标识字符串。

匹配某种字符串模式：

- `%`：匹配任意子串。
- `_`：匹配一个字符。

```sql
SELECT title
FROM Course
WHERE title LIKE '%Science%';
```

```sql
SELECT title
FROM Course
WHERE title LIKE '_____'; -- 匹配恰好有 5 个字符的字符串
```

用 `ESCAPE` 定义转移字符：

```sql
SELECT title
FROM Course
WHERE title LIKE 'abc\%def%'; -- 匹配以 'abc%def' 开头的字符串
```

用 `*` 指代全部属性：

```sql
SELECT Course.*, Section.*
FROM Course
         NATURAL JOIN Section;
```

用 `ORDER BY` 配合 `ASC` / `DESC` 指定结果顺序：

```sql
SELECT course_id
FROM Course
ORDER BY course_id DESC;
```

### 集合运算

`UNION` 表示并运算 $\cup$：

```sql
(SELECT course_id
 FROM Course
 WHERE course_id LIKE '2022%'
   AND title LIKE '%bio%')
UNION
(SELECT course_id
 FROM Course
 WHERE course_id LIKE '2019%'
   AND title LIKE '%chem%');
```

`INTERSECT` 表示交运算 $\cap$：

```sql
(SELECT course_id
 FROM Course
 WHERE course_id > '2022'
   AND title LIKE '%Biology%')
INTERSECT
(SELECT course_id
 FROM Course
 WHERE course_id < '2019'
   AND title LIKE '%chem%');
```

`EXCEPT` 表示差运算 $-$：

```sql
(SELECT course_id
 FROM Course
 WHERE course_id LIKE '2022%'
   AND title LIKE '%Biology%')
EXCEPT
(SELECT course_id
 FROM Course
 WHERE course_id LIKE '2019%'
   AND title LIKE '%chem%');
```

`UNION`、`INTERSECT` 和 `EXCEPT` 都自动去重，如需保留，需用 `~ ALL`。

### 空值

第 3 种布尔值 `unknown`：`1 < null` 等无法比较的结果为 `unknown`。

### 聚集函数

- `avg()`
- `min()`
- `max()`
- `sum()`
- `count()`

```sql
SELECT AVG(Credits)
FROM Course
WHERE course_id LIKE '2020%';
```

`GROUP BY`：在 `GROUP BY` 子句中，所有在其指定的属性上相同的元组被分到同一组。

```sql
SELECT AVG(Credits)
FROM Course
GROUP BY SUBSTRING(course_id, 0, 4);
```

`HAVING`：对 `GROUP BY` 子句添加限定条件，在分组之后才检查其谓词。

```sql
SELECT AVG(credits)
FROM Course
GROUP BY SUBSTRING(course_id, 0, 4)
HAVING AVG(credits) > 2;
```

### 子查询

用子查询改写上文的 `UNION`：

```sql
SELECT course_id
FROM Course
WHERE course_id LIKE '2022%'
  AND title LIKE '%bio%'
  AND course_id IN
      (SELECT course_id
       FROM Course
       WHERE course_id LIKE '2019%'
         AND title LIKE '%chem%');
```

也可用于枚举集合：

```sql
SELECT course_id
FROM Course
WHERE title IN ('Biology', 'Geology');
```

用 `SOME` / `ALL` 与子查询结果比较：

```sql
SELECT course_id
FROM Course
WHERE credits >= ALL (SELECT credits
                     FROM Course
                     WHERE title LIKE '%bio%');
```

---

| Sales       |
| ----------- |
| sale_id     |
| product     |
| quantity    |
| sale_date   |
| customer_id |
| sale_amount |

> 1\. 选择订单总数和总金额。

```sql
SELECT 
	COUNT(sale_id) AS total_orders,
	SUM(sale_amount) AS total_amount
FROM Sales;
```

>2\. 选择每种产品的总金额。

```sql
SELECT
	product,
	SUM(sale_amount) AS total_amount
FROM Sales
GROUP BY product;
```

> 3\. 选择售出超过 `10` 件的产品的品名和售出量。

```sql
SELECT
	product,
	SUM(quantity) AS total_quantity
FROM Sales
GROUP BY product
HAVING total_quantity > 10;
```

> 4\. 选择每个订单的金额都大于所有订单的平均金额的产品。

```sql
SELECT product
FROM Sales
GROUP BY product
HAVING
	MIN(sale_amount) > (
		SELECT AVG(sale_amount)
		FROM Sales
	);
```

> 5\. 选择总金额大于一些订单的金额、但不是大于所有订单的金额的产品。

```sql
SELECT product
FROM Sales
GROUP BY product
HAVING
	SUM(sale_amount) > SOME (SELECT sale_amount FROM Sales) 
	AND SUM(sale_amount) <= ALL (SELECT sale_amount FROM Sales);
```