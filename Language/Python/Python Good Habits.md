#### 格式化字符串

```python
s = "Hello, " + name
```

```python
s = f"Hello, {name}"
```

#### 文件处理

```python
f = open(filename, "w")
f.write("Hello!\n") # 如果抛出异常, 文件将不会关闭
f.close()
```

```python
with open(filename) as f:
	f.write("Hello!\n")

# 或者

try:
    f = open(filename, 'w')
    f.write("Hello!\n")
finally:
    f.close()
```

#### 一行流

```python
sq = []
for x in range(10):
    sq.append(x * x)
```

```python
sq = [x * x for x in range(10)]
```

#### 检查 `None`

```python
if x == None:
	# ...
```

```python
if x is None:
	# ...
```

#### 循环

一个数组：

```python
for i in range(len(arr)):
	# 使用 i, arr[i]
```

```python
for i, v in enumerate(arr):
	# 使用 i, v
```

多个数组：

```python
for i in range(len(a)):
	# 使用 i, a[i], b[i]
```

```python
for i, (v1, v2) in enumerate(zip(a, b)):
	# 使用 i, v1, v2
```

#### 字典

不必要的 `keys()`：

```python
for key in dic.keys():
	# 使用 key
```

```python
for key in dic:
	# 使用 key
```

同时使用键值对：

```python
for key in dic:
	val = dic[key]
	# 使用 key, val
```

```python
for key, val in dic.items():
	# 使用 key, val
```

#### 解构元组

```python
tup = 1, 2
# ...
x = tup[0]
y = tup[1]
```

```python
tup = 1, 2
# ...
x, y = tup
```

#### 打印日志

```python
def logging_demo():
	print("debug info")
	print("just some info")
	print("bad error")


logging_demo()
```

```python
def logging_demo():
	logging.debug("debug info")
	logging.info("just some info")
	logging.error("bad error")


level = logging.DEBUG
fmt = "[%(levelname)s] %(asctime)s - %(message)s"
logging.basicConfig(level=level, format=fmt)
logging_demo()
```

#### 使用 Pandas / Numpy

```python
x = list(range(100))
y = list(range(100))
s = [a + b for a, b in zip(x, y)]
```

```python
x = np.arange(100)
y = np.arange(100)
s = x + y
```