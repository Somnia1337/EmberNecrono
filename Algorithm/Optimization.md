## 时间

### 结构

#### `Stack` / `Deque`

`Deque<> stk = new ArrayDeque<>()` 快于 `Stack<> stk = new Stack<>()`。

#### 重新赋值 / `clear()`

```java
Set<> set = new HashSet<>();
loop {
	set.clear();
	// ...
}
```

快于

```java
loop {
	Set<> set = new HashSet<>();
	// ...
}
```

#### 字符串连接 / `StringBuilder`

```java
StringBuilder ans = new StringBuilder();
ans.append(x)
   .append("A")
   .append(y)
   .append("B");
return ans.toString();
```

快于

```java
return x + "A" + y + "B";
```

### 方法

#### `Comparator` / Lambda

`Arrays.sort(arr, (a, b) -> a[1] - b[1])` 快于 `Arrays.sort(arr, Comparator.comparingInt(a -> a[1]))`。