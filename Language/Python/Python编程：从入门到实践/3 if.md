## 1. if

```python
runners = ['Somnia1337', 'Feinberg', 'k4yfour', 'Zylenox', 'doogile']
for runner in runners:
	if runner == 'Somnia1337':
		print(f"{runner} is me!")
	else:
		print(runner)
```

## 2. 多个条件

**关键字and**/**or**/**not**：相当于逻辑与/或/非。

```python
if runner != 'k4yfour' and runner != 'doogile':
	print(f"{runner}'s id is capped.'")
```

```python
if runner == 'Feinberg' or runner == 'Zylenox':
	print(f"{runner} is one of my favorite runners!")
```

```python
if 'PluginL' not in runners:
	runners.append('PluginL')
```

## 3. elif与else

```python
if a:
	A
elif b:
	B
else:
	X
```

## 4. 使用if处理列表

### 1) 处理空列表

在if语句中==使用列表名作为条件表达式==时，==当列表非空时返回True==。

```python
runners = ['Somnia1337', 'Feinberg', 'k4yfour', 'Zylenox', 'doogile']
if runners:
	for runner in runners:
		print(runner)
else:
	print("There are no runners yet.")
```

> 对于一切空值——数值0、空值None、单引号空字符`''`、双引号空字符`""`、空列表`[]`、空元组`()`、空字典`{}`——Python都将返回False。

### 2) 处理多个列表

可以在遍历两个列表的其中一个时，用if语句检查当前元素是否包含在另一个列表中。

```python
runners = ['Somnia1337', 'Feinberg', 'k4yfour', 'Zylenox', 'doogile']
top_runners = ['Feinberg', 'Zylenox']
for runner in runners:
	if runner in top_runners:
		print(f"{runner} is a top runner!")
	else:
		print(f"{runner} is a great runner.")
```