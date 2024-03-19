## 1. 函数input

函数input：从控制台读取用户输入，接收一个字符串参数，将其显示在屏幕上作为提示语。

```python
message = input("Type here: ")
print(f"You typed {message}")
```

函数int：将输入的字符串转换为整数。

```python
age = input("How old are you? ")
age = int(age)
print(age >= 18)
```

## 2. while

```python
cur = 1
while cur <= 5:
	print(cur)
	cur += 1
```

要循环处理列表或字典，最好使用while而非for。

```python
unconfirmed_users = ['alice', 'bob', 'cathy']
confirmed_users = []
while unconfirmed_users:
	current_user = unconfirmed_users.pop()
	confirmed_users.append(current_user)
```

删除所有指定元素：

```python
pets = ['dog', 'cat', 'dog', 'fish', 'rabbit', 'dog']
while 'dog' in pets:
	pets.remove('dog')
```