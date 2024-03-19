```text
175 181 182 183 196 197 511 512 577 584 586 595 596 597 603 607 610 619 620 627 1050 1068 1069 1075 1076 1082 1083 1084 1141 1142 1148 1173 1179 1241 1251 1280 1294 1303 1321 1322 1327 1341 1350 1378 1407 1421 1468 1484 1495 1511 1517 1527 1543 1565 1571 1581 1607 1623 1633 1757 1821 1873
```

#### [175. 组合两个表](https://leetcode.cn/problems/combine-two-tables/)

@2023.11.08

```text
Person
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+

Address
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+

Output
+-----------+----------+---------------+----------+
| firstName | lastName | city          | state    |
+-----------+----------+---------------+----------+
```

用左连接使在 `Address` 中找不到的行也被列出。

```sql
SELECT
    Person.firstName,
    Person.lastName,
    Address.city,
    Address.state
FROM
    Person
    LEFT JOIN Address ON Person.personId = Address.personId;
```

#### [181. 超过经理收入的员工](https://leetcode.cn/problems/employees-earning-more-than-their-managers/)

@2023.11.08

```text
Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+

Output
+----------+
| Employee |
+----------+
```

查询 `e1`、`e2`，满足 `e1.managerId=e2.id` 且 `e1.salary>e2.salary`。

```sql
SELECT
    e1.name AS employee
FROM
    Employee e1,
    Employee e2
WHERE
    e1.managerId = e2.id
    AND e1.salary > e2.salary;
```

#### [182. 查找重复的电子邮箱](https://leetcode.cn/problems/duplicate-emails/)

@2023.11.08

```text
Person
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+

Output
+---------+
| Email   |
+---------+
```

按 `email` 分组，选择计数大于 1 的分组的 `email`。

```sql
SELECT
    email
FROM
    Person
GROUP BY
    email
HAVING
    count(email) > 1;
```

#### [183. 从不订购的客户](https://leetcode.cn/problems/customers-who-never-order/)

@2023.11.08

```text
Customers
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+

Orders
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+

Output
+-----------+
| Customers |
+-----------+
```

1. 排除条件

内层 `SELECT` 从 `Orders` 中选择所有 `customerId`，外层 `SELECT` 从 `Customers` 中选择所有未在内层表中出现的 `id` 对应的 `name`。

```sql
SELECT
    name AS customers
FROM
    Customers
WHERE
    id NOT IN (
        SELECT
            customerId
        FROM
            Orders
    );
```

2. 左连接

将 `Customers.id` 与 `Orders.customerId` 相同的行左连接，从中选择 `Orders.customerId` 为空对应的 `name`。

```sql
SELECT
    name AS customers
FROM
    Customers
    LEFT JOIN Orders ON Customers.id = Orders.customerId
WHERE
    Orders.customerId IS NULL;
```

#### [196. 删除重复的电子邮箱](https://leetcode.cn/problems/delete-duplicate-emails/)

@2023.11.08

```text
Person
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+

Output
+----+------------------+
| id | email            |
+----+------------------+
```

内层 `SELECT` 将 `Person` 按 `email` 分组，从每组中选择 `id` 最小的行，别名 `keep`，外层 `DELETE` 从 `Person` 中删除不在 `keep` 中的行。

```sql
DELETE FROM Person
WHERE
    id NOT IN (
        SELECT
            m
        FROM
            (
                SELECT
                    MIN(id) AS m
                FROM
                    Person
                GROUP BY
                    email
            ) AS keep
    );
```

#### [197. 上升的温度](https://leetcode.cn/problems/rising-temperature/)

@2023.11.10

```text
Weather
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+

Output
+----+
| id |
+----+
```

`SUBDATE` 的使用。

```sql
SELECT
    b.id
FROM
    Weather AS a,
    Weather AS b
WHERE
    a.recordDate = SUBDATE (b.recordDate, 1) # a 的日期为 b 的前 1 天
    AND b.temperature > a.temperature;
```

#### [511. 游戏玩法分析 I](https://leetcode.cn/problems/game-play-analysis-i/)

@2023.11.10

```text
Activity
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+

Output
+-----------+-------------+
| player_id | first_login |
+-----------+-------------+
```

分组，选最小日期。

```sql
SELECT DISTINCT
    player_id,
    MIN(event_date) AS first_login
FROM
    Activity
GROUP BY
    player_id;
```

#### [512. 游戏玩法分析 II](https://leetcode.cn/problems/game-play-analysis-ii/)

@2023.11.10

```text
Activity
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+

Output
+-----------+-----------+
| player_id | device_id |
+-----------+-----------+
```

子查询为 [511. 游戏玩法分析 I](https://leetcode.cn/problems/game-play-analysis-i/)。

```sql
SELECT
    player_id,
    device_id
FROM
    Activity
WHERE
    (player_id, event_date) IN (
        SELECT
            player_id,
            MIN(event_date)
        FROM
            Activity
        GROUP BY
            player_id
    );
```

#### [577. 员工奖金](https://leetcode.cn/problems/employee-bonus/)

@2023.11.09

```text
Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+

Bonus
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+

Output
+------+-------+
| name | bonus |
+------+-------+
```

`LEFT JOIN` 两表。

```sql
SELECT
    e.name,
    b.bonus
FROM
    Employee AS e
    LEFT JOIN Bonus AS b ON e.empId = b.empId
WHERE
    bonus < 1000
    OR bonus IS NULL;
```

#### [584. 寻找用户推荐人](https://leetcode.cn/problems/find-customer-referee/)

@2023.11.09

```text
Customer
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+

Output
+------+
| name |
+------+
```

```sql
SELECT
    name
FROM
    Customer
WHERE
    referee_id <> 2
    OR referee_id IS NULL;
```

#### [586. 订单最多的客户](https://leetcode.cn/problems/customer-placing-the-largest-number-of-orders/)

@2023.11.09

```text
Orders
+-----------------+----------+
| Column Name     | Type     |
+-----------------+----------+
| order_number    | int      |
| customer_number | int      |
+-----------------+----------+

Output
+-----------------+
| customer_number |
+-----------------+
```

`GROUP BY` 后 `COUNT(*)`。

```sql
SELECT
    customer_number
FROM
    Orders
GROUP BY
    customer_number
ORDER BY
    COUNT(*) DESC
LIMIT
    1;
```

#### [595. 大的国家](https://leetcode.cn/problems/big-countries/)

@2023.11.09

```text
World
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+

Output
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
```

```sql
SELECT
    name,
    population,
    area
FROM
    World
WHERE
    area >= 3000000
    OR population >= 25000000;
```

#### [596. 超过5名学生的课](https://leetcode.cn/problems/classes-more-than-5-students/)

@2023.11.09

```text
Courses
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+

Output
+---------+ 
| class   | 
+---------+
```

```sql
SELECT
    class
FROM
    Courses
GROUP BY
    class
HAVING
    COUNT(*) >= 5;
```

#### [597. 好友申请 I：总体通过率](https://leetcode.cn/problems/friend-requests-i-overall-acceptance-rate/)

@2023.11.13

```text
FriendRequest
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| sender_id      | int     |
| send_to_id     | int     |
| request_date   | date    |
+----------------+---------+

RequestAccepted
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+

Output
+-------------+
| accept_rate |
+-------------+
```

```sql
SELECT
    ROUND(
        ifnull (
            COUNT(DISTINCT r.requester_id, r.accepter_id) / COUNT(DISTINCT f.sender_id, f.send_to_id),
            0
        ),
        2
    ) accept_rate
FROM
    FriendRequest AS f,
    RequestAccepted AS r;
```

#### [603. 连续空余座位](https://leetcode.cn/problems/consecutive-available-seats/)

@2023.11.10

```text
Cinema
+-------------+------+
| Column Name | Type |
+-------------+------+
| seat_id     | int  |
| free        | bool |
+-------------+------+

Output
+---------+
| seat_id |
+---------+
```

选择存在 `b.seat_id` 满足 `ABS(a.seat_id-b.seat_id)=1` 且 `a.free=b.free=1` 的 `a.seat_id`。

```sql
SELECT DISTINCT
    a.seat_id
FROM
    Cinema AS a,
    Cinema AS b
WHERE
    ABS(a.seat_id - b.seat_id) = 1
    AND a.free = 1
    AND b.free = 1
ORDER BY
    1;
```

#### [607. 销售员](https://leetcode.cn/problems/sales-person/)

@2023.11.09

```text
SalesPerson
+-----------------+---------+
| Column Name     | Type    |
+-----------------+---------+
| sales_id        | int     |
| name            | varchar |
| salary          | int     |
| commission_rate | int     |
| hire_date       | date    |
+-----------------+---------+

Company
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| com_id      | int     |
| name        | varchar |
| city        | varchar |
+-------------+---------+

Orders
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| order_date  | date |
| com_id      | int  |
| sales_id    | int  |
| amount      | int  |
+-------------+------+

Output
+------+
| name |
+------+
```

双重嵌套 `SELECT`。

```sql
SELECT
    name
FROM
    SalesPerson
WHERE
    sales_id NOT IN ( # 排除 com_id 对应 'RED' 公司 sales_id
        SELECT
            sales_id
        FROM
            Orders
        WHERE
            com_id IN ( # 选出 'RED' 公司的 com_id
                SELECT
                    com_id
                FROM
                    Company
                WHERE
                    name = 'RED'
            )
    );
```

#### [610. 判断三角形](https://leetcode.cn/problems/triangle-judgement/)

@2023.11.09

```text
Triangle
+-------------+------+
| Column Name | Type |
+-------------+------+

Output
+----+----+----+----------+
| x  | y  | z  | triangle |
+----+----+----+----------+
```

```sql
SELECT
    x,
    y,
    z,
    IF (
        x + y > z
        AND x + z > y
        AND y + z > x,
        'Yes',
        'No'
    ) AS triangle
FROM
    Triangle;
```

#### [613. 直线上的最近距离](https://leetcode.cn/problems/shortest-distance-in-a-line/)

@2023.11.10

```text
Point
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
+-------------+------+

Output
+----------+
| shortest |
+----------+
```

选择满足 `a.x<>b.x` 的 `MIN(ABS(a.x-b.x))`。

```sql
SELECT
    MIN(ABS(a.x - b.x)) AS shortest
FROM
    Point AS a,
    Point AS b
WHERE
    a.x <> b.x;
```

#### [619. 只出现一次的最大数字](https://leetcode.cn/problems/biggest-single-number/)

@2023.11.10

```text
MyNumbers
+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
+-------------+------+

Output
+------+
| num  |
+------+
```

1. 子查询

子查询选择只出现一次的数字，父查询选择其中的最大者。

```sql
SELECT
    MAX(num) AS num
FROM
    MyNumbers
WHERE
    num IN (
        SELECT
            num
        FROM
            MyNumbers
        GROUP BY
            num
        HAVING
            COUNT(*) = 1
    );
```

2. 排序

若计数为 1 则为 `num` 本身，否则为 `NULL`，倒序排序，限制 1 条。

```sql
SELECT
    IF (COUNT(num) = 1, num, NULL) AS num
FROM
    MyNumbers
GROUP BY
    num
ORDER BY
    COUNT(num),
    num DESC
LIMIT
    1;
```

#### [620. 有趣的电影](https://leetcode.cn/problems/not-boring-movies/)

@2023.11.09

```text
Cinema
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+

Output
+---------+-----------+--------------+-----------+
|   id    | movie     |  description |  rating   |
+---------+-----------+--------------+-----------+
```

```sql
SELECT
    id,
    movie,
    description,
    rating
FROM
    Cinema
WHERE
    (id & 1) = 1
    AND description <> 'boring'
ORDER BY
    rating DESC;
```

#### [627. 变更性别](https://leetcode.cn/problems/swap-salary/)

@2023.11.10

```text
Salary
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| name        | varchar  |
| sex         | ENUM     |
| salary      | int      |
+-------------+----------+

Output
+----+------+-----+--------+
| id | name | sex | salary |
+----+------+-----+--------+
```

`UPDATE` 与 `IF`。

```sql
UPDATE Salary
SET
    sex = IF (sex = 'm', 'f', 'm');
```

#### [1050. 合作过至少三次的演员和导演](https://leetcode.cn/problems/actors-and-directors-who-cooperated-at-least-three-times/)

@2023.11.10

```text
ActorDirector
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| actor_id    | int     |
| director_id | int     |
| timestamp   | int     |
+-------------+---------+

Output
+-------------+-------------+
| actor_id    | director_id |
+-------------+-------------+
```

根据两个 `id` 分组，选出计数至少为 3 者。

```sql
SELECT
    actor_id,
    director_id
FROM
    ActorDirector
GROUP BY
    actor_id,
    director_id
HAVING
    COUNT(*) >= 3;
```

#### [1068. 产品销售分析 I](https://leetcode.cn/problems/product-sales-analysis-i/)

@2023.11.10

```text
Sales
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+

Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+

Output
+--------------+-------+-------+
| product_name | year  | price |
+--------------+-------+-------+
```

`Sales LEFT JOIN Product ON product_id`。

```sql
SELECT
    p.product_name,
    s.year,
    s.price
FROM
    Sales AS s
    LEFT JOIN Product AS p ON s.product_id = p.product_id;
```

#### [1069. 产品销售分析 II](https://leetcode.cn/problems/product-sales-analysis-ii/)

@2023.11.10

```text
Sales
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+

Product (not used)
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+

Output
+--------------+----------------+
| product_id   | total_quantity |
+--------------+----------------+
```

分组，对 `quantity` 求和。

```sql
SELECT
    product_id,
    SUM(quantity) AS total_quantity
FROM
    Sales
GROUP BY
    product_id;
```

#### [1075. 项目员工 I](https://leetcode.cn/problems/project-employees-i/)

@2023.11.13

```text
Project
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+

Employee
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+

Output
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
```

```sql
SELECT
    project_id,
    ROUND(SUM(e.experience_years) / COUNT(p.employee_id), 2) AS average_years
FROM
    Project AS p,
    Employee AS e
WHERE
    p.employee_id = e.employee_id
GROUP BY
    p.project_id;
```

#### [1076. 项目员工II](https://leetcode.cn/problems/project-employees-ii/)

@2023.11.10

```text
Project
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+

Employee (not used)
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+

Output
+-------------+
| project_id  |
+-------------+
```

子查询选择所有项目的员工计数，父查询选择计数大于所有子查询计数的项目。

```sql
SELECT
    project_id
FROM
    Project
GROUP BY
    project_id
HAVING
    COUNT(employee_id) >= ALL (
        SELECT
            COUNT(employee_id)
        FROM
            Project
        GROUP BY
            project_id
    );
```

#### [1082. 销售分析 I](https://leetcode.cn/problems/sales-analysis-i/)

@2023.11.13

```text
Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+

Sales
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+------ ------+---------+

Output
+-------------+
| seller_id   |
+-------------+
```

```sql
SELECT
    seller_id
FROM
    Sales
GROUP BY
    seller_id
HAVING
    SUM(price) >= ALL (
        SELECT
            SUM(price)
        FROM
            sales
        GROUP BY
            seller_id
    );
```

#### [1083. 销售分析 II](https://leetcode.cn/problems/sales-analysis-ii/)

@2023.11.13

```text
Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+

Sales
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+------ ------+---------+

Output
+-------------+
| buyer_id    |
+-------------+
```

```sql
SELECT DISTINCT
    buyer_id
FROM
    Sales AS s
WHERE
    buyer_id IN (
        SELECT
            buyer_id
        FROM
            Sales
        WHERE
            product_id IN (
                SELECT
                    product_id
                FROM
                    Product
                WHERE
                    product_name = 'S8'
            )
    )
    AND buyer_id NOT IN (
        SELECT
            buyer_id
        FROM
            Sales
        WHERE
            product_id IN (
                SELECT
                    product_id
                FROM
                    Product
                WHERE
                    product_name = 'iPhone'
            )
    );
```

```sql
SELECT
    s.buyer_id
FROM
    Product AS p,
    Sales AS s
WHERE
    p.product_id = s.product_id
GROUP BY
    s.buyer_id
HAVING
    SUM(p.product_name = 'S8') > 0
    AND SUM(p.product_name = 'iphone') < 1;
```

#### [1084. 销售分析III](https://leetcode.cn/problems/sales-analysis-iii/)

@2023.11.13

```text
Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+

Sales
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+------ ------+---------+

Output
+-------------+
| buyer_id    |
+-------------+
```

```sql
SELECT
    p.product_id AS product_id,
    product_name
FROM
    Product AS p,
    Sales AS s
WHERE
    p.product_id = s.product_id
GROUP BY
    p.product_id
HAVING
    MIN(sale_date) >= '2019-01-01'
    AND MAX(sale_date) <= '2019-03-31';
```

#### [1141. 查询近30天活跃用户数](https://leetcode.cn/problems/user-activity-for-the-past-30-days-i/)

@2023.11.14

```text
Activity
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |
+---------------+---------+

Output
+------------+--------------+ 
| day        | active_users |
+------------+--------------+
```

```sql
SELECT
    activity_date AS DAY,
    COUNT(DISTINCT user_id) AS active_users
FROM
    Activity
WHERE
    activity_date <= '2019-07-27'
    AND activity_date > '2019-06-27'
GROUP BY
    activity_date;
```

#### [1142. 过去30天的用户活动 II](https://leetcode.cn/problems/user-activity-for-the-past-30-days-ii/)

@2023.11.14

```text
Activity
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |
+---------------+---------+

Output
+------------+--------------+ 
| day        | active_users |
+------------+--------------+
```

```sql
SELECT
    IFNULL (
        ROUND(
            (COUNT(DISTINCT session_id)) / (COUNT(DISTINCT user_id)),
            2
        ),
        0
    ) AS average_sessions_per_user
FROM
    Activity
WHERE
    activity_date <= '2019-07-27'
    AND activity_date > '2019-06-27';
```

#### [1148. 文章浏览 I](https://leetcode.cn/problems/article-views-i/)

@2023.11.09

```text
Views
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+

Output
+------+
| id   |
+------+
```

```sql
SELECT DISTINCT
    author_id AS id
FROM
    Views
WHERE
    author_id = viewer_id
ORDER BY
    author_id;
```

#### [1173. 即时食物配送 I](https://leetcode.cn/problems/immediate-food-delivery-i/)

@2023.11.14

```text
Delivery
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+

Output
+----------------------+
| immediate_percentage |
+----------------------+
```

```sql
SELECT
    ROUND(
        (
            SUM(
                IF (order_date = customer_pref_delivery_date, 1, 0)
            ) * 100
        ) / COUNT(*),
        2
    ) AS immediate_percentage
FROM
    Delivery;
```

#### [1179. 重新格式化部门表](https://leetcode.cn/problems/reformat-department-table/)

@2023.11.14

```text
Department
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| revenue       | int     |
| month         | varchar |
+---------------+---------+

Output
+----+-------------+-------------+-----+-------------+
| id | Jan_Revenue | Feb_Revenue | ... | Dec_Revenue |
+----+-------------+-------------+-----+-------------+
```

```sql
SELECT
    id,
    SUM(IF (MONTH = 'Jan', revenue, NULL)) AS Jan_Revenue,
    SUM(IF (MONTH = 'Feb', revenue, NULL)) AS Feb_Revenue,
    SUM(IF (MONTH = 'Mar', revenue, NULL)) AS Mar_Revenue,
    SUM(IF (MONTH = 'Apr', revenue, NULL)) AS Apr_Revenue,
    SUM(IF (MONTH = 'May', revenue, NULL)) AS May_Revenue,
    SUM(IF (MONTH = 'Jun', revenue, NULL)) AS Jun_Revenue,
    SUM(IF (MONTH = 'Jul', revenue, NULL)) AS Jul_Revenue,
    SUM(IF (MONTH = 'Aug', revenue, NULL)) AS Aug_Revenue,
    SUM(IF (MONTH = 'Sep', revenue, NULL)) AS Sep_Revenue,
    SUM(IF (MONTH = 'Oct', revenue, NULL)) AS Oct_Revenue,
    SUM(IF (MONTH = 'Nov', revenue, NULL)) AS Nov_Revenue,
    SUM(IF (MONTH = 'Dec', revenue, NULL)) AS Dec_Revenue
FROM
    Department
GROUP BY
    id;
```

#### [1241. 每个帖子的评论数](https://leetcode.cn/problems/number-of-comments-per-post/)

@2023.11.09

```text
Submissions
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| sub_id        | int      |
| parent_id     | int      |
+---------------+----------+

Output
+---------+--------------------+
| post_id | number_of_comments |
+---------+--------------------+
```

```sql
SELECT
    s1.sub_id AS post_id,
    COUNT(DISTINCT s2.sub_id) AS number_of_comments
FROM
    Submissions AS s1
    LEFT JOIN Submissions AS s2 ON s1.sub_id = s2.parent_id
WHERE
    s1.parent_id IS NULL
GROUP BY
    s1.sub_id
ORDER BY
    1;
```

#### [1251. 平均售价](https://leetcode.cn/problems/average-selling-price/)

@2023.11.14

```text
Prices
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+

UnitsSold
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+

Output
+------------+---------------+
| product_id | average_price |
+------------+---------------+
```

```sql
SELECT
    p.product_id AS product_id,
    IFNULL (ROUND(SUM(price * units) / SUM(units), 2), 0) AS average_price
FROM
    Prices AS p
    LEFT JOIN UnitsSold AS u ON u.product_id = p.product_id
    AND purchase_date >= start_date
    AND purchase_date <= end_date
GROUP BY
    product_id;
```

#### [1280. 学生们参加各科测试的次数](https://leetcode.cn/problems/students-and-examinations/)

@2023.11.09

```text
Students
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+

Subjects
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+

Examinations
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+

Output
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
```

```sql
SELECT
    s.student_id,
    s.student_name,
    j.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM
    Students s
    JOIN Subjects j
    LEFT JOIN Examinations e ON e.student_id = s.student_id
    AND j.subject_name = e.subject_name
GROUP BY
    s.student_id,
    j.subject_name
ORDER BY
    s.student_id,
    j.subject_name;
```

#### [1294. 不同国家的天气类型](https://leetcode.cn/problems/weather-type-in-each-country/)

@2023.11.14

```text
Countries
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| country_id    | int     |
| country_name  | varchar |
+---------------+---------+

Weather
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| country_id    | int     |
| weather_state | varchar |
| day           | date    |
+---------------+---------+

Output
+--------------+--------------+
| country_name | weather_type |
+--------------+--------------+
```

```sql
SELECT
    country_name,
    (
        CASE
            WHEN t_avg <= 15 THEN 'Cold'
            WHEN t_avg >= 25 THEN 'Hot'
            ELSE 'Warm'
        END
    ) AS weather_type
FROM
    (
        SELECT
            country_name,
            AVG(weather_state) AS t_avg
        FROM
            Countries AS c
            LEFT JOIN Weather AS w ON c.country_id = w.country_id
        WHERE
            w.day LIKE '2019-11%'
        GROUP BY
            country_name
    ) AS t;
```

#### [1303. 求团队人数](https://leetcode.cn/problems/find-the-team-size/)

@2023.11.09

```text
Employee
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| team_id       | int     |
+---------------+---------+

Output
+-------------+------------+
| employee_id | team_size  |
+-------------+------------+
```

常规做法：

```sql
SELECT
    e1.employee_id,
    count(*) team_size
FROM
    employee e1
    JOIN employee e2 ON e1.team_id = e2.team_id
GROUP BY
    e1.employee_id;
```

窗口函数：

```sql
SELECT
    employee_id,
    COUNT(*) OVER (
        PARTITION BY
            team_id
    ) AS team_size
FROM
    Employee;
```

#### [1321. 餐馆营业额变化增长](https://leetcode.cn/problems/restaurant-growth/)

@2023.11.11

```text
Customer
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+

Output
+--------------+--------------+----------------+
| visited_on   | amount       | average_amount |
+--------------+--------------+----------------+
```

```sql
SELECT
    a.visited_on,
    SUM(b.amount) AS amount,
    ROUND(SUM(b.amount) / 7, 2) AS average_amount
FROM
    (
        SELECT DISTINCT
            visited_on
        FROM
            Customer
    ) AS a
    JOIN Customer AS b ON datediff (a.visited_on, b.visited_on) BETWEEN 0 AND 6
WHERE
    a.visited_on >= (
        SELECT
            MIN(visited_on)
        FROM
            Customer
    ) + 6
GROUP BY
    a.visited_on
ORDER BY
    visited_on;
```

#### [1322. 广告效果](https://leetcode.cn/problems/ads-performance/)

@2023.11.14

```text
Ads
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| ad_id         | int     |
| user_id       | int     |
| action        | enum    |
+---------------+---------+

Output
+-------+-------+
| ad_id | ctr   |
+-------+-------+
```

```sql
SELECT
    ad_id,
    IFNULL (
        ROUND(
            SUM(IF (action = 'Clicked', 1, 0)) * 100 / SUM(
                IF (
                    action = 'Clicked'
                    OR action = 'Viewed',
                    1,
                    0
                )
            ),
            2
        ),
        0
    ) AS ctr
FROM
    Ads
GROUP BY
    ad_id
ORDER BY
    ctr desc,
    ad_id;
```

`SUM(IF(exp, 1, 0))` 可简化为 `SUM(exp)`。

```sql
SELECT
    ad_id,
    IFNULL (
        ROUND(
            SUM(action = 'Clicked') * 100 / SUM(action <> 'Ignored'),
            2
        ),
        0
    ) AS ctr
FROM
    Ads
GROUP BY
    ad_id
ORDER BY
    ctr desc,
    ad_id;
```

#### [1327. 列出指定时间段内所有的下单产品](https://leetcode.cn/problems/list-the-products-ordered-in-a-period/)

@2023.11.14

```text
Products
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
+------------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
+---------------+---------+

Output
+--------------------+---------+
| product_name       | unit    |
+--------------------+---------+
```

```sql
SELECT
    product_name,
    SUM(unit) AS unit
FROM
    Orders AS o
    LEFT JOIN Products AS p ON o.product_id = p.product_id
WHERE
    order_date LIKE '2020-02%'
GROUP BY
    o.product_id
HAVING
    SUM(unit) >= 100;
```

#### [1341. 电影评分](https://leetcode.cn/problems/movie-rating/)

@2023.11.11

```text
Movies
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+

Users
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+

MovieRating
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+

Output
+--------------+
| results      |
+--------------+
```

```sql
(
    SELECT
        title AS results
    FROM
        MovieRating AS mr,
        Movies AS m
    WHERE
        mr.movie_id = m.movie_id
        AND mr.created_at BETWEEN '2020-02-01' AND '2020-02-29'
    GROUP BY
        m.movie_id
    ORDER BY
        AVG(mr.rating) DESC,
        title
    LIMIT
        1
)
UNION ALL
(
    SELECT
        u.name AS results
    FROM
        MovieRating mr,
        Users u
    WHERE
        u.user_id = mr.user_id
    GROUP BY
        u.user_id
    ORDER BY
        COUNT(u.user_id) DESC,
        u.name
    LIMIT
        1
);
```

#### [1350. 院系无效的学生](https://leetcode.cn/problems/students-with-invalid-departments/)

@2023.11.09

```text
Departments
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

Students
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
| department_id | int     |
+---------------+---------+

Output
+------+----------+
| id   | name     |
+------+----------+
```

```sql
SELECT
    s.id,
    s.name
FROM
    Students AS s
WHERE
    s.department_id NOT IN (
        SELECT
            d.id
        FROM
            Departments AS d
    );
```

#### [1378. 使用唯一标识码替换员工ID](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/)

@2023.11.14

```text
Employees
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

EmployeeUNI
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+

Output
+-----------+----------+
| unique_id | name     |
+-----------+----------+
```

```sql
SELECT
    unique_id,
    name
FROM
    Employees AS e
    LEFT JOIN EmployeeUNI AS u ON e.id = u.id;
```

#### [1407. 排名靠前的旅行者](https://leetcode.cn/problems/top-travellers/)

@2023.11.15

```text
Users
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

Rides
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| user_id       | int     |
| distance      | int     |
+---------------+---------+

Output
+----------+--------------------+
| name     | travelled_distance |
+----------+--------------------+
```

```sql
SELECT
    name,
    IFNULL (SUM(distance), 0) AS travelled_distance
FROM
    Users AS u
    LEFT JOIN Rides AS r ON u.id = r.user_id
GROUP BY
    u.id
ORDER BY
    travelled_distance DESC,
    name;
```

#### [1421. 净现值查询](https://leetcode.cn/problems/npv-queries/)

@2023.11.16

```text
NPV
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| year          | int     |
| npv           | int     |
+---------------+---------+

Queries
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| year          | int     |
+---------------+---------+

Output
+------+--------+--------+
| id   | year   | npv    |
+------+--------+--------+
```

```sql
SELECT
    q.*,
    IFNULL (n.npv, 0) AS 'npv'
FROM
    Queries AS q
    LEFT JOIN NPV AS n ON q.id = n.id
    AND n.year = q.year;
```

#### [1468. 计算税后工资](https://leetcode.cn/problems/calculate-salaries/)

@2023.11.12

```text
Salaries
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| company_id    | int     |
| employee_id   | int     |
| employee_name | varchar |
| salary        | int     |
+---------------+---------+

Output
+------------+-------------+---------------+--------+
| company_id | employee_id | employee_name | salary |
+------------+-------------+---------------+--------+
```

```sql
SELECT
    company_id,
    employee_id,
    employee_name,
    CASE
        WHEN MAX(salary) OVER (
            PARTITION BY
                company_id
        ) < 1000 THEN salary
        WHEN MAX(salary) OVER (
            PARTITION BY
                company_id
        ) BETWEEN 1000 AND 10000 THEN ROUND(0.76 * salary, 0)
        ELSE ROUND(0.51 * salary, 0)
    END AS salary
FROM
    Salaries
ORDER BY
    company_id,
    employee_id;
```

#### [1484. 按日期分组销售产品](https://leetcode.cn/problems/group-sold-products-by-the-date/)

@2023.11.16

```text
Activities
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| sell_date   | date    |
| product     | varchar |
+-------------+---------+

Output
+------------+----------+------------------------------+
| sell_date  | num_sold | products                     |
+------------+----------+------------------------------+
```

`group_concat` 的使用。

```sql
SELECT
    sell_date,
    COUNT(DISTINCT product) num_sold,
    group_concat(
        DISTINCT product
        ORDER BY
            product separator ','
    ) products
FROM
    Activities
GROUP BY
    sell_date
ORDER BY
    sell_date;
```

#### [1495. 上月播放的儿童适宜电影](https://leetcode.cn/problems/friendly-movies-streamed-last-month/)

@2023.11.15

```text
TVProgram
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| program_date  | date    |
| content_id    | int     |
| channel       | varchar |
+---------------+---------+

Content
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| content_id       | varchar |
| title            | varchar |
| Kids_content     | enum    |
| content_type     | varchar |
+------------------+---------+

Output
+--------------+
| title        |
+--------------+
```

```sql
SELECT DISTINCT
    title
FROM
    Content AS c
    JOIN TVProgram AS p ON c.content_id = p.content_id
WHERE
    kids_content = 'Y'
    AND content_type = 'Movies'
    AND program_date LIKE '2020-06%';
```

#### [1511. 消费者下单频率](https://leetcode.cn/problems/customer-order-frequency/)

@2023.11.16

```text
Customers
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| country       | varchar |
+---------------+---------+

Product
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| description   | varchar |
| price         | int     |
+---------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_id    | int     |
| order_date    | date    |
| quantity      | int     |
+---------------+---------+

Output
+----------+-------------+------------+------------+----------+
| order_id | customer_id | product_id | order_date | quantity |
+----------+-------------+------------+------------+----------+
```

```sql
SELECT
    c.customer_id AS customer_id,
    c.name AS name
FROM
    Customers AS c
WHERE
    c.customer_id IN (
        SELECT
            o.customer_id
        FROM
            Orders AS o
            LEFT JOIN Product AS c ON o.product_id = c.product_id
        WHERE
            order_date LIKE '2020-06%'
        GROUP BY
            o.customer_id
        HAVING
            SUM(price * quantity) >= 100
    )
    AND c.customer_id IN (
        SELECT
            o.customer_id
        FROM
            Orders AS o
            LEFT JOIN Product AS c ON o.product_id = c.product_id
        WHERE
            order_date LIKE '2020-07%'
        GROUP BY
            o.customer_id
        HAVING
            SUM(price * quantity) >= 100
    );
```

```sql
SELECT
    o.customer_id,
    c.name
FROM
    Orders AS o
    LEFT JOIN Customers AS c USING (customer_id)
    LEFT JOIN Product AS p USING (product_id)
WHERE
    order_date LIKE "2020-06%"
    OR "2020-07%"
GROUP BY
    customer_id
HAVING
    SUM(
        CASE
            WHEN order_date LIKE "2020-06%" THEN quantity * price
            ELSE 0
        END
    ) >= 100
    AND SUM(
        CASE
            WHEN order_date LIKE "2020-07%" THEN quantity * price
            ELSE 0
        END
    ) >= 100;
```

#### [1517. 查找拥有有效邮箱的用户](https://leetcode.cn/problems/find-users-with-valid-e-mails/)

@2023.11.16

```text
Users
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
| mail          | varchar |
+---------------+---------+

Output
+---------+-----------+-------------------------+
| user_id | name      | mail                    |
+---------+-----------+-------------------------+
```

```sql
SELECT
    *
FROM
    users
WHERE
    mail regexp '^[a-zA-Z]+[a-zA-Z0-9_\\.\\/\\-]*@leetcode\\.com$';
```

#### [1527. 患某种疾病的患者](https://leetcode.cn/problems/patients-with-a-condition/)

@2023.11.17

```text
Patients
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |
+--------------+---------+

Output
+------------+--------------+--------------+
| patient_id | patient_name | conditions   |
+------------+--------------+--------------+
```

```sql
SELECT
    *
FROM
    Patients
WHERE
    conditions LIKE 'DIAB1%'
    OR conditions LIKE '% DIAB1%';
```

#### [1543. 产品名称格式修复](https://leetcode.cn/problems/fix-product-name-format/)

@2023.11.17

```text
Sales
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| sale_id      | int     |
| product_name | varchar |
| sale_date    | date    |
+--------------+---------+

Output
+--------------+-----------+-------+
| product_name | sale_date | total |
+--------------+-----------+-------+
```

```sql
SELECT
    TRIM(LOWER(product_name)) AS product_name,
    LEFT(sale_date, 7) AS sale_date,
    COUNT(1) AS total
FROM
    Sales
GROUP BY
    1,
    2
ORDER BY
    1,
    2;
```

#### [1565. 按月统计订单数与顾客数](https://leetcode.cn/problems/unique-orders-and-customers-per-month/)

@2023.11.17

```text
Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| invoice       | int     |
+---------------+---------+

Output
+---------+-------------+----------------+
| month   | order_count | customer_count |
+---------+-------------+----------------+
```

```sql
SELECT
    LEFT(order_date, 7) AS MONTH,
    COUNT(DISTINCT order_id) AS order_count,
    COUNT(DISTINCT customer_id) AS customer_count
FROM
    Orders
WHERE
    invoice > 20
GROUP BY
    1;
```

#### [1571. 仓库经理](https://leetcode.cn/problems/warehouse-manager/)

@2023.11.17

```text
Warehouse
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| name         | varchar |
| product_id   | int     |
| units        | int     |
+--------------+---------+

Products
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| Width         | int     |
| Length        | int     |
| Height        | int     |
+---------------+---------+

Output
+----------------+------------+
| warehouse_name | volume     |
+----------------+------------+
```

```sql
SELECT
    name AS warehouse_name,
    SUM(units * width * length * height) AS volume
FROM
    Warehouse AS w
    JOIN Products AS p ON w.product_id = p.product_id
GROUP BY
    name;
```

#### [1581. 进店却未进行过交易的顾客](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/)

@2023.11.17

```text
Visits
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+

Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+

Output
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
```

```sql
SELECT
    customer_id,
    COUNT(*) AS count_no_trans
FROM
    Visits
WHERE
    visit_id NOT IN (
        SELECT
            visit_id
        FROM
            Transactions
    )
GROUP BY
    customer_id;
```

#### [1607. 没有卖出的卖家](https://leetcode.cn/problems/sellers-with-no-sales/)

@2023.11.18

```text
Customer (not used)
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
+---------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| sale_date     | date    |
| order_cost    | int     |
| customer_id   | int     |
| seller_id     | int     |
+---------------+---------+

Seller
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| seller_id     | int     |
| seller_name   | varchar |
+---------------+---------+

Output
+-------------+
| seller_name |
+-------------+
```

```sql
SELECT
    seller_name
FROM
    Seller AS s
WHERE
    seller_id NOT IN (
        SELECT
            seller_id
        FROM
            Orders
        WHERE
            YEAR (sale_date) = '2020'
    )
ORDER BY
    seller_name;
```

#### [1623. 三人国家代表队](https://leetcode.cn/problems/all-valid-triplets-that-can-represent-a-country/)

@2023.11.18

```text
SchoolA
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+

SchoolB
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+

SchoolC
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+

Output
+----------+----------+----------+
| member_A | member_B | member_C |
+----------+----------+----------+
```

```sql
SELECT
    a.student_name AS member_A,
    b.student_name AS member_B,
    c.student_name AS member_C
FROM
    SchoolA AS a,
    SchoolB AS b,
    SchoolC AS c
WHERE
    a.student_name <> b.student_name
    AND b.student_name <> c.student_name
    AND a.student_name <> c.student_name
    AND a.student_id <> b.student_id
    AND b.student_id <> c.student_id
    AND a.student_id <> c.student_id;
```

#### [1633. 各赛事的用户注册率](https://leetcode.cn/problems/percentage-of-users-attended-a-contest/)

@2023.11.18

```text
Users
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+

Register
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+

Output
+------------+------------+
| contest_id | percentage |
+------------+------------+
```

```sql
SELECT
    contest_id,
    ROUND(
        COUNT(r.user_id) * 100 / (
            SELECT
                COUNT(*)
            FROM
                Users
        ),
        2
    ) AS percentage
FROM
    Register AS r
    LEFT JOIN Users AS u USING (user_id)
GROUP BY
    contest_id
ORDER BY
    percentage DESC,
    contest_id;
```

#### [1757. 可回收且低脂的产品](https://leetcode.cn/problems/recyclable-and-low-fat-products/)

@2023.12.09

```text
Products
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+

Output
+-------------+
| product_id  |
+-------------+
```

```sql
SELECT
    product_id
FROM
    Products
WHERE
    low_fats = 'Y'
    AND recyclable = 'Y';
```

#### [1821. 寻找今年具有正收入的客户](https://leetcode.cn/problems/find-customers-with-positive-revenue-this-year/)

@2023.11.08

```text
Customers
+--------------+------+
| Column Name  | Type |
+--------------+------+
| customer_id  | int  |
| year         | int  |
| revenue      | int  |
+--------------+------+

Output
+-------------+
| customer_id |
+-------------+
```

选择 `year` 为 2021 且 `revenue > 0` 的 `customer_id`。

```sql
SELECT
    customer_id
FROM
    Customers
WHERE
    year = 2021
    AND revenue > 0;
```

#### [1873. 计算特殊奖金](https://leetcode.cn/problems/calculate-special-bonus/)

@2023.11.08

```text
Employees
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| employee_id | int     |
| name        | varchar |
| salary      | int     |
+-------------+---------+

Output
+-------------+-------+
| employee_id | bonus |
+-------------+-------+
```

`bonus` 的结构是 `IF (bool, T:_, F:_) AS bonus`，结果按 `employee_id` 排序。

```sql
SELECT
    employee_id,
    IF (
        Name NOT LIKE 'M%'
        AND (employee_id & 1) = 1,
        salary,
        0
    ) AS bonus
FROM
    Employees
ORDER BY
    employee_id;
```