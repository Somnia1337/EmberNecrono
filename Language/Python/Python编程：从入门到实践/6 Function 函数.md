## 1. 定义函数

关键字def：定义函数。

```python
def greet():
	"""显示简单的问候语""" #文档注释，描述函数功能
	print("Hello!")

greet()
```

为其添加一个参数：

```python
def greet(username):
	"""显示简单的问候语"""
	print(f"Hello, {username.title()}!")

greet('somnia1337')
```

## 2. 传参

传参的方式有：位置实参、关键字实参、列表与字典。

### 1) 位置实参

即为按顺序传参。

### 2) 关键字实参

```python
def get_remainder(a, b):
	return a % b

print(get_remainder(a=3, b=2))
```

### 3) 默认参数

```python
def get_formatted_name(first_name, last_name, middle_name=""):
	if middle_name:
		full_name = f"{first_name} {middle_name} {last_name}"
	else:
		full_name = f"{first_name} {last_name}"
	return full_name.title()
```

## 3. 返回值

可以从函数返回字典：

```python
def build_person(first_name, last_name):
	person = {"first": first_name, "last": last_name}
	return person
```

## 4. 传递列表

```python
def greet_users(usernames):
	for username in usernames:
		print(f"Hello, {username}!")

usernames = ['Amy', 'Bob']
greet_users(usernames)
```

要禁止函数修改列表，可以传递其副本：

```python
_func_name_(_list_name_[:])
```

## 5. 传递任意数量的实参

单星号：指示创建元组。

```python
def print_everything(*anything):
	for thing in anything:
		print(thing)

print_everything("Amy")
print_everything("Bob", "Carl")
```

形参`*anything`指示创建一个名为`anything`的元组，包含函数接收的所有值。

双星号：指示创建字典。

```python
def build_profile(first, last, **info):
	info["first"] = first
	info["last"] = last
	return info

user = build_profile("somnia", "lu",
					 location="sichuan",
					 field="software")
print(user)
```

形参`**info`指示创建一个名为`info`的字典，包含函数接收的所有键值对。

如果除了任意数量的实参，函数还需接收特定数量的其他实参，则前者必须放在函数声明的末尾：

```python
def make_pizza(size, *toppings):
```

## 6. 将函数存储在模块中

### 1) 模块

模块：扩展名为`.py`的文件，包含要导入程序的代码。

示例：

1. 创建文件`pizza_utils.py`：

```python
# pizza_utils.py
def make_pizza(size, *toppings):
	print(f"Making a {size}-inch pizza with...")
	for topping in toppings:
		print(topping)
	print("Your pizza is ready!")
```

2. 在同一目录下创建文件`making_pizzas.py`：

```python
# making_pizzas.py
import pizza_utils

pizza_utils.make_pizza(16, "mushrooms", "green peppers")
```

语句`import pizza_utils`将前一个文件导入程序中。

如果使用星号导入模块的所有函数，调用时就无需`_module_name_.`：

```python
from pizza import *

make_pizza(16, "mushrooms", "green peppers")
```

还可以只导入特定函数，也能简化调用：

```python
from pizza_utils import make_pizza # 更多函数用逗号分隔

make_pizza(16, "mushrooms", "green peppers")
```

### 2) 别名

**关键字as**：在导入函数时为其指定别名（一般用于过长的/冲突的函数名）。

```python
from pizza_utils import make_pizza as mp

mp(16, "mushrooms", "green peppers")
```

还可以为模块指定别名：

```python
import pizza_utils as pu

pu.make_pizza(16, "mushrooms", "green peppers")
```