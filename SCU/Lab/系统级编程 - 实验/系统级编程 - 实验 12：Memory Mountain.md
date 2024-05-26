@2024.05.23 | Week 13

卢剑歌 2022141461145

> [!question] 1.1 What is the variable of `size` and `stride` used for?

- `size` 为读取的数据大小
- `stride` 为读取数据时的间距

> [!question] 1.2 What is the meaning of the return value of the function `run()`?

`run()` 返回读取数据的速率（Mb/s）



![[memory_mountain-1.png|500]]

![[memory_mountain-2.png|500]]

> [!question] 8\. How many ridges do you get? And what are they correspond to?

3 个，在 `64k`、`1024k`、`8m` 处。

> [!question] 9\. Just compare the mountain with the information that you just got by running as `everest.exe`. And explain the How the spatial locality and temporal locality are shown in this figure.

- 工作集变大 => 时间局部性（指令被重复访问的频率降低）
- `stride` 变大 => 空间局部性（相邻指令被访问的频率降低）