## 方法

### SQL

```text
Students(student_id, student_name)
Courses(course_id, course_name)
Enrollments(student_id, course_id, grade)
```

#### 除法查询

查询所有学生，他们选修了 `id` 为 `'123'` 的同学选修的所有课程：

```sql
SELECT s.student_id, s.student_name
FROM Students s
WHERE NOT EXISTS (
    SELECT e1.course_id
    FROM Enrollments e1
    WHERE e1.student_id = '123'
    EXCEPT
    SELECT e2.course_id
    FROM Enrollments e2
    WHERE e2.student_id = s.student_id
);
```

#### 聚集函数查询

查询所有学生都选择的课程的课程号：

```sql
SELECT course_id
FROM Enrollments
GROUP BY course_id
HAVING COUNT(DISTINCT student_id) = (
	SELECT COUNT(*)
	FROM Students
);
```

查询所有学生以及课程名和成绩，他们的成绩不低于该门课程所有学生的平均成绩：

```sql
SELECT s.student_id, s.student_name, c.course_name, e1.grade
FROM Students s
	JOIN Enrollments e1 ON s.student_id = e1.student_id
	JOIN Courses c ON e1.course_id = c.course_id
WHERE e1.grade >= (
    SELECT AVG(e2.grade)
    FROM Enrollments e2
    WHERE e2.course_id = e1.course_id
);
```

### 判断分解结果是否具有依赖保持性

对 $R_1$ 到 $R_n$ 中的所有 $(R_i,\space R_j)$，如果都有 $R_i \cap R_j$ 的属性集 $A$ 为 $R_i$ 或 $R_j$ 的 ==超码==，称该分解具有依赖保持性。

### 求正则覆盖

方法 1（教材）：

1. 合并右侧：`A -> B & A -> C` ==> `A -> BC`
2. 移除两侧冗余：找到任一 `a -> b`，`a` 或 `b` 包含无关属性，删去无关属性
3. 检查是否较之前有变化，若有，转到 `1.`
4. 停止，输出

方法 2（网络）：

1. 分解右侧复合：`A -> BC` ==> `A -> B, A -> C`
2. 移除左侧冗余：`AB -> C & A+ -> C` ==> `A -> C`
3. 移除冗余依赖：`A -> B & B -> C & A -> C` ==> 移除 `A -> C`

### 范式分解

#### 2NF 到 3NF

1. 找到问题依赖：`X -> A`，`X` 不是超码且 `A` 不是主属性
2. 将 `R` 分解为 `R1 = { X -> A }` 和 `R2 = R - { A }`
3. 重复 `1.` 和 `2.`，直到找不出问题依赖

#### 3NF 到 BCNF

1. 找到问题依赖：`X -> A`，`X` 不含候选码且 `A` 不属于 `X`
2. 将 `R` 分解为 `R1 = { X -> A }` 和 `R2 = R - { A }`
3. 重复 `1.` 和 `2.`，直到找不出问题依赖

## 知识

表的 `基数 = 行数`

移除视图定义：`DROP VIEW Viewname`

从 ER 模型转换为关系模型时，强实体 -> 表

DBMS 负责保证数据一致性

关键词：

|     | 创建       | 删除       |
| --- | -------- | -------- |
| 表   | `CREATE` | `DROP`   |
| 记录  | `INSERT` | `DELETE` |

创建表示例：

```sql
CREATE TABLE Product(
	ProductNo char(10),
	PName var char(20) NOT NULL,
	Price decimal(8,2),
	Producer varchar(60),
	ProduceDate date,
	PRIMARY KEY (ProductNo),
	CHECK Price > 0
);
```

---

**范式**：

- 1NF：所有属性不可分割（所有属性值都是原子性的）
- 2NF：每个非主属性都 **完全函数依赖** 于码（所有非主属性完全函数依赖于每个候选码）
- 3NF：不存在非主属性 **传递函数依赖** 于码（非主属性不传递函数依赖于候选码）
- BCNF：不存在 **任何属性** **部分函数依赖**/**传递函数依赖** 于码（任何非平凡的函数依赖 $X \rightarrow Y$ 中，$X$ 必须是超码）

**事务**：不可分割执行的一条 / 一组 SQL 语句，或者整个程序。

```sql
BEGIN TRANSACTION
	-- SQL 语句
COMMIT / ROLLBACK
```

事务的 **ACID** 特性：原子性，一致性，隔离性，持续性。

### 例题

#### 关系代数 & SQL

```text
Teacher(ID, name, dept_name)
Project(ID, name, teacher_ID, budget)
Student(ID, name, age, dept_name)
Participate(student_ID, project_ID, salary)
```

> [!question] List the `id`s and `name`s of all projects whose leader is the teacher of the "Software" department

$$\rm \large \Pi_{P.id,\space name}(\sigma_{P.teacher_id = T.id \wedge dept_name = 'Software'}(T) \times P)$$

> [!question] List the `id`s and `name`s of all projects with a budget greater than $10,000$ and with a name include "Software"

```sql
select id, name
from project
where budget > 10000
	and name like '%Software %';
```

> [!question] List `id`s and `name`s of all student of "Software" department who have NOT participated in any project

```sql
select id, name
from student
where dept_name = 'Software'
	and id not in (
		select distinct student_ID
		from participate
	);
```

> [!question] List `id`s and `name`s of all projects and the number of students who have participated in it, sort the results in descending order based on the number of students

```sql
select id, name
count(*) as stu_nums
from participate, project
where participate.project_id = project.id
group by id
order by stu_nums desc;
```

> [!question] List `id`s of projects whose students earn a higher salary on average than the average salary at project with id '001'

```sql
select project_id
from participate
group by project_id
having avg(salary) > (
	select avg(salary)
	from participate
	where project_id= '001'
);
```

> [!question] List the `name`s, sum `salary` of all students who have the most (highest) sum salary

```sql
select student.name, sum(salary)
from student, participate
where student.id = participate.student_id
group by student.id
having sum(salary) >= all (
	select sum(salary)
	from student, participate
	where student.id = participate.student_id
	group by student.id
);
```

> [!question] List the `name`s of all students who have participated in all projects led by the teacher whose name is 'Enstein'

```sql
select name
from student s
where not exists (
	select project.id
	from teacher,project
	where teacher.id = project.teacher_id
		and teacher.name = 'Enstein'
	except
	select participate.project_id
	from student, participate
	where student.id = participate.student_id
		and student.id = s.id
);
```