```text
176 178 180 184 534 550 570 574 578 580 585 602 608 612 614 626 1045 1070 1077 1098 1107 1112 1126 1132 1149 1158 1164 1174 1204 1212 1264 1270 1285 1308 1355 1364 1393 1398 1440 1445 1459 1532 1549 1555 1596 1699 1715 1747 1783 1831 1867 1875 1907 1934 1990 2020 2041 2051 2084 2228 2292
```

#### [176. 第二高的薪水](https://leetcode.cn/problems/second-highest-salary/)

@2023.11.10

```text
Employee
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+

Output
+---------------------+
| SecondHighestSalary |
+---------------------+
```

1. 排序

降序排序，选择第 2 个。

```sql
SELECT
    (
        SELECT DISTINCT
            salary
        FROM
            Employee
        ORDER BY
            salary DESC
        LIMIT
            1
        OFFSET
            1
    ) AS secondHighestSalary;
```

2. 最大值

选择小于最大值的数据中的最大者。

```sql
SELECT
    MAX(salary) AS secondHighestSalary
FROM
    Employee
WHERE
    salary < (
        SELECT
            MAX(salary)
        FROM
            Employee
    );
```

#### [178. 分数排名](https://leetcode.cn/problems/rank-scores/)

@2023.11.10

```text
Scores
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| score       | decimal |
+-------------+---------+

Output
+-------+------+
| score | rank |
+-------+------+
```

窗口函数 `DENSE_RANK()` 不会跳过排名值，例如对 `8 8 6 4 3`，`RANK()` 排名为 `1 1 3 4 5`，`DENSE_RANK()` 排名为 `1 1 2 3 4`。

```sql
SELECT
    score,
    DENSE_RANK() OVER (
        ORDER BY
            score DESC
    ) AS 'rank'
FROM
    Scores;
```

#### [180. 连续出现的数字](https://leetcode.cn/problems/consecutive-numbers/)

@2023.11.10

```text
Logs
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+

Output
+-----------------+
| ConsecutiveNums |
+-----------------+
```

选择满足 `a.id+2=b.id+1=c.id` 且 `a.num=b.num=c.num` 的 `a.num`。

```sql
SELECT DISTINCT
    a.num AS consecutiveNums
FROM
    Logs AS a,
    Logs AS b,
    Logs AS c
WHERE
    a.id + 1 = b.id
    AND b.id + 1 = c.id
    AND a.num = b.num
    AND b.num = c.num;
```

选择 `(id+1, num)` 及 `(id+2, num)` 均存在的 `num`。

```sql
SELECT DISTINCT
    num AS consecutiveNums
FROM
    Logs
WHERE
    (id + 1, num) IN (
        SELECT
            *
        FROM
            Logs
    )
    AND (id + 2, num) IN (
        SELECT
            *
        FROM
            Logs
    );
```

#### [184. 部门工资最高的员工](https://leetcode.cn/problems/department-highest-salary/)

@2023.11.10

```text
Employee
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+

Department
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+

Output
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
```

子查询按 `departmentId` 分组，选择 `salary` 最高者，父查询选择包含在子查询的结果。

```sql
SELECT
    d.name AS department,
    e.name AS employee,
    salary
FROM
    Employee AS e,
    Department AS d
WHERE
    e.departmentId = d.id
    AND (e.salary, e.departmentId) IN (
        SELECT
            MAX(salary),
            departmentId
        FROM
            Employee
        GROUP BY
            departmentId
    );
```

#### [534. 游戏玩法分析 III](https://leetcode.cn/problems/game-play-analysis-iii/)

@2023.11.11

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
+-----------+------------+---------------------+
| player_id | event_date | games_played_so_far |
+-----------+------------+---------------------+
```

自连接，用 `a.id` 与 `a.date` 分组，用 `a.id = b.id` 与 `a.date >= b.date` 限定，累加 `b.played`。

```sql
SELECT
    a.player_id,
    a.event_date,
    SUM(b.games_played) AS games_played_so_far
FROM
    Activity AS a,
    Activity AS b
WHERE
    a.player_id = b.player_id
    AND a.event_date >= b.event_date
GROUP BY
    a.player_id,
    a.event_date;
```

`SUM(played)`，`OVER()` 中以 `id` 分区，以 `date` 排序。

```sql
SELECT
    player_id,
    event_date,
    SUM(games_played) OVER (
        PARTITION BY
            player_id
        ORDER BY
            event_date
    ) AS games_played_so_far
FROM
    Activity;
```

#### [550. 游戏玩法分析 IV](https://leetcode.cn/problems/game-play-analysis-iv/)

@2023.11.11

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
+-----------+
| fraction  |
+-----------+
```

子查询按 `id` 分组，选出最小 `date` 的后 1 天，父查询的分子为子查询中不同的 `id` 计数，分母为所有不同的 `id` 计数。

```sql
SELECT
    ROUND(
        COUNT(DISTINCT player_id) / (
            SELECT
                COUNT(DISTINCT player_id)
            FROM
                Activity
        ),
        2
    ) AS fraction
FROM
    Activity
WHERE
    (player_id, event_date) IN (
        SELECT
            player_id,
            MIN(event_date) + INTERVAL 1 DAY -- 最早日期的后 1 天
        FROM
            Activity
        GROUP BY
            player_id
    );
```

#### [570. 至少有5名直接下属的经理](https://leetcode.cn/problems/managers-with-at-least-5-direct-reports/)

@2023.11.10

```text
Employee
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+

Output
+------+
| name |
+------+
```

子查询按 `managerId` 分组，选择行数至少为 5 的 `managerId`，父查询选择 `id` 在子查询内的 `name`。

```sql
SELECT
    name
FROM
    Employee
WHERE
    id IN (
        SELECT
            managerId
        FROM
            Employee
        GROUP BY
            managerId
        HAVING
            COUNT(*) >= 5
    );
```

#### [574. 当选者](https://leetcode.cn/problems/winning-candidate/)

@2023.11.10

```text
Candidate
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| name        | varchar  |
+-------------+----------+

Vote
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| candidateId | int  |
+-------------+------+

Output
+------+
| name |
+------+
```

按 `candidateId` 分组，按 `COUNT(*)` 倒序排序，选择第 1 个。

```text
SELECT
    name
FROM
    Candidate AS c
WHERE
    c.id IN (
        SELECT
            candidateId
        FROM
            Vote
        GROUP BY
            candidateId
        ORDER BY
            COUNT(*) DESC
        LIMIT
            1
    );
```

报错 `This version of MySQL doesn't yet support 'LIMIT & IN/ALL/ANY/SOME subquery'`，可用 `JOIN` 改写：

```sql
SELECT
    c.name
FROM
    Candidate AS c
    JOIN (
        SELECT
            candidateId
        FROM
            Vote
        GROUP BY
            candidateId
        ORDER BY
            COUNT(*) DESC
        LIMIT
            1
    ) AS c1 ON c.id = c1.candidateId;
```

#### [578. 查询回答率最高的问题](https://leetcode.cn/problems/get-highest-answer-rate-question/)

@2023.11.11

```text
SurveyLog
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| action      | ENUM |
| question_id | int  |
| answer_id   | int  |
| q_num       | int  |
| timestamp   | int  |
+-------------+------+

Output
+------------+
| survey_log |
+------------+
```

按 `question_id` 分组，按 `COUNT('answer') / COUNT('show')` 降序、`question_id` 升序排序，选择第 1 个。

```sql
SELECT
    question_id AS survey_log
FROM
    SurveyLog AS s
GROUP BY
    question_id
ORDER BY
    (
        SELECT
            COUNT(*)
        FROM
            SurveyLog
        WHERE
            ACTION = 'answer'
            AND question_id = s.question_id
    ) / (
        SELECT
            COUNT(*)
        FROM
            SurveyLog
        WHERE
            ACTION = 'show'
            AND question_id = s.question_id
    ) DESC,
    question_id
LIMIT
    1;
```

#### [580. 统计各专业学生人数](https://leetcode.cn/problems/count-student-number-in-departments/)

@2023.11.10

```text
Student
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| student_name | varchar |
| gender       | varchar |
| dept_id      | int     |
+--------------+---------+

Department
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| dept_id     | int     |
| dept_name   | varchar |
+-------------+---------+

Output
+-------------+----------------+
| dept_name   | student_number |
+-------------+----------------+
```

学生人数为 0 的学院也需列出，用 `Department LEFT JOIN Student`，按 `d.dept_id` 分组，按 `COUNT(s.dept_id)` 降序、`dept_name` 升序排序。

```sql
SELECT
    dept_name,
    COUNT(s.dept_id) AS student_number
FROM
    Department AS d
    LEFT JOIN Student AS s ON d.dept_id = s.dept_id
GROUP BY
    d.dept_id
ORDER BY
    COUNT(s.dept_id) DESC,
    dept_name;
```

#### [585. 2016年的投资](https://leetcode.cn/problems/investments-in-2016/)

@2023.11.11

```text
Insurance
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+

Output
+----------+
| tiv_2016 |
+----------+
```

子查询 1 选择至少另有一人与自己在 2015 年投资相同的 `pid`，子查询 2 选择至少另有一人与自己所在城市相同的 `pid`，父查询选择 `pid` 在子查询 1 中且不在子查询 2 中的人在 2016 年的总投资。

```sql
SELECT
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM
    Insurance
WHERE
    pid IN (
        SELECT
            a.pid
        FROM
            Insurance AS a,
            Insurance AS b
        WHERE
            a.pid <> b.pid
            AND a.tiv_2015 = b.tiv_2015
    )
    AND pid NOT IN (
        SELECT
            a.pid
        FROM
            Insurance AS a,
            Insurance AS b
        WHERE
            a.pid <> b.pid
            AND a.lat = b.lat
            AND a.lon = b.lon
    );
```

创建子查询 `i1`，选择 `pid, cnt_tiv_2015, cnt_city`，后两者分别为按 `tiv_2015`、按 `lat, lon` 分区时每区的行数，`i JOIN i1 ON i.pid = i1.pid`，限制 `cnt_tiv_2015 > 1` 与 `cnt_city = 1`。

```sql
SELECT
    ROUND(SUM(i.tiv_2016), 2) AS tiv_2016
FROM
    Insurance AS i
    JOIN (
        SELECT
            pid,
            COUNT(*) OVER (
                PARTITION BY
                    tiv_2015
            ) AS cnt_tiv_2015,
            COUNT(*) OVER (
                PARTITION BY
                    lat,
                    lon
            ) AS cnt_city
        FROM
            Insurance
    ) AS i1 ON i.pid = i1.pid
WHERE
    cnt_tiv_2015 > 1
    AND cnt_city = 1;
```

可以简化子查询，直接选择 `*`，再添加新的两列，作为新的表 `i1`，直接从中选择。

```sql
SELECT
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM
    (
        SELECT
            *,
            COUNT(*) OVER (
                PARTITION BY
                    tiv_2015
            ) AS cnt_tiv_2015,
            COUNT(*) OVER (
                PARTITION BY
                    lat,
                    lon
            ) AS cnt_city
        FROM
            Insurance
    ) AS i1
WHERE
    cnt_tiv_2015 > 1
    AND cnt_city = 1;
```

#### [602. 好友申请 II ：谁有最多的好友](https://leetcode.cn/problems/friend-requests-ii-who-has-the-most-friends/)

@2023.11.10

```text
RequestAccepted
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+

Output
+----+-----+
| id | num |
+----+-----+
```

给定表 `RequestAccepted`：

```text
+--------------+-------------+-------------+
| requester_id | accepter_id | accept_date |
+--------------+-------------+-------------+
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |
+--------------+-------------+-------------+
```

`UNION ALL` 子查询会将两列中的每个 `_id` 全部列举：

```sql
SELECT
	requester_id AS id
FROM
	RequestAccepted
UNION ALL
SELECT
	accepter_id
FROM
	RequestAccepted
```

```text
Subquery Output
| id |  
| -- |  
| 1  |  
| 1  |  
| 2  |  
| 3  |  
| 2  |  
| 3  |  
| 3  |  
| 4  |
```

将子查询结果按 `id` 分组，按 `COUNT(*)` 降序排序，选择第 1 个。

```sql
SELECT
    id,
    COUNT(*) AS num
FROM
    (
        SELECT
            requester_id AS id
        FROM
            RequestAccepted
        UNION ALL
        SELECT
            accepter_id
        FROM
            RequestAccepted
    ) AS t
GROUP BY
    id
ORDER BY
    num DESC
LIMIT
    1;
```

#### [608. 树节点](https://leetcode.cn/problems/tree-node/)

@2023.11.11

```text
Tree
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| p_id        | int  |
+-------------+------+

Output
+----+-------+
| id | type  |
+----+-------+
```

双重 `IF` 嵌套：

```sql
SELECT
    id,
    IF (
        p_id IS NULL,
        'Root',
        IF (
            id IN (
                SELECT
                    p_id
                FROM
                    Tree
            ),
            'Inner',
            'Leaf'
        )
    ) AS type
FROM
    Tree;
```

`CASE`：

```sql
SELECT
    id,
    CASE
        WHEN p_id IS NULL THEN 'Root'
        WHEN id IN (
            SELECT
                p_id
            FROM
                Tree
        ) THEN 'Inner'
        ELSE 'Leaf'
    END AS type
FROM
    Tree;
```

#### [612. 平面上的最近距离](https://leetcode.cn/problems/shortest-distance-in-a-plane/)

@2023.11.11

```text
Point2D
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
+-------------+------+

Output
+----------+
| shortest |
+----------+
```

自连接，限制不同的点，选择最近距离。

```sql
SELECT
    MIN(
        ROUND(
            SQRT(POWER(a.x - b.x, 2) + POWER(a.y - b.y, 2)),
            1
        )
    ) AS shortest
FROM
    Point2D AS a,
    Point2D AS b
WHERE
    (a.x, a.y) <> (b.x, b.y);
```

#### [614. 二级关注者](https://leetcode.cn/problems/second-degree-follower/)

@2023.11.10

```text
Follow
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| followee    | varchar |
| follower    | varchar |
+-------------+---------+

Output
+----------+-----+
| follower | num |
+----------+-----+
```

按 `followee` 分组，该 `followee` 需出现在 `follower` 中。

```sql
SELECT
    followee AS follower,
    COUNT(DISTINCT follower) AS num
FROM
    Follow
WHERE
    followee IN (
        SELECT DISTINCT
            follower
        FROM
            follow
    )
GROUP BY
    followee;
```

自连接，`ON f1.followee = f2.follower`，按 `f1.followee` 分组。

```sql
SELECT
    f1.followee AS follower,
    COUNT(DISTINCT f1.follower) AS num
FROM
    Follow AS f1
    JOIN Follow AS f2 ON f1.followee = f2.follower
GROUP BY
    f1.followee;
```

#### [626. 换座位](https://leetcode.cn/problems/exchange-seats/)

@2023.11.10

```text
Seat
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+

Output
+----+---------+
| id | student |
+----+---------+
```

与 1 异或，将奇变偶、偶变奇，经过 `ORDER BY` 后赋予 `RANK()`。

```text
x  (x-1)^1  RANK() OVER(ORDER BY)
1  1        2
2  0        1
3  3        4
4  2        3
5  5        5
```

```sql
SELECT
    RANK() OVER (
        ORDER BY
            (id - 1) ^ 1
    ) AS id,
    student
FROM
    seat;
```

#### [1045. 买下所有产品的客户](https://leetcode.cn/problems/customers-who-bought-all-products/)

@2023.11.10

```text
Customer
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| customer_id | int     |
| product_key | int     |
+-------------+---------+

Product
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_key | int     |
+-------------+---------+

Output
+-------------+
| customer_id |
+-------------+
```

按 `customer_id` 分组，选出其下不同 `product_key` 的数量等于 `Product` 表中总数的 `customer_id`。

```sql
SELECT
    customer_id
FROM
    Customer
GROUP BY
    customer_id
HAVING
    COUNT(DISTINCT product_key) = (
        SELECT
            COUNT(*)
        FROM
            Product
    );
```

#### [1070. 产品销售分析 III](https://leetcode.cn/problems/product-sales-analysis-iii/)

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
+------------+------------+----------+-------+
| product_id | first_year | quantity | price |
+------------+------------+----------+-------+
```

子查询按 `product_id` 分组，选出每种产品的最小年份，父查询限制 `(product_id, year)` 在子查询中。

```sql
SELECT
    product_id,
    year AS first_year,
    quantity,
    price
FROM
    Sales
WHERE
    (product_id, year) IN (
        SELECT
            product_id,
            MIN(year)
        FROM
            Sales
        GROUP BY
            product_id
    );
```

无需子查询，直接按 `product_id` 分组，限制 `year = MIN(year)`。

```sql
SELECT
    product_id,
    year AS first_year,
    quantity,
    price
FROM
    Sales
GROUP BY
    product_id
HAVING
    year = MIN(year);
```

#### [1077. 项目员工 III](https://leetcode.cn/problems/project-employees-iii/)

@2023.11.10

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
| project_id  | employee_id   |
+-------------+---------------+
```

子查询按 `project_id` 分组，选择 `MAX(experience_years)`，父查询限制 `(p.project_id, experience_years)` 在子查询中。

```sql
SELECT
    p.project_id,
    p.employee_id
FROM
    Project AS p,
    Employee AS e
WHERE
    p.employee_id = e.employee_id
    AND (p.project_id, experience_years) IN (
        SELECT
            project_id,
            MAX(experience_years)
        FROM
            Project p,
            Employee e
        WHERE
            p.employee_id = e.employee_id
        GROUP BY
            project_id
    );
```

%%TODO: 窗口函数%%

#### [1098. 小众书籍](https://leetcode.cn/problems/unpopular-books/)

@2023.11.10

```text
Books
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| book_id        | int     |
| name           | varchar |
| available_from | date    |
+----------------+---------+

Orders
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| order_id       | int     |
| book_id        | int     |
| quantity       | int     |
| dispatch_date  | date    |
+----------------+---------+

Output
+-----------+--------------------+
| book_id   | name               |
+-----------+--------------------+
```

子查询按 `book_id` 分组，选择过去一年中销量不少于 10 的 `book_id`，父查询排除子查询结果，并限制起售日期在 1 个月前。

```sql
SELECT DISTINCT
    b.book_id,
    b.name
FROM
    Books AS b
    LEFT JOIN Orders AS o ON b.book_id = o.book_id
WHERE
    b.available_from < '2019-05-23'
    AND b.book_id NOT IN (
        SELECT
            book_id
        FROM
            Orders
        WHERE
            dispatch_date > '2018-06-23'
        GROUP BY
            book_id
        HAVING
            SUM(quantity) >= 10
    );
```

#### [1107. 每日新用户统计](https://leetcode.cn/problems/new-users-daily-count/)

@2023.11.10

```text
Traffic
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| activity      | enum    |
| activity_date | date    |
+---------------+---------+

Output
+------------+-------------+
| login_date | user_count  |
+------------+-------------+
```

子查询按 `user_id` 分组，选择 90 天内最小的 `'login'` 操作日期 `login_date`，父查询按 `login_date` 分组，计算 `COUNT(user_id)`。

```sql
SELECT
    login_date,
    COUNT(user_id) AS user_count
FROM
    (
        SELECT
            MIN(activity_date) AS login_date,
            user_id
        FROM
            Traffic
        WHERE
            activity = 'login'
        GROUP BY
            user_id
        HAVING
            DATEDIFF ('2019-06-30', login_date) <= 90
    ) AS Valid
GROUP BY
    login_date;
```

不能将 `HAVING` 的内容移到 `WHERE` 中：

```text
SELECT
    login_date,
    COUNT(user_id) AS user_count
FROM
    (
        SELECT
            MIN(activity_date) AS login_date,
            user_id
        FROM
            Traffic
        WHERE
            activity = 'login'
            AND DATEDIFF ('2019-06-30', login_date) <= 90
        GROUP BY
            user_id
    ) AS Valid
GROUP BY
    login_date;
```

因为 `WHERE` 中使用了 `login_date`，而此时还未计算 `login_date`。

#### [1112. 每位学生的最高成绩](https://leetcode.cn/problems/highest-grade-for-each-student/)

@2023.11.10

```text
Enrollments
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| course_id     | int     |
| grade         | int     |
+---------------+---------+

Output
+------------+-------------------+
| student_id | course_id | grade |
+------------+-----------+-------+
```

子查询按 `student_id` 分组，选择每个学生的最大分数，父查询也按 `student_id` 分组，选择 `(id, grade)` 在子查询内的行。

```sql
SELECT
    student_id,
    MIN(course_id) AS course_id,
    grade
FROM
    Enrollments
WHERE
    (student_id, grade) IN (
        SELECT
            student_id,
            MAX(grade)
        FROM
            Enrollments
        GROUP BY
            student_id
    )
GROUP BY
    student_id
ORDER BY
    student_id;
```

#### [1126. 查询活跃业务](https://leetcode.cn/problems/active-businesses/)

@2023.11.11

```text
Events
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| business_id   | int     |
| event_type    | varchar |
| occurences    | int     | 
+---------------+---------+

Output
+-------------+
| business_id |
+-------------+
```

子查询按 `event_type` 分区，计算每种事件的平均活动次数 `oc_avg` 附加在原表最后，父查询限制 `occurences > oc_avg`，按 `business_id` 分组，选择计数多于 2 的 `bussiness_id`。

```sql
SELECT
    business_id
FROM
    (
        SELECT
            *,
            AVG(occurences) OVER (
                PARTITION BY
                    event_type
            ) AS oc_avg
        FROM
            Events
    ) AS e
WHERE
    occurences > oc_avg
GROUP BY
    business_id
HAVING
    COUNT(DISTINCT event_type) >= 2;
```

#### [1132. 报告的记录 II](https://leetcode.cn/problems/reported-posts-ii/)

@2023.11.12

```text
Actions
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| post_id       | int     |
| action_date   | date    |
| action        | enum    |
| extra         | varchar |
+---------------+---------+

Removals
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| post_id       | int     |
| remove_date   | date    | 
+---------------+---------+

Output
+-----------------------+
| average_daily_percent |
+-----------------------+
```

`Actions LEFT JOIN Removals`，按 `action_date` 分组，对 `post_id` 计数。

```sql
SELECT
    ROUND(
        SUM(rmv_cnt * 100 / rpt_cnt) / COUNT(action_date),
        2
    ) AS average_daily_percent
FROM
    (
        SELECT
            action_date,
            COUNT(DISTINCT a.post_id) AS rpt_cnt,
            COUNT(DISTINCT r.post_id) AS rmv_cnt
        FROM
            Actions AS a
            LEFT JOIN Removals AS r USING(post_id)
        WHERE
            a.extra = 'spam'
        GROUP BY
            a.action_date
    ) AS v;
```

#### [1149. 文章浏览 II](https://leetcode.cn/problems/article-views-ii/)

@2023.11.11

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

按 `(view_date, view_id)` 分组，选择不同的 `article_id` 多于 1 者。

```sql
SELECT DISTINCT
    viewer_id AS id
FROM
    Views
GROUP BY
    view_date,
    viewer_id
HAVING
    COUNT(DISTINCT article_id) > 1
ORDER BY
    id;
```

#### [1158. 市场分析 I](https://leetcode.cn/problems/market-analysis-i/)

@2023.11.11

```text
Users
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |
+----------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| buyer_id      | int     |
| seller_id     | int     |
+---------------+---------+

Items (not used)
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| item_id       | int     |
| item_brand    | varchar |
+---------------+---------+

Output
+-----------+------------+----------------+
| buyer_id  | join_date  | orders_in_2019 |
+-----------+------------+----------------+
```

`Users LEFT JOIN Orders`，限制 `YEAR (order_date) = '2019'`，按 `user_id` 分组，对 `order_id` 计数。

```sql
SELECT
    user_id AS buyer_id,
    join_date,
    COUNT(order_id) AS orders_in_2019
FROM
    Users
    LEFT JOIN Orders ON user_id = buyer_id
    AND YEAR (order_date) = '2019'
GROUP BY
    user_id;
```

#### [1164. 指定日期的产品价格](https://leetcode.cn/problems/product-price-at-a-given-date/)

@2023.11.11

```text
Products
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+

Output
+------------+-------+
| product_id | price |
+------------+-------+
```

分别选择：

- `MAX(change_date) <= '2019-08-16'` 的产品，其最新价格作为 `price`
- `MIN(change_date) > '2019-08-16'` 的产品，将 `10` 作为 `price`

最后 `UNION` 两部分选择结果。

```sql
SELECT
    product_id,
    new_price AS price
FROM
    Products
WHERE
    (product_id, change_date) IN (
        SELECT
            product_id,
            MAX(change_date)
        FROM
            Products AS p
        WHERE
            change_date <= '2019-08-16'
        GROUP BY
            p.product_id
    )
UNION
SELECT
    product_id,
    10 AS price
FROM
    Products
GROUP BY
    product_id
HAVING
    MIN(change_date) > '2019-08-16';
```

#### [1174. 即时食物配送 II](https://leetcode.cn/problems/immediate-food-delivery-ii/)

@2023.11.11

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

子查询选择每个客户的最早下单日期，父查询对下单日期与期望日期相同的订单计数，除以总订单数。

```sql
SELECT
    ROUND(
        SUM(order_date = customer_pref_delivery_date) * 100 / COUNT(*),
        2
    ) AS immediate_percentage
FROM
    Delivery
WHERE
    (customer_id, order_date) IN (
        SELECT
            customer_id,
            MIN(order_date)
        FROM
            Delivery
        GROUP BY
            customer_id
    );
```

#### [1204. 最后一个能进入巴士的人](https://leetcode.cn/problems/last-person-to-fit-in-the-bus/)

@2023.11.11

```text
Queue
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+

Output
+-------------+
| person_name |
+-------------+
```

子查询用 `SUM(weight) OVER (ORDER BY turn)` 按排队顺序累加乘客体重，父查询选择累加值不大于 `1000` 的乘客名，倒序排序选择第 1 个。

```sql
SELECT
    person_name
FROM
    (
        SELECT
            *,
            SUM(weight) OVER (
                ORDER BY
                    turn
            ) AS total
        FROM
            Queue
    ) AS t
WHERE
    t.total <= 1000
ORDER BY
    total DESC
LIMIT
    1;
```

#### [1212. 查询球队积分](https://leetcode.cn/problems/team-scores-in-football-tournament/)

@2023.11.12

```text
Teams
+---------------+----------+
| Column Name   | Type     |
+---------------+----------+
| team_id       | int      |
| team_name     | varchar  |
+---------------+----------+

Matches
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| match_id      | int     |
| host_team     | int     |
| guest_team    | int     | 
| host_goals    | int     |
| guest_goals   | int     |
+---------------+---------+

Output
+------------+--------------+---------------+
| team_id    | team_name    | num_points    |
+------------+--------------+---------------+
```

按 `team_id` 分组，在 `SUM()` 内用 `CASE` 比较两队得分，用 `IF()` 判断 `team_id` 是否需要加分。

```sql
SELECT
    team_id,
    team_name,
    SUM(
        CASE
            WHEN host_goals > guest_goals THEN IF (team_id = host_team, 3, 0)
            WHEN host_goals < guest_goals THEN IF (team_id = guest_team, 3, 0)
            ELSE IF (team_id IN (host_team, guest_team), 1, 0)
        END
    ) AS num_points
FROM
    Teams AS t,
    Matches AS m
GROUP BY
    team_id
ORDER BY
    num_points DESC,
    team_id;
```

#### [1264. 页面推荐](https://leetcode.cn/problems/page-recommendations/)

@2023.11.11

```text
Friendship
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user1_id      | int     |
| user2_id      | int     |
+---------------+---------+

Likes
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| page_id     | int     |
+-------------+---------+

Output
+------------------+
| recommended_page |
+------------------+
```

1. 硬干

选择 `page_id`，其对应的 `user_id` 不为 `1` 且为 `1` 的朋友，且 `page_id` 不是 `1` 已经喜欢的。

```sql
SELECT DISTINCT
    page_id AS recommended_page
FROM
    Likes
WHERE
    user_id <> 1
    AND (
        user_id IN (
            SELECT
                user1_id
            FROM
                Friendship
            WHERE
                user2_id = 1
        )
        OR user_id IN (
            SELECT
                user2_id
            FROM
                Friendship
            WHERE
                user1_id = 1
        )
    )
    AND page_id NOT IN (
        SELECT
            page_id
        FROM
            Likes
        WHERE
            user_id = 1
    );
```

2. `UNION`

判断好友关系的部分可分别选择另一侧为 `1` 的 `user1_id`、`user2_id`，用 `UNION` 整合。

```sql
SELECT DISTINCT
    page_id AS recommended_page
FROM
    Likes
WHERE
    user_id IN (
        SELECT
            user1_id AS user_id
        FROM
            Friendship
        WHERE
            user2_id = 1
        UNION
        SELECT
            user2_id AS user_id
        FROM
            Friendship
        WHERE
            user1_id = 1
    )
    AND page_id NOT IN (
        SELECT
            page_id
        FROM
            Likes
        WHERE
            user_id = 1
    );
```

3. `CASE`

判断好友关系的部分可用 `CASE` 从 `Friendship` 中选择另一侧为 `1` 的 `id`。

```sql
SELECT DISTINCT
    page_id AS recommended_page
FROM
    Likes
WHERE
    user_id IN (
        SELECT
            (
                CASE
                    WHEN user1_id = 1 THEN user2_id
                    WHEN user2_id = 1 THEN user1_id
                END
            ) AS user_id
        FROM
            Friendship
        WHERE
            user1_id = 1
            OR user2_id = 1
    )
    AND page_id NOT IN (
        SELECT
            page_id
        FROM
            Likes
        WHERE
            user_id = 1
    );
```

#### [1270. 向公司 CEO 汇报工作的所有人](https://leetcode.cn/problems/all-people-report-to-the-given-manager/)

@2023.11.11

```text
Employees
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| employee_name | varchar |
| manager_id    | int     |
+---------------+---------+

Output
+-------------+
| employee_id |
+-------------+
```

直接讨论 3 层关系。

```sql
SELECT
    employee_id
FROM
    Employees
WHERE
    employee_id <> 1
    AND (
        manager_id = 1 -- 第 1 类
        OR manager_id IN ( -- 第 2 类
            SELECT
                employee_id
            FROM
                Employees
            WHERE
                employee_id <> 1
                AND manager_id = 1
        )
        OR manager_id IN ( -- 第 3 类
            SELECT
                employee_id
            FROM
                Employees
            WHERE
                employee_id <> 1
                AND manager_id IN (
                    SELECT
                        employee_id
                    FROM
                        Employees
                    WHERE
                        employee_id <> 1
                        AND manager_id = 1
                )
        )
    );
```

优化：最内层选择经理的直接下属，外层选择他们的直接下属，如此嵌套 3 层。

```sql
SELECT
    employee_id
FROM
    Employees
WHERE
    manager_id IN ( -- 第 3 层
        SELECT
            employee_id
        FROM
            Employees
        WHERE
            manager_id IN ( -- 第 2 层
                SELECT
                    employee_id
                FROM
                    Employees
                WHERE
                    manager_id = 1 -- 第 1 层
            )
    )
    AND employee_id <> 1;
```

3 表连接。

```sql
SELECT DISTINCT
    e1.employee_id
FROM
    Employees e1,
    Employees e2,
    Employees e3
WHERE
    e1.manager_id = e2.employee_id
    AND e2.manager_id = e3.employee_id
    AND e3.manager_id = 1
    AND e1.employee_id <> 1;
```

%%TODO%%

#### [1285. 找到连续区间的开始和结束数字](https://leetcode.cn/problems/find-the-start-and-end-number-of-continuous-ranges/)

@2023.11.12

```text
Logs
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| log_id        | int     |
+---------------+---------+

Output
+------------+--------------+
| start_id   | end_id       |
+------------+--------------+
```

```sql
SELECT
    MIN(log_id) AS start_id,
    MAX(log_id) AS end_id
FROM
    (
        SELECT
            log_id,
            log_id - ROW_NUMBER() OVER (
                ORDER BY
                    log_id
            ) AS num
        FROM
            Logs
    ) AS g
GROUP BY
    num;
```

#### [1308. 不同性别每日分数总计](https://leetcode.cn/problems/running-total-for-different-genders/)

@2023.11.11

```text
Scores
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| player_name   | varchar |
| gender        | varchar |
| day           | date    |
| score_points  | int     |
+---------------+---------+

Output
+--------+------------+-------+
| gender | day        | total |
+--------+------------+-------+
```

```sql
SELECT
    a.gender,
    a.day,
    SUM(b.score_points) AS total
FROM
    Scores AS a
    JOIN Scores AS b ON a.gender = b.gender
    AND a.day >= b.day
GROUP BY
    a.gender,
    a.day
ORDER BY
    a.gender,
    a.day;
```

#### [1355. 活动参与者](https://leetcode.cn/problems/activity-participants/)

@2023.11.11

```text
Friends
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
| activity      | varchar |
+---------------+---------+

Activities (not used)
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+

Output
+--------------+
| activity     |
+--------------+
| Singing      |
+--------------+
```

```sql
SELECT
    activity
FROM
    Friends
GROUP BY
    activity
HAVING
    COUNT(*) > SOME (
        SELECT
            COUNT(*)
        FROM
            Friends
        GROUP BY
            activity
    )
    AND COUNT(*) < SOME (
        SELECT
            COUNT(*)
        FROM
            Friends
        GROUP BY
            activity
    );
```

#### [1364. 顾客的可信联系人数量](https://leetcode.cn/problems/number-of-trusted-contacts-of-a-customer/)

@2023.11.12

```text
Customers
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| customer_name | varchar |
| email         | varchar |
+---------------+---------+

Contacts
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | id      |
| contact_name  | varchar |
| contact_email | varchar |
+---------------+---------+

Invoices
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| invoice_id   | int     |
| price        | int     |
| user_id      | int     |
+--------------+---------+

Output
+----------+-------------+-----+------------+--------------------+
|invoice_id|customer_name|price|contacts_cnt|trusted_contacts_cnt|
+----------+-------------+-----+------------+--------------------+
```

```sql
SELECT
    invoice_id,
    customer_name,
    price,
    COUNT(t.user_id) AS contacts_cnt,
    COUNT(
        CASE
            WHEN t.contact_name IN (
                SELECT
                    customer_name
                FROM
                    Customers
            ) THEN 1
            ELSE NULL
        END
    ) AS trusted_contacts_cnt
FROM
    Invoices AS i
    JOIN Customers AS c ON c.customer_id = i.user_id
    LEFT JOIN Contacts AS t ON t.user_id = i.user_id
GROUP BY
    invoice_id
ORDER BY
    invoice_id;
```

#### [1393. 股票的资本损益](https://leetcode.cn/problems/capital-gainloss/)

@2023.11.11

```text
Stocks
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| stock_name    | varchar |
| operation     | enum    |
| operation_day | int     |
| price         | int     |
+---------------+---------+

Output
+---------------+-------------------+
| stock_name    | capital_gain_loss |
+---------------+-------------------+
```

```sql
SELECT DISTINCT
    buy.stock_name,
    (sell.total - buy.total) AS capital_gain_loss
FROM
    (
        SELECT
            stock_name,
            SUM(price) AS total
        FROM
            Stocks
        WHERE
            operation = 'Buy'
        GROUP BY
            stock_name
    ) AS buy
    JOIN (
        SELECT
            stock_name,
            SUM(price) AS total
        FROM
            Stocks
        WHERE
            operation = 'Sell'
        GROUP BY
            stock_name
    ) AS sell ON buy.stock_name = sell.stock_name;
```

```sql
SELECT
    stock_name,
    SUM(IF (operation = 'Sell', price, - price)) AS capital_gain_loss
FROM
    Stocks
GROUP BY
    stock_name;
```

#### [1398. 购买了产品 A 和产品 B 却没有购买产品 C 的顾客](https://leetcode.cn/problems/customers-who-bought-products-a-and-b-but-not-c/)

@2023.11.11

```text
Customers
+---------------------+---------+
| Column Name         | Type    |
+---------------------+---------+
| customer_id         | int     |
| customer_name       | varchar |
+---------------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| customer_id   | int     |
| product_name  | varchar |
+---------------+---------+

Output
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
```

```sql
SELECT
    c.customer_id,
    customer_name
FROM
    Customers AS c
    JOIN Orders AS o ON c.customer_id = o.customer_id
GROUP BY
    c.customer_id
HAVING
    SUM(product_name = 'A')
    AND SUM(product_name = 'B')
    AND NOT SUM(product_name = 'C');
```

#### [1440. 计算布尔表达式的值](https://leetcode.cn/problems/evaluate-boolean-expression/)

@2023.11.12

```text
Variables
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| name          | varchar |
| value         | int     |
+---------------+---------+

Expressions
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| left_operand  | varchar |
| operator      | enum    |
| right_operand | varchar |
+---------------+---------+

Output
+--------------+----------+---------------+-------+
| left_operand | operator | right_operand | value |
+--------------+----------+---------------+-------+
```

```sql
SELECT
    left_operand,
    operator,
    right_operand,
    CASE
        WHEN operator = '='
        AND v1.value = v2.value THEN 'true'
        WHEN operator = '>'
        AND v1.value > v2.value THEN 'true'
        WHEN operator = '<'
        AND v1.value < v2.value THEN 'true'
        ELSE 'false'
    END AS 'value'
FROM
    Expressions AS e
    JOIN Variables AS v1 ON e.left_operand = v1.name
    JOIN Variables AS v2 ON e.right_operand = v2.name;
```

#### [1445. 苹果和桔子](https://leetcode.cn/problems/apples-oranges/)

@2023.11.11

```text
Sales
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| sale_date     | date    |
| fruit         | enum    | 
| sold_num      | int     | 
+---------------+---------+

Output
+------------+--------------+
| sale_date  | diff         |
+------------+--------------+
```

```sql
SELECT
    sale_date,
    SUM(IF (fruit = 'apples', sold_num, - sold_num)) AS diff
FROM
    Sales
GROUP BY
    sale_date;
```

#### [1459. 矩形面积](https://leetcode.cn/problems/rectangles-area/)

@2023.11.12

```text
Points
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| x_value       | int     |
| y_value       | int     |
+---------------+---------+

Output
+----------+-------------+-------------+
| p1       | p2          | area        |
+----------+-------------+-------------+
```

```sql
SELECT
    pt1.id AS p1,
    pt2.id AS p2,
    (
        ABS(pt1.x_value - pt2.x_value) * ABS(pt1.y_value - pt2.y_value)
    ) AS area
FROM
    Points AS pt1
    JOIN Points AS pt2
WHERE
    pt1.id < pt2.id
    AND pt1.x_value <> pt2.x_value
    AND pt1.y_value <> pt2.y_value
ORDER BY
    area desc,
    p1,
    p2;
```

#### [1532. 最近的三笔订单](https://leetcode.cn/problems/the-most-recent-three-orders/)

@2023.11.12

```text
Customers
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| cost          | int     |
+---------------+---------+

Output
+---------------+-------------+----------+------------+
| customer_name | customer_id | order_id | order_date |
+---------------+-------------+----------+------------+
```

```sql
SELECT
    name AS customer_name,
    c.customer_id,
    order_id,
    o.order_date
FROM
    Customers AS c
    JOIN Orders AS o ON c.customer_id = o.customer_id
    JOIN (
        SELECT
            customer_id,
            order_date,
            ROW_NUMBER() OVER (
                PARTITION BY
                    customer_id
                ORDER BY
                    order_date DESC
            ) AS row_num
        FROM
            Orders
    ) AS rk ON o.customer_id = rk.customer_id
    AND o.order_date = rk.order_date
    AND rk.row_num <= 3
ORDER BY
    name,
    o.customer_id,
    o.order_date DESC;
```

#### [1549. 每件商品的最新订单](https://leetcode.cn/problems/the-most-recent-orders-for-each-product/)

@2023.11.12

```text
Customers (not used)
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| product_id    | int     |
+---------------+---------+

Products
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| price         | int     |
+---------------+---------+

Output
+--------------+------------+----------+------------+
| product_name | product_id | order_id | order_date |
+--------------+------------+----------+------------+
```

```sql
SELECT
    product_name,
    o.product_id,
    order_id,
    order_date
FROM
    Orders AS o,
    Products AS p
WHERE
    o.product_id = p.product_id
    AND (order_date, o.product_id) IN (
        SELECT
            MAX(order_date),
            product_id
        FROM
            Orders
        GROUP BY
            product_id
    )
ORDER BY
    1,
    2,
    3;
```

```sql
SELECT
    product_name,
    o.product_id,
    order_id,
    order_date
FROM
    Orders AS o
    LEFT JOIN Products AS p ON p.product_id = o.product_id
WHERE
    (o.product_id, order_date) IN (
        SELECT
            product_id,
            MAX(order_date)
        FROM
            Orders
        GROUP BY
            product_id
    )
ORDER BY
    1,
    2,
    3;
```

#### [1555. 银行账户概要](https://leetcode.cn/problems/bank-account-summary/)

@2023.11.12

```text
Users
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| user_id      | int     |
| user_name    | varchar |
| credit       | int     |
+--------------+---------+

Transactions
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| trans_id      | int     |
| paid_by       | int     |
| paid_to       | int     |
| amount        | int     |
| transacted_on | date    |
+---------------+---------+

Output
+---------+-----------+--------+-----------------------+
| user_id | user_name | credit | credit_limit_breached |
+---------+-----------+--------+-----------------------+
```

```sql
SELECT DISTINCT
    u.user_id,
    u.user_name,
    (
        u.credit + IFNULL (i_n.m_in, 0) + IFNULL (ou_t.m_out, 0)
    ) AS credit,
    IF (
        (
            u.credit + IFNULL (i_n.m_in, 0) + IFNULL (ou_t.m_out, 0)
        ) >= 0,
        'No',
        'Yes'
    ) AS credit_limit_breached
FROM
    Users AS u
    LEFT JOIN (
        SELECT
            paid_to,
            SUM(amount) AS m_in
        FROM
            Transactions
        GROUP BY
            paid_to
    ) AS i_n ON u.user_id = i_n.paid_to
    LEFT JOIN (
        SELECT
            paid_by,
            SUM(- amount) AS m_out
        FROM
            Transactions
        GROUP BY
            paid_by
    ) AS ou_t ON u.user_id = ou_t.paid_by;
```

```sql
SELECT
    a.user_id,
    user_name,
    SUM(a.credit) AS credit,
    (
        CASE
            WHEN SUM(a.credit) < 0 THEN 'Yes'
            ELSE 'No'
        END
    ) AS credit_limit_breached
FROM
    (
        SELECT
            paid_by AS user_id,
            - amount AS credit
        FROM
            transactions
        UNION ALL
        SELECT
            paid_to AS user_id,
            amount
        FROM
            transactions
        UNION ALL
        SELECT
            user_id,
            credit
        FROM
            users
    ) AS a
    JOIN users AS b ON a.user_id = b.user_id
GROUP BY
    a.user_id;
```

#### [1596. 每位顾客最经常订购的商品](https://leetcode.cn/problems/the-most-frequently-ordered-products-for-each-customer/)

@2023.11.12

```text
Customers (not used)
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
+---------------+---------+

Orders
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| order_id      | int     |
| order_date    | date    |
| customer_id   | int     |
| product_id    | int     |
+---------------+---------+

Products
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| product_name  | varchar |
| price         | int     |
+---------------+---------+

Output
+-------------+------------+--------------+
| customer_id | product_id | product_name |
+-------------+------------+--------------+
```

```sql
SELECT
    o.customer_id,
    o.product_id,
    p.product_name
FROM
    Orders AS o,
    Products AS p
WHERE
    o.product_id = p.product_id
GROUP BY
    o.customer_id,
    o.product_id
HAVING
    COUNT(*) >= ALL (
        SELECT
            COUNT(product_id)
        FROM
            Orders AS o1
        WHERE
            o1.customer_id = o.customer_id
        GROUP BY
            o1.product_id
    );
```

#### [1699. 两人之间的通话次数](https://leetcode.cn/problems/number-of-calls-between-two-persons/)

@2023.11.12

```text
Calls
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| from_id     | int     |
| to_id       | int     |
| duration    | int     |
+-------------+---------+

Output
+---------+---------+------------+----------------+
| person1 | person2 | call_count | total_duration |
+---------+---------+------------+----------------+
```

```sql
SELECT
    from_id AS person1,
    to_id AS person2,
    COUNT(*) AS call_count,
    SUM(duration) AS total_duration
FROM
    (
        SELECT
            IF (from_id < to_id, from_id, to_id) AS from_id,
            IF (from_id < to_id, to_id, from_id) AS to_id,
            duration
        FROM
            Calls
    ) AS Sorted
GROUP BY
    from_id,
    to_id;
```

```sql
SELECT
    IF (from_id < to_id, from_id, to_id) AS person1,
    IF (from_id > to_id, from_id, to_id) AS person2,
    COUNT(*) AS call_count,
    SUM(duration) AS total_duration
FROM
    Calls
GROUP BY
    person1,
    person2;
```

#### [1715. 苹果和橘子的个数](https://leetcode.cn/problems/count-apples-and-oranges/)

@2023.11.12

```text
Boxes
+--------------+------+
| Column Name  | Type |
+--------------+------+
| box_id       | int  |
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+

Chests
+--------------+------+
| Column Name  | Type |
+--------------+------+
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+

Output
+-------------+--------------+
| apple_count | orange_count |
+-------------+--------------+
```

```sql
SELECT
    SUM(a_count) AS apple_count,
    SUM(o_count) AS orange_count
FROM
    (
        SELECT
            SUM(
                b.apple_count + CASE
                    WHEN b.chest_id IS NULL THEN 0
                    ELSE (
                        SELECT
                            apple_count
                        FROM
                            Chests AS c
                        WHERE
                            c.chest_id = b.chest_id
                    )
                END
            ) AS a_count
        FROM
            Boxes AS b
    ) AS Apples,
    (
        SELECT
            SUM(
                b.orange_count + CASE
                    WHEN b.chest_id IS NULL THEN 0
                    ELSE (
                        SELECT
                            orange_count
                        FROM
                            Chests AS c
                        WHERE
                            c.chest_id = b.chest_id
                    )
                END
            ) AS o_count
        FROM
            Boxes AS b
    ) AS Oranges;
```

```sql
SELECT
    SUM(b.apple_count) + SUM(IFNULL (c.apple_count, 0)) AS apple_count,
    SUM(b.orange_count) + SUM(IFNULL (c.orange_count, 0)) AS orange_count
FROM
    Boxes AS b
    LEFT JOIN Chests AS c ON c.chest_id = b.chest_id;
```

#### [1747. 应该被禁止的 Leetflex 账户](https://leetcode.cn/problems/leetflex-banned-accounts/)

@2023.11.12

```text
LogInfo
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| account_id  | int      |
| ip_address  | int      |
| login       | datetime |
| logout      | datetime |
+-------------+----------+

Output
+------------+
| account_id |
+------------+
```

```sql
SELECT DISTINCT
    a.account_id AS account_id
FROM
    LogInfo a,
    LogInfo b
WHERE
    a.account_id = b.account_id
    AND a.ip_address <> b.ip_address
    AND a.logout <= b.logout
    AND b.login <= a.logout;
```

#### [1783. 大满贯数量](https://leetcode.cn/problems/grand-slam-titles/)

@2023.11.12

```text
Players
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| player_id      | int     |
| player_name    | varchar |
+----------------+---------+

Championships
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| year          | int     |
| Wimbledon     | int     |
| Fr_open       | int     |
| US_open       | int     |
| Au_open       | int     |
+---------------+---------+

Output
+-----------+-------------+-------------------+
| player_id | player_name | grand_slams_count |
+-----------+-------------+-------------------+
```

```sql
SELECT
    p.player_id,
    p.player_name,
    SUM(player_id = wimbledon) + SUM(player_id = fr_open) + SUM(player_id = us_open) + SUM(player_id = au_open) AS grand_slams_count
FROM
    Players AS p
    JOIN Championships AS c
GROUP BY
    p.player_id,
    p.player_name
HAVING
    grand_slams_count > 0;
```

#### [1831. 每天的最大交易](https://leetcode.cn/problems/maximum-transaction-each-day/)

@2023.11.12

```text
Transactions
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| transaction_id | int      |
| day            | datetime |
| amount         | int      |
+----------------+----------+

Output
+----------------+
| transaction_id |
+----------------+
```

```sql
SELECT
    a.transaction_id
FROM
    Transactions AS a
    LEFT JOIN Transactions AS b ON DATE (a.day) = DATE (b.day)
    AND b.amount > a.amount
GROUP BY
    a.transaction_id
HAVING
    COUNT(b.transaction_id) = 0
ORDER BY
    1;
```

#### [1867. 最大数量高于平均水平的订单](https://leetcode.cn/problems/orders-with-maximum-quantity-above-average/)

@2023.11.13

```text
OrdersDetails
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| product_id  | int  |
| quantity    | int  |
+-------------+------+

Output
+----------+
| order_id |
+----------+
```

```sql
SELECT
    order_id
FROM
    OrdersDetails
GROUP BY
    order_id
HAVING
    MAX(quantity) > ALL (
        SELECT
            AVG(quantity)
        FROM
            OrdersDetails
        GROUP BY
            order_id
    );
```

#### [1875. 将工资相同的雇员分组](https://leetcode.cn/problems/group-employees-of-the-same-salary/)

@2023.11.13

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
+-------------+---------+--------+---------+
| employee_id | name    | salary | team_id |
+-------------+---------+--------+---------+
```

```sql
SELECT
    e.*,
    DENSE_RANK() OVER (
        ORDER BY
            t.salary
    ) AS team_id
FROM
    Employees AS e
    JOIN (
        SELECT
            employee_id,
            salary,
            COUNT(*) OVER (
                PARTITION BY
                    salary
            ) AS cnt
        FROM
            Employees
    ) AS t ON e.employee_id = t.employee_id
WHERE
    cnt > 1
ORDER BY
    4,
    1;
```

```sql
SELECT
    e.*,
    DENSE_RANK() OVER (
        ORDER BY
            salary
    ) AS team_id
FROM
    Employees AS e
WHERE
    salary IN (
        SELECT
            salary
        FROM
            Employees
        GROUP BY
            salary
        HAVING
            COUNT(*) > 1
    )
ORDER BY
    4,
    1;
```

#### [1907. 按分类统计薪水](https://leetcode.cn/problems/count-salary-categories/)

@2023.11.13

```text
Accounts
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+

Output
+----------------+----------------+
| category       | accounts_count |
+----------------+----------------+
```

```sql
SELECT
    'Low Salary' AS category,
    COUNT(*) AS accounts_count
FROM
    Accounts
WHERE
    income < 20000
UNION ALL
SELECT
    'Average Salary' AS category,
    COUNT(*) AS accounts_count
FROM
    Accounts
WHERE
    income BETWEEN 20000 AND 50000
UNION ALL
SELECT
    'High Salary' AS category,
    COUNT(*) AS accounts_count
FROM
    Accounts
WHERE
    income > 50000;
```

#### [1934. 确认率](https://leetcode.cn/problems/confirmation-rate/)

@2023.11.13

```text
Signups
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+

Confirmations
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+

Output
+---------+-------------------+
| user_id | confirmation_rate |
+---------+-------------------+
```

```sql
SELECT
    s.user_id,
    ROUND(AVG(IF (action = 'confirmed', 1, 0)), 2) AS confirmation_rate
FROM
    Signups s
    LEFT JOIN Confirmations c ON s.user_id = c.user_id
GROUP BY
    1;
```

#### [1990. 统计实验的数量](https://leetcode.cn/problems/count-the-number-of-experiments/)

@2023.11.17

```text
Experiments
+-----------------+------+
| Column Name     | Type |
+-----------------+------+
| experiment_id   | int  |
| platform        | enum |
| experiment_name | enum |
+-----------------+------+

Output
+----------+-----------------+-----------------+
| platform | experiment_name | num_experiments |
+----------+-----------------+-----------------+
```

```sql
SELECT
    platform,
    experiment_name,
    COUNT(experiment_id) AS num_experiments
FROM
    (
        SELECT
            'Android' AS platform
        UNION
        SELECT
            'IOS'
        UNION
        SELECT
            'Web'
    ) AS t1
    JOIN (
        SELECT
            'Reading' AS experiment_name
        UNION
        SELECT
            'Sports'
        UNION
        SELECT
            'Programming'
    ) AS t2
    LEFT JOIN Experiments USING (platform, experiment_name)
GROUP BY
    1,
    2
ORDER BY
    1,
    2;
```

#### [2020. 无流量的帐户数](https://leetcode.cn/problems/number-of-accounts-that-did-not-stream/)

@2023.11.13

```text
Subscriptions
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| start_date  | date |
| end_date    | date |
+-------------+------+

Streams
+-------------+------+
| Column Name | Type |
+-------------+------+
| session_id  | int  |
| account_id  | int  |
| stream_date | date |
+-------------+------+

Output
+----------------+
| accounts_count |
+----------------+
```

```sql
SELECT
    COUNT(DISTINCT sub.account_id) AS accounts_count
FROM
    Subscriptions AS sub
    LEFT JOIN Streams AS str ON sub.account_id = str.account_id
WHERE
    (
        YEAR (end_date) = 2021
        OR YEAR (start_date) = 2021
    )
    AND YEAR (stream_date) != 2021;
```

#### [2041. 面试中被录取的候选人](https://leetcode.cn/problems/accepted-candidates-from-the-interviews/)

@2023.11.17

```text
Candidates
+--------------+----------+
| Column Name  | Type     |
+--------------+----------+
| candidate_id | int      |
| name         | varchar  |
| years_of_exp | int      |
| interview_id | int      |
+--------------+----------+

Rounds
+--------------+------+
| Column Name  | Type |
+--------------+------+
| interview_id | int  |
| round_id     | int  |
| score        | int  |
+--------------+------+

Output
+--------------+
| candidate_id |
+--------------+
```

```sql
SELECT
    candidate_id
FROM
    Candidates AS c
    JOIN Rounds AS r USING (interview_id)
WHERE
    years_of_exp >= 2
GROUP BY
    candidate_id
HAVING
    SUM(score) > 15;
```

#### [2051. 商店中每个成员的级别](https://leetcode.cn/problems/the-category-of-each-member-in-the-store/)

@2023.11.17

```text
Members
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| member_id   | int     |
| name        | varchar |
+-------------+---------+

Visits
+-------------+------+
| Column Name | Type |
+-------------+------+
| visit_id    | int  |
| member_id   | int  |
| visit_date  | date |
+-------------+------+

Purchases
+----------------+------+
| Column Name    | Type |
+----------------+------+
| visit_id       | int  |
| charged_amount | int  |
+----------------+------+

Output
+-----------+---------+----------+
| member_id | name    | category |
+-----------+---------+----------+
```

```sql
SELECT
    member_id,
    name,
    (
        CASE
            WHEN rate >= 80 THEN 'Diamond'
            WHEN rate >= 50 THEN 'Gold'
            WHEN vis > 0 THEN 'Silver'
            ELSE 'Bronze'
        END
    ) AS category
FROM
    (
        SELECT
            m.member_id,
            name,
            IFNULL (COUNT(v.visit_id), 0) AS vis,
            (100 * IFNULL (COUNT(p.visit_id), 0)) / IFNULL (COUNT(v.visit_id), 0) AS rate
        FROM
            Members AS m
            LEFT JOIN Visits AS v USING (member_id)
            LEFT JOIN Purchases AS p USING (visit_id)
        GROUP BY
            m.member_id
    ) AS t;
```

#### [2084. 为订单类型为 0 的客户删除类型为 1 的订单](https://leetcode.cn/problems/drop-type-1-orders-for-customers-with-type-0-orders/)

@2023.11.19

```text
Orders
+-------------+------+
| Column Name | Type |
+-------------+------+
| order_id    | int  |
| customer_id | int  |
| order_type  | int  |
+-------------+------+

Output
+----------+-------------+------------+
| order_id | customer_id | order_type |
+----------+-------------+------------+
```

```sql
SELECT
    *
FROM
    Orders
WHERE
    (customer_id, order_type) NOT IN (
        SELECT DISTINCT
            customer_id,
            '1'
        FROM
            Orders
        WHERE
            order_type = 0
    );
```

#### [2228. 7 天内两次购买的用户](https://leetcode.cn/problems/users-with-two-purchases-within-seven-days/)

@2023.11.13

```text
Purchases
+---------------+------+
| Column Name   | Type |
+---------------+------+
| purchase_id   | int  |
| user_id       | int  |
| purchase_date | date |
+---------------+------+

Output
+---------+
| user_id |
+---------+
```

```sql
SELECT DISTINCT
    a.user_id
FROM
    Purchases AS a,
    Purchases AS b
WHERE
    a.purchase_id <> b.purchase_id
    AND a.user_id = b.user_id
    AND ABS(DATEDIFF (a.purchase_date, b.purchase_date)) <= 7
ORDER BY
    1;
```

#### [2292. 连续两年有 3 个及以上订单的产品](https://leetcode.cn/problems/products-with-three-or-more-orders-in-two-consecutive-years/)

@2023.11.18

```text
Orders
+---------------+------+
| Column Name   | Type |
+---------------+------+
| order_id      | int  |
| product_id    | int  |
| quantity      | int  |
| purchase_date | date |
+---------------+------+

Output
+------------+
| product_id |
+------------+
```

```sql
SELECT DISTINCT
    t1.product_id
FROM
    Orders t1
    LEFT JOIN Orders t2 ON YEAR (t1.purchase_date) = YEAR (t2.purchase_date) - 1
    AND t1.product_id = t2.product_id
WHERE
    t2.order_id IS NOT NULL
GROUP BY
    t1.product_id,
    YEAR (t1.purchase_date)
HAVING
    COUNT(DISTINCT t1.order_id) >= 3
    AND COUNT(DISTINCT t2.order_id) >= 3;
```