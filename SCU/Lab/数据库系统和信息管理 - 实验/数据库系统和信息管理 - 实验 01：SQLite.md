@2024.04.24 | Week 09

所下载数据库 `tennis.sqlite` 有 `Players`，`Rankings`，`Matches` 3 张表。

### 查询 `Players` 表

`Players` 表结构：

![[Snipaste_240424_200017.png]]

> 1\. 查询选手身高的最值

```sql
SELECT
    MAX(height) AS maxHeight, MIN(height) AS minHeight
FROM
    Players;
```

![[Snipaste_240424_201626.png|500]]

> 2\. 查询选手数量最多的前 `3` 个国家

```sql
SELECT
    ioc AS nationality,
    COUNT(ioc) AS playerCount
FROM
    Players
GROUP BY
    ioc
ORDER BY
    playerCount DESC
LIMIT 3;
```

![[Snipaste_240424_201846.png|600]]

> 3\. 查询右手选手与左手选手的比例，结果保留 `2` 位小数

```sql
SELECT ROUND(
        (
            SELECT
                COUNT(*)
            FROM
                Players
            WHERE
                hand = 'R'
        ) * 1.0 / (
            SELECT
                COUNT(*)
            FROM
                Players
            WHERE
                hand = 'L'
        ), 2) AS "R : L"
FROM
    Players;
```

![[Snipaste_240424_204158.png|600]]

### 查询 `Rankings` 表

`Rankings` 表结构：

![[Snipaste_240424_205310.png|500]]

> 4\. 查询每 `50` 人的积分

```sql
SELECT
    rank,
    points
FROM
    Rankings
WHERE
    rank LIKE '%01'
    OR rank = '1';
```

![[Snipaste_240424_205337.png|500]]

### 查询 `Matches` 表

`Matches` 表结构：

![[Snipaste_240424_210137.png]]

> 5\. 查询比赛次数占全部比赛次数的比例 `>= 1%` 的锦标赛名称、比赛次数以及占比，倒序排序

```sql
SELECT
    tourney_name AS tourney,
    COUNT(*) AS matchCount,
    ROUND(
        COUNT(*) * 100.0 / (
            SELECT
                COUNT(*)
            FROM
                Matches
        ),
    2) AS "ratio %"
FROM
    Matches
GROUP BY
    tourney_name
HAVING
    COUNT(*) >= (
        SELECT
            COUNT(*)
        FROM
            Matches
    ) / 100.0
ORDER BY
    COUNT(*) DESC;
```

![[Snipaste_240424_211148.png|500]]

> 6\. 查询每种场地材质下胜场数最多的选手

```sql
SELECT
    surface,
    winner_id
FROM
    Matches
GROUP BY
    surface
HAVING
    COUNT(*) >= (
        SELECT
            COUNT(*)
        FROM
            Matches
        GROUP BY
            winner_id
    );
```

![[Snipaste_240424_214600.png|600]]