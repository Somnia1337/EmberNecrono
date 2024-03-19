## 1. 一个简单的 Java 程序

```java
public class HelloJava {
    public static void main(String[] args) {
        System.out.println("Hello, Java!"); //*
    }
}
```

**关键字 `class`**：表明 Java 程序的全部内容都包含在 **类**(class)中。

**关键字 `public`**：访问修饰符，表明这个类是 **公共类**（公共可见）。

**类名** `HelloJava`：必须以字母开头，后跟字母、数字的任意组合（习惯上使用 **驼峰命名法**(camel case)，即每个单词首字母为大写）。

**源代码文件**：文件名必须与公共类的类名相同，并以 `.java` 作为扩展名。1 个源文件中只能有 1 个公共类，而可以有任意个非公共类。

**方法 `main()`**：每个 Java 程序都必须包含的方法（Java 中的“方法”即 C 中的“函数”）。

**方法 `println()`**：在打印结束后自动添加 `'\n'`，`System.out` 对象还有一个不自动添加 `'\n'` 的方法 `print()`。

**点号(.)**：调用方法，星标语句对 `System.out` 调用其方法 `println()`。

```java
*object.*method(*parameters)
```

## 2. 注释

`// ...`，`/* ... */`：用法与 C 相同，`/* */` 不能像括号一样嵌套，第一次出现的 `*/` 将与开头的 `/*` 匹配。

`/** */`：用于文档。

## 3. 数据类型

8 种基本类型：4 种整型，2 种浮点类型，1 种字符类型，1 种布尔类型。

### 1) 整型

| 类型 | 存储大小/字节 |
| ---- | ---- |
| byte | 1 |
| short | 2 |
| int | 4 |
| long | 8 |

Java 中整型的取值范围与具体平台无关，无论移植到哪里，一个类型的存储大小都不改变。

特殊的前后缀：

- 长整型：后缀 `L/l`，例如 `4000000000L`。
- 16 进制：前缀 `0X/0x`，例如 `0xCAFE`。
- 8 进制：前缀 `0`，例如 `010`，等于十进制下的 `8`。
- 2 进制：前缀 `0B/0b`，例如 `0b1001`，等于十进制下的 `9`。

可以为数字字面量加下划线以提高其可读性（例如 `1_000_000`），编译时它将被删除。

不存在无符号形式(unsigned)。

### 2) 浮点类型

| 类型 | 存储大小/字节 |
| ---- | ---- |
| float | 4 |
| double | 8 |

`float` 值有后缀 `F/f`，没有后缀的小数默认为 `double`，也可以为 `double` 值添加后缀 `D/d`。

浮点数值的计算过程中将产生舍入误差（例如 `2.0 - 1.1` 的打印结果为 `0.8999999999999999`），如需绝对精确，应使用 **BigDecimal 类**。

### 3) `char` 类型

最好不使用 `char`。

### 4) `boolean` 类型

二元值：`false` / `true`。

整型值与布尔值不能相互转换，在 C 中常错写的 `if (x = 0)`（应为 `x == 0`）在 Java 中无法编译。

## 4. 变量与常量

### 1) 声明变量

**标识符**：可由字母、数字、货币符号以及标点连接符组成，首字符不能为数字。

“字母”可为表示自然语言字母的任何 Unicode 字符，包括 ä、π 等，“标点连接符”包括下划线(\_)、波浪线(﹏)等。

### 2) 初始化变量

**关键字 `var`**：对于局部变量，如果可以从变量的初始值推断其类型，就只需使用关键字 `var`，而不再需要声明类型。

```java
var vacationDays = 12; // int
var greeting = "Hello"; // String
```

### 3) 常量

**关键字 `final`**：将变量声明为常量。`final` 表示这个变量的值已经是最终值、不能再更改。

```java
final double CM_PER_INCH = 2.54;
```

习惯上常量名全部字母为大写。

**类常量**（见 \[2 4. 2) 静态常量]）：在类内声明的常量，此时除 `final` 外，还需 **关键字 `static`**。

```java
public class Constants {
    public static final double d = 2.54;
	
    public static void main(String args[]) {
        double w = 8.5;
        double h = 11;
        System.out.println("Paper size in centimeters: " + w * d + " by " + h * d);
    }
}
```

如果一个类常量被标记 `public`，那么其他类也可以访问它（`*Class.*FIELD`）。上例中，其他类可以通过 `Constants.d` 访问常量 `d`。

保留字 `const`：目前未使用，仍需用 `final` 声明常量。

### 4) 枚举类型

有时一个变量只包含有限种可能值（如披萨的小、中、大、超大），可以使用枚举类型。

```java
enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE}

Size s = Size.MEDIUM;
```

如果枚举内只有枚举，可以不加分号，而有其他内容（方法、属性、构造函数等）时必须加分号。

`Size` 类型的变量只能存储它的声明中所列的某个值或 `null`（表示此变量没有设置任何值）。

## 5. 运算符

### 1) 算术运算符

整数除以 `0` 将产生一个异常，浮点数除以 `0` 将得到无穷大或 `NaN`(Not a Number)。

### 2) 数学函数与常量

**方法 `sqrt()`**：计算平方根。

```java
double e = Math.sqrt(16);
```

**静态方法**：不处理对象的方法，`sqrt()` 即为静态方法。

**方法 `floorMod()`**：计算最小正余数。

```java
double f = Math.floorMod(a, b);
```

普通的求余运算符(%)可能返回负结果（当被除数为负、除数为正时），`floorMod()` 能解决此问题。遗憾的是，`floorMod()` 在被除数为正、除数为负时仍将返回负结果。

```java
/* Math 类的其他常用方法和常量 */
/* 三角函数 */
Math::sin
Math::cos
Math::tan
Math::atan
Math::atan2

/* 幂函数, 指数函数及其反函数 */
Math::pow
Math::exp
Math::log // 自然对数
Math::log10

/* 常量 */
Math::PI
Math::E
```

如需多次使用 `Math` 类的方法，不必每次都加 `Math.`，而可以在源文件的开头导入 `Math`：

```java
import static java.lang.Math.*;
```

`Math` 的方法在计算时全部使用 `double`，这可能导致不同平台上的运行结果不同，为了避免，可以使用 `StrictMath` 类（类似于 Math，不过效率较差）。

如果一个计算结果溢出（如 `1000000000 * 3`，超过最大的 `int`），算术运算将返回错误结果而不产生异常，此时应使用 **方法 `Math.multiplyExact()`**，调用 `multiplyExact(1000000000, 3)` 将抛出一个异常。类似的方法还有 `addExact()`，`subtractExact()` 等。

### 3) 类型转换

可能损失精度的类型转换：`int` → `float`，`long` → `float`，`long` → `double`。

### 4) 强制类型转换

```java
double x = 9.997;
int nx = (int) x; // 9
```

强制转换将截断小数部分。

**方法 `Math.round()`**：对浮点数四舍五入。

```java
double x = 9.997;
int nx = (int) Math.round(x); // 10
```

`round()` 返回值类型为 `long`，仍需显式地强制转换。

如果强制转换时数值超出目标类型的上限（例如 `byte(300)`），结果将截断为一个错误值。

### 5) 赋值

如果运算结果与左侧操作对象的类型不符，结果将被自动强制转换。

```java
int x = 0;
x += 3.5; // 3

// 等价于

int x = 0;
x = (int) (x + 3.5); // 3
```

### 6) `switch` 表达式

```java
String seasonName = switch (seasonCode) {
	case 0 -> "Spring";
	case 1 -> "Summer";
	case 2 -> "Fall";
	case 3 -> "Winter";
	default -> "???";
};
```

可以为一个 `case` 指定多个标签，用逗号分隔：

```java
String isOdd = switch (num) {
	case 1, 3, 5 -> "Yes";
	case 2, 4, 6 -> "No";
	default -> "???";
};
```

参数类型为整数或字符串的 `switch` 表达式必须有标签 `default`。

## 6. 字符串

### 1) 子串

**方法 `substring()`**：截取字符串的子串。

```java
String subString = *motherString.substring(*start, *end);
```

`start` 为开始截取的字符编号，`end` 为刚好不截取的第一个字符编号。

```java
String greeting = "Hello";
String s = greeting.substring(0, 3); // "Hel"
```

### 2) 拼接

**字符串连接符**(+)：连接字符串。

```java
String head = "Clash";
String tail = "Royale";
String combinedWord = head + tail; // "ClashRoyale"
```

当一个非字符串的值与字符串连接时，它将转换为字符串，此特性常用于打印语句。

```java
int ans = 1;
System.out.println("The answer is " + ans); // The answer is 1
```

**方法 `join()`**：将多个字符串拼接为一个，以指定的界定符(delimiter)分隔。

```java
String joinedString = String.join(*delimiter, *subString1, *subString2, ...);
```

第一个参数 `delimiter` 为界定符字符串，它将出现在拼接后的每两个子串之间。

```java
String all = String.join(" ", "A", "B", "C"); // "A B C"
```

**方法 `repeat()`**：将一个字符串重复多次拼接为一个新串。

```java
*string.repeat(*times)。
```

```java
String repeated = "Java".repeat(3); // "JavaJavaJava"
```

### 3) 不可变

`String` 类没有提供方法来直接修改字符串变量的值，可以提取要保留的子串，再拼接上替换的字符 实现间接修改。

```java
String word = "Hello";
word = word.substring(0, 3) + "p"; // "Help"
```

C 与 Java 字符串的区别：

- C 的字符串是字符数组（`char []`），Java 的字符串类似于 C 的指针（`char*`）。
- C 的字符串可以修改、不可共享（因为是独属于某个变量的值），Java 的字符串不可修改、可以共享（可以认为 Java 的字符串都存储在一个公共库内，字符串变量只是指向某个位置）。

### 4) 比较

**方法 `equals()`**：比较两个字符串，区分大小写。

```java
*a.equals(*b);
```

如果`a`、`b`相同返回 `true`，否则返回 `false`。

**方法 `equalsIgnoreCase()`**：比较两个字符串，忽略大小写。

不可以用双等号(`==`)比较两个字符串（除非用于 Null 串），因为它只能确定两个字符串是否存放在相同位置，而实际上只有字符串字面常量共享。

### 5) 空串与 Null 串

**空串**：长度为 0 的字符串（`""`）。

```java
if (str.length() == 0)
// 或者
if (str.equals(""))
// 再或者
if (str.isEmpty())
```

**Null 串**：值为 `null` 的字符串，`null` 表示目前没有任何对象与之关联。

```java
if (str == null)
```

### 6) 构建字符串

**`StringBuilder` 类** 可以高效地拼接很多字符串。

```java
StringBuilder builder = new StringBuilder; // 创建对象

builder.append(ch); // 追加单个字符
builder.append(str); // 追加字符串

String completedString = builder.toString(); // 转换为 String
```

### 7) 文本块

**文本块**：可以换行的字符串。

```java
"Hello\nJava\n"

// 此字符串含有多个换行符，不易于读写，它等价于文本块

"""
Hello
Java
"""
```

开始三引号(""")后的换行符不属于文本块，而结束三引号(""")前的换行符属于文本块。

文本块适合用于包含其他语言编写的代码，如 SQL 和 HTML。

## 7. 输入与输出

### 1) 读取输入

**`Scanner` 类** 在 `java.util` 包中定义，读取控制台输入前，要先构建一个与“标准输入流” `System.in` 关联的 `Scanner` 实例。

```java
import java.util.*;

Scanner in = new Scanner(System.in);
```

**方法 `nextLine()`**：读取一行输入。

**方法 `next()`**：读取一个单词（以空格作为分隔符）。

**方法 `nextInt()`**：读取一个整数。

**方法 `nextDouble()`**：读取一个浮点数。

```java
*scanner.*method();
```

```java
Scanner nameReader = new Scanner(System.in);
System.out.println("What's your name?");
String name = nameReader.nextLine();
```

### 2) 格式化输出

**方法 `print()`**：将参数打印到控制台。

**方法 `printf()`**：更精确地控制打印，其用法覆盖了 C 的函数 `printf()`，另外还有新增。

> 用于 `printf()` 的标志见 \[P59]

**方法 `format()`**：构建一个格式化的字符串（不打印到控制台）。

```java
String message = String.format("a = %d, b = %d", a, b);
```

以上语句还可以用 **方法 `formatted()`**（Java 15）简化为：

```java
String message = "a = %d, b = %d".formatted(a, b);
```

### 3) 文件输入与输出

读取文件前，要先构建一个 `Scanner` 实例。

```java
Scanner in = new Scanner(Path.of("myfile.txt"), StandardCharsets.UTF_8);
```

写入文件前，要先构建一个 `PrintWriter` 实例。

```java
PrintWriter out = new PrintWriter("myfile.txt", StandardCharsets.UTF_8);
```

使用绝对路径时，要在每个反斜线之前再加一个额外的反斜线转义，例如 `"c:\\files\\myfile.txt"`。

可以像输出到 `System.out` 一样使用 `print()`、`println()`、`printf()` 等方法输出到文件。

## 8. 控制流程

### 1) 块

与 C 不同的是，Java 不允许在嵌套的块中声明同名变量。

### 2) 条件语句

与 C 相同。

### 3) 循环

> 见 \[1 10. 3) for each 循环]

### 4) `switch`（语句）

与 \[1 5. 6) `switch` 表达式] 中的 `switch` 表达式不同，这里介绍的是 `switch` 语句。

`switch` 在 C 中的经典形式也适用于 Java。

> `switch` 的 4 种形式见 \[P73]

`case` 标签可以有多个字符串：

```java
String input = ...;
switch (input.toLowerCase()) {
    case "yes", "y" -> {
        ...
    }
    case "no", "n" -> {
        ...
    }
    default -> {
        ...
    }
}
```

**直通行为**：`switch` 中执行某个标签后，在遇到 `break` 前，后续的标签也被执行的行为。

如果每个 `case` 以冒号(:)结束，则有直通行为，以箭头(->)结束，则没有直通行为。

不能在同一个 `switch` 中混用冒号和箭头。

**关键字 `yield`**：终止 `switch` 的执行，并额外生成一个值作为表达式的值。

`switch` 的每个分支必须生成一个值，这个值一般直接跟在箭头之后，但有时需要不止一条语句，此时应在箭头之后使用 花括号与 `yield`。

```java
case "Spring" -> {
    System.out.println("spring time!");
    yield 1;
}
```

`switch` 表达式优于语句，如果在每个分支内为变量赋值 / 为方法调用值，则应使用表达式。

```java
// 表达式
num = switch (season) {
	case "Spring", "Summer", "Winter" -> 6;
	case "Fall" -> 4;
	default -> 1;
};

// 优于

// 语句
switch (season) {
	case "Spring", "Summer", "Winter" -> num = 6;
	case "Fall" -> num = 4;
	default -> num = 1;
}
```

### 5) 中断控制流程的语句

**带标签 `break`**：可以跳出多重嵌套的循环。嵌套很深的循环语句可能产生意外结果，有时需要完全跳出所有嵌套循环。

**标签**：任意取名，放在想要跳出的最外层循环之前，后跟冒号。

```java
read_data: // 标签
while () {
    for () {
        if () break read_data; // 直接跳出 while
    }
}
```

标签还可以放在任何语句块之前，遇到 `break *lable` 时将跳出整个块。

`continue` 与 C 相同。

## 9. 大数

**`BigInteger` 类**、**`BigDecimal` 类**：`java.math` 包中的两个类，可以处理包含任意长度数字序列的数值（有时基本的整数和浮点数精度不满足需求）。

```java
BigInteger.ZERO
BigInteger.ONE
BigInteger.TWO
BigInteger.TEN
```

**方法 `valueOf()`**：将普通的数转换为大数。

```java
BigInteger a = BigInteger.valueOf(100);
```

对于极长的数，需要构建一个实例（`BigDecimal` 类型则总是必须构建实例）。

```java
BigInteger reallyBig = new BigInteger("2938547390493857652716904682634902717656783095880143523");
```

大数运算时不能使用常规的运算符，只能使用大数类中的方法 `add()` 和 `mutiply()` 等。

```java
BigInteger c = a.add(b); // c = a + b
```

## 10. 数组

### 1) 声明数组

```java
int[] a = new int[100];

// 或者

var a = new int[100];
```

长度不要求是常量，不过创建之后无法改变。

可以提供初始化列表，此时不再需要 `new`。

```java
int[] a = {2, 3, 5, 7, 11};
```

最后一个值后面允许有逗号，如需不断为数组添加新值，这样很方便。

**重新初始化** 时需要 `new`。

```java
a = new int[]{1, 3, 5, 7, 9};
```

**复制**：对新定义的数组用赋值运算符，将已有数组的内容复制到新数组。

```java
int[] b = new int[]{2, 3, 5, 7, 11};
int[] c = b;
```

### 2) 访问数组元素

创建一个——

  - 数字数组时，所有元素默认初始化为 `0`。
  - `boolean` 数组时，所有元素默认初始化为 `false`。
  - 对象（比如字符串）数组时，所有元素默认初始化为 `null`（表示没有指向对象）。
  
**方法 length**：返回数组的长度。

```java
for (int i = 0; i < a.length; i++) System.out.println(a[i]);
```

### 3) for each 循环

**for-each 循环**：依次处理数组中的所有元素。

```java
for (*Type *x : *collection) { // "for each x in collection"
	...
}
```

它将给定变量 `*x` 依次设置为集合中的所有元素，并执行语句块 `...`。`*collection` 必须是一个数组或者实现 `Iterable` 接口的类对象（例如 `ArrayList`）。

上文的代码可以用 for-each 简化为：

```java
for (int x : a) System.out.println(x);
```

for-each 循环的循环变量将遍历数组中所有元素的值，而不是索引值。

**方法 `Arrays.toString()`**：返回一个二维数组的元素列表字符串，形式如 `"[2, 3, 5, 7, 11]"`。

```java
System.out.println(Arrays.toString(a));
```

### 4) 数组拷贝

**数组拷贝**：将一个数组变量拷贝到另一个数组变量，此时两个变量将引用同一个数组。

```java
int[] a;
int[] b = a; // 现在, 对 a 和 b 的修改是同步的
b[5] = 12; // a[5] 也为 12
```

**方法 `Arrays.copyOf()`**：将一个数组所有元素的值拷贝到另一个数组。

```java
int[] copiedNumbers = Array.copyOf(numbers, numbers.length);
```

第 2 个参数为新数组的长度，它可以用于扩容数组。

```java
numbers = Arrays.copyOf(numbers, numbers.length * 2); // 将原数组扩充到 2 倍长
```

当然也可以压缩数组，此时将只拷贝指定长度内的一部分值。

### 5) 命令行参数

**参数 `String[] args`**：出现在每个 `main()` 中，表示 `main()` 将接收一个字符串数组，也就是命令行上指定的参数。

在命令行中这样调用 `java *ClassName -g a bbb`，`args` 将为 `["g","a","bbb"]`。

### 6) 数组排序

**方法 `Arrays.sort()`**：用优化的快速排序算法对数值型数组排序。

```java
int[] a = new int[10000];
...
Arrays.sort(a);
```

##### 实例：抽彩游戏（P84）

```java
import java.util.Arrays;
import java.util.Scanner;

class LotteryDrawing {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.print("Numbers you need to draw: ");
        int k = in.nextInt();
        System.out.print("The highest number you can draw: ");
        int n = in.nextInt();
        int[] nums = new int[n];
        Arrays.setAll(nums, a -> a + 1);
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            int r = (int) (Math.random() * n);
            res[i] = nums[r];
            nums[r] = nums[--n];
        }
        Arrays.sort(res);
        System.out.println("Bet the following combination, it'll make you rich!");
        System.out.println(Arrays.toString(res));
    }
}
```

**方法 `Math.random()`**：返回一个 $[0, 1)$ 的随机浮点数，用整型 `n` 乘以这个浮点数可以得到 $[0, n - 1]$ 的随机整数。

### 7) 多维数组

for-each 只能循环处理“行”，无法自动循环处理二维数组的所有元素，遍历整个二维数组时需要两个嵌套的 for-each。

```java
int[][] a = ...;

for (int[] row : a)
    for (int val : row)
        ...
```

**方法 `Arrays.deepToString()`**：返回一个二维数组的元素列表字符串，形式如 `"[[1, 2], [3, 4], [5, 6]]"`。

```java
System.out.println(Arrays.deepToString(a));
```

### 8) 不规则数组

**不规则数组**：各行长度不同的数组。

```text
1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
```

```java
int[][] arr = new int[n][];
for (int i = 0; i < n; i++) arr[i] = new int[i + 1];
```