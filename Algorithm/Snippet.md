### 计数

#### `Map` 统计 `int[]`

```java
for (int i = 0; i < nums.length; i++) {
	pos.computeIfAbsent(nums[i], e -> new ArrayList<>()).add(i);
}
```

#### `int[]` 统计 `String`

```java
int[] count = new int[26];
for (char c : s.toCharArray()) count[c - 'a']++;
```

### 字符串

#### 去除字符串的前导"0"

```java
/*子串*/
while (s.length() > 1 && s.startsWith("0")) s = s.substring(1);
```

```java
/*正则表达式*/
s = s.replaceFirst("^0+", "");
```

取子串的方法为线性，效率更高；使用正则表达式的方法可读性更好。

#### 以空格为分隔符拆分字符串

```java
/*正则表达式*/
String[] words = s.split("\\s+");
```

只有用正则表达式才能处理连续空格的情况，如果使用`" "`作为分隔符：

```java
String[] words = s.split(" ");
```

则会在连续空格之间拆分出多余的空串`""`。

```java
/*示例程序*/
class Main {
    public static void main(String[] args) {
        String s = " a  b   c    d     e      ";
		System.out.println(Arrays.toString(s.split("\\s+")));
        System.out.println(Arrays.toString(s.split(" ")));
    }
}
```

输出：

```text
[, a, b, c, d, e]
[, a, , b, , , c, , , , d, , , , , e]
```

注意到使用正则表达式的语句也拆分出了空串，可以先用**方法trim**去除`s`首尾的空格再拆分。

```java
/*示例程序*/
class Main {
    public static void main(String[] args) {
        String s = " a  b   c    d     e      ";
        s = s.trim(); //去除s首尾的空格
        System.out.println(Arrays.toString(s.split("\\s+")));
    }
}
```

输出：

```text
[a, b, c, d, e]
```

#### 以指定的 `char` 分割字符串

```java
char c = (char) ('a' + i);
...
String[] parts = s.split("\\Q" + c + "\\E");
// 或者
String[] parts = s.split(String.valueOf(c));
```

### 数组

#### 数组覆盖

```java
int[] nums = ...;
int[] ans = new int[len];
...
System.arraycopy(nums, 0, ans, 0, len); //用nums覆盖ans
```

#### 列表转数组

```java
// List<String>
List<String> list1 = new ArrayList<>();
...
String[] strs = list1.toArray(new String[0]);

// List<int[]>
List<int[]> list2 = new ArrayList<>();
...
int[][] nums = list2.toArray(new int[list2.size()][2]);
```

### 自定义排序

### 位运算

#### 位运算大小写转换

- 大小写互换：`c ^= 32`。
- 均变为小写：`c |= 32`。
- 均变为大写：`c &= -33`。

### 数据结构

#### 哈希表判空操作简化

以哈希表的value类型Integer为例，与其先判断`containsKey(key)`再将`get(key)`赋给int变量，可以直接赋给Integer变量，再判断其是否为null。

```java
Map<KeyType, Integer> hashmap = new HashMap<>();
...
Integer x = hashmap.get(key);
if (i != null && i > 0) ...
```

#### 初始化哈希表

```java
Map<KeyType, ValueType> map = new HashMap<>() {{
    put(key1, value1);
    put(key2, value2);
    ...
}};
```

#### 向List\<List\<Integer>>添加新列表

```java
List<List<Integer>> ans = new ArrayList<>();
...
loop {
	...
	ans.add(Arrays.asList(a, b, c));
}
```