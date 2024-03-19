## 1. 接口

### 1) 接口的概念

**接口**(interface)：描述类应该做什么（而不指定具体如何做），它不是类，而是对符合此接口的类的一组需求。

例如，`Arrays::sort` 可以排序对象数组，同时要求对象所属的类实现 `Comparable` 接口。

```java
public interface Comparable {
    int compareTo(Object other);
}
```

抽象方法 `compareTo()` 没有具体实现，它将被实现 `Comparable` 接口的类实现。

接口中的方法自动为 `public`，字段自动为 `public static final`，都无需标记。

接口中可以有多个方法、常量，但不能有实例字段。

要使类实现接口，需要：

1. 将类声明为实现该接口。
2. 对该接口中的所有方法提供定义。

**关键字 `implements`**：声明类实现某个接口。

```java
class Employee implements Comparable
```

现在，`Employee` 需要提供 `compareTo` 的定义。

```java
public int compareTo(Object otherObject) {
    Employee other = (Employee) otherObject;
    return Double.compare(salary, other.salary);
}
```

**`Double::compare`**：`*param1 < *param2` 返回负值，相等返回 `0`，否则返回正值。

`compareTo()` 在接口中没有标记 `public`（因为接口中的方法自动为 `public`），但是提供定义时必须标记 `public`，否则编译器将默认其访问属性为包可访问。

```java
class Employee implements Comparable<Employee> {
    public int compareTo(Employee other) {
        return Double.compare(salary, other.salary);
    }
    // ...
}
```

使用接口的理由：Java 是一种强类型语言，调用方法时编译器要能检查其确实存在。

```java
if (a[i].compareTo(a[j]) > 0) {
	swap(a[i], a[j]);
	// ...
}
```

编译器必须确认对 `a` 一定有可用的 `compareTo()`。如果 `a` 是一个 `Comparable` 对象的数组，就可以确保其存在（因为任何实现 `Comparable` 接口的类都必须实现该方法）。

### 2) 接口的属性

接口不是类，不能用 `new` 实例化一个接口，不过可以声明接口变量。

```java
x = new Comparable(...); // ERROR
Comparable x; // OK
```

接口变量只能引用实现了该接口的一个类对象：

```java
x = new Employee(...);
```

使用 `instanceOf` 检查对象是否实现了某个接口：

```java
if (anObject instanceOf Comparable) ...;
```

接口可以扩展。

```java
public interface Moveable {
    void move(double x, double y);
}
```

```java
public interface Powered extends Moveable {
    double SPEED_LIMIT = 95;
	
    double milesPerGallon();
}
```

每个类只能有一个父类，但可以实现多个接口，例如，如果一个类实现 Java 的内置接口 `Cloneable`，`Object::clone` 就可以创建该类对象的一个完全副本。

如果类实现多个接口，在声明中用逗号分隔。

```java
class Employee implements Cloneable, Comparable
```

### 3) 接口与抽象类

```java
abstract class Comparable {
    public abstract int compareTo(Object other);
}
```

这样，`Employee` 类只需继承 `Comparable`，并实现 `compareTo()`。

```java
class Employee extends Comparable {
    public int compareTo(Object other) {
        // ...
    }
}
```

之前提到，每个类只能拥有一个父类，此例中，`Employee` 已经有父类 `Person`，就不能再扩展 `Comparable`。

其他一些语言允许一个类拥有多个父类（C++），这个特性称为 **多重继承**。它将使语言变得复杂、降低效率。

### 4) 静态和私有方法

> P241

### 5) 默认方法

Java 8 之前，接口中的方法只能为抽象方法，而 Java 8 之后，可以为接口方法提供一个默认实现，用 **修饰符default** 标记。

```java
public interface Comparable<T> {
    default int compareTo(T other) {return 0;}
}
```

`Comparable` 接口的每个实现都将覆盖它，不过有些情况下它也有用处（第 7 章）。

默认方法可以调用其他方法。

```java
public interface Collection {
    int size();
	
    default boolean isEmpty() {return size() == 0;}
}
```

这样，实现 `Collection` 的程序员就无需自行实现经常需要使用的 `isEmpty()`。

默认方法的一个重要用途是 **接口演化**，以 `Collection` 接口为例，假设从前有人写了一个 `Bag` 类，它实现 `Collection`，后来为 `Collection` 增加了一个方法 `stream()`，如果 `stream()` 不是默认方法，那么 `Bag` 类将无法编译，因为它没有实现此新方法。默认方法保证了 **源代码兼容**。

### 6) 解决默认方法冲突

> P242-244

### 7) 接口与回调

**回调**(callback)：一种程序设计模式，可以指定某个特定事件发生时采取的动作，例如用户点击按钮时触发的动作。

`java.swing` 包中有一个 `Timer` 类，假设程序中有一个时钟，它可以通过 `Timer` 每秒更新表盘。

实例化 `Timer` 时，需要设置一个时间间隔，并告诉它经过这些时间要做什么。很多语言中可以提供函数名，而 Java 中可以向 `Timer` 传递一个对象，并指定 `Timer` 对其调用某个方法。

`Timer` 要求指定一个对象，且其对应的类需要实现 `java.awt.event` 包的 `ActionListener` 接口。

```java
public interface ActionListener {
    void actionPerformed(ActionEvent event);
}
```

每当经过一定时间，`Timer` 对象将调用 `actionPerformed()`，例如，将 `actionPerformed()` 实现为每秒打印一条消息并发出提示音：

```java
class TimePrinter implements ActionListener {
    public void actionPerformed(ActionEvent event) {
        System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
        Toolkit.getDefaultToolkit().beep();
    }
}
```

然后，实例化 `TimePrinter`，将该对象传递给 `Timer` 的构造器：

```java
TimePrinter listener = new TimePrinter();
Timer timer = new Timer(1000, listener);
```

`Timer` 构造器的第一个参数是一个时间间隔（单位 ms），即经过多长时间触发一次，第二个参数是一个监听器对象（实现了 ActionListener 的类的实例）。

最后，用 **`start()`** 启动计时器：

```java
timer.start();
```

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.time.Instant;

class TimerTest {
    public static void main(String[] args) {
        var listener = new TimePrinter();
        var timer = new Timer(1000, listener);
        timer.start();
        JOptionPane.showMessageDialog(null, "Quit program?");
        System.exit(0);
    }
}

class TimePrinter implements ActionListener {
    public void actionPerformed(ActionEvent event) {
        System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
        Toolkit.getDefaultToolkit().beep();
    }
}
```

### 8) `Comparator` 接口

`String` 类实现了 `Comparable` 接口，`String::compareTo` 可以按字典顺序比较字符串。现在要按长度顺序对字符串排序，但不可能在 `String` 类中用两种不同方式实现 `compareTo`，要处理这种情况，`Arrays::sort` 还有第二个版本，接收一个数组和一个实现了 `Comparator` 的类的实例作为参数。

```java
public interface Comparator<T> {
    int compare(T first, T second);
}
```

编写类 `LengthComparator`，实现 `Comparator` 并指定泛型类型为 `String`：

```java
class LengthComparator implements Comparator<String> {
    public int compare(String first, String second) {
        return first.length() - second.length();
    }
}
```

完成比较时，实例化 `LengthComparator`，将该对象传递给 `Arrays::sort`：

```java
LengthComparator comp = new LengthComparator();
Arrays.sort(words, comp);
```

### 9) 对象克隆

> P247-252

## 2. lambda 表达式

### 1) 为什么引入 lambda 表达式

lambda 表达式是一个可传递的代码块，可以在以后执行若干次。

观察上文 `TimerTest` 和 `LengthComparator` 的例子，它们有一个共同点：都将一个代码块传递给某个目标（`Timer` 或 `sort`），这段代码将在未来某个时间调用。

### 2) lambda 表达式的语法

在上例 `LengthComparator` 中，要计算 `first.length() - second.length()`，由于 Java 是一种强类型语言，要指定 `first` 和 `second` 的类型，改写如下：

```java
(String first, String second) -> first.length() - second.length();
```

这就是一个 lambda 表达式，它是一个代码块、以及传入该块的所有变量的规范。

如果代码块的任务无法用一个表达式完成，就要像编写方法一样使用中括号(`{}`)，并显式地包含 `return` 语句：

```java
(String first, String second) -> {
	if (first.length() < second.length()) return -1;
	else if (first.length() > second.length()) return 1;
	else return 0;
}
```

即使 lambda 没有参数，也要像编写无参数方法一样提供括号：

```java
() -> { for (int i = 0; i < 100; i++) System.out.println(i); }
```

如果编译器能够确定某个参数类型，则可以不标注：

```java
Comparator<String> comp = (a, b) -> a.length() - b.length();
```

在这里，编译器可以推理出 `a` 和 `b` 必然是字符串（因为此 lambda 将返回值赋给字符串比较器 `comp`），这段代码还可简写为：

```java
Comparator<String> comp = Comparator.comparingInt(String::length);
```

如果只有一个参数且其类型可以确定，则小括号也可以省略：

```java
ActionListener listener = event -> System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
```

无需指定 lambda 的返回值类型，因为它总是由上下文推理得出。

存在不返回任何值的分支的 lambda 不合法：

```java
(int x) -> { if (x > 0) return 1; } // ERROR
```

### 3) 函数式接口

**函数式接口**(functional interface)：只有一个抽象方法的接口，需要其实例时可以重写为提供一个 lambda。

上文的方法 `Arrays::sort` 调用：

```java
LengthComparator comp = new LengthComparator();
Arrays.sort(words, comp);

class LengthComparator implements Comparator<String> {
	public int compare(String a, String b) {
		return a.length() - b.length();
	}
}
```

`comp` 是实现了 `Comparator` 的 `LengthComparator` 的实例，而 `Comparator` 是一个函数式接口，因此可以改写为：

```java
Arrays.sort(words, Comparator.comparingInt(String::length));
```

最好将 lambda 看作一个方法，而非一个对象。

lambda 可以转换为一个接口，例如上文的例子 `TimerTest`：

```java
TimePrinter listener = new TimePrinter();
Timer timer = new Timer(1000, listener);

class TimePrinter implements ActionListener {
	public void actionPerformed(ActionEvent event) {
		System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
		Toolkit.getDefaultToolkit().beep();
	}
}
```

改写为：

```java
Timer timer = new Timer(1000, event -> {
	System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
	Toolkit.getDefaultToolkit().beep();
});
```

后者更短，而且不需要单独编写 `TimerPrinter`，可读性更好。

`java.util.function` 包中有一个 **函数式接口 `Predicate`**，其中只声明了一个方法 `test()`。

```java
public interface Predicate<T> {
	boolean test(T, t);
}
```

`Predicate` 常用于过滤数组、集合或流中的元素（见 \[11 流]）。

ArrayList 中有一个 **`removeIf()`**，其参数为一个实现了 `Predicate` 的类的实例，`Predicate` 专用于传递 `lambda`，例如，删除 `list` 中的所有 `null` 值：

```java
list.removeIf(e -> e == null);
```

**函数式接口 `Supplier`**：只含一个抽象方法 `get()`，后者不接受任何参数，返回一个泛型类型的值。

```java
public interface Supplier<T> {
	T get();
}
```

`Supplier` 通常用以实现 **懒计算**(lazy evaluation)，即只在需要数据时才计算。创建一个 `Supplier` 对象并定义计算方法，此后，每当需要生成新数据时，只需对该 `Supplier` 对象调用 `get()`。

```java
Supplier<Integer> randomIntegerSupplier = () -> new Random().nextInt();
// ...
int randomNumber = randomIntegerSupplier.get();
```

### 4) 方法引用

有时 lambda 涉及一个方法，例如调用：

```java
Timer timer = new Timer(1000, event -> System.out.println(event));
```

如果直接把 `println()` 传递给 `Timer` 构造器就更好了：

```java
Timer timer = new Timer(1000, System.out::println);
```

表达式 `System.out::println` 是一个 **方法引用**，它指示编译器生成一个函数式接口的实例，覆盖该接口的抽象方法来调用给定的方法。此例中，编译器将生成一个 `ActionListener` 实例，其 `actionPerformed(ActionEvent e)` 将调用 `System.out.println(e)`。

```java
Arrays.sort(words, String::compareToIgnoreCase);
```

用双冒号(`::`)分隔方法名与对象或类名，有 3 种情况：

- `*object::*instanceMethod`，方法引用等价于一个 lambda，其参数要传递给方法。
- `*Class::*instanceMethod`，首个参数将成为方法的隐式参数，例如上例中的 `String::compareToIgnoreCase` 等价于 `(x, y) -> x.compareToIgnoreCase(y)`。
- `*Class::*staticMethod`，所有参数都将成为方法的显式参数，例如 `Math::pow` 等价于 `(x, y) -> Math.pow(x, y)`。

> 更多示例见 \[P259 表 6-1]

只有当 lambda 只调用一个方法且无其他操作时，才能将其重写为方法引用。例如，`s -> s.length() == 0` 就不能重写为方法引用，因为除 length 调用外，还进行了比较。

可以在方法引用中使用 `this` 参数，例如，`this::equals` 等价于 `x -> this.equals(x)`。

类似地，也可以使用 `super`。

```java
class Greeter {
    public void greet(ActionEvent event) {
        System.out.println("Hello, the time is " + Instant.ofEpochMilli(event.getWhen()));
    }
}

class RepeatedGreeter extends Greeter {
    public void greet(ActionEvent event) {
        var timer = new Timer(1000, super::greet);
        timer.start();
    }
}
```

`RepeatedGreeter::greet` 开始执行后，构造一个 `Timer` 实例 `timer`，然后每隔 1000 ms 执行一次父类方法 `Greeter::greet`。

### 5) 构造器引用

构造器引用与方法引用类似，不过方法名为 `new`，例如，`Person::new` 是对 `Person` 构造器的引用，具体构造器的选择取决于上下文。

可以建立数组类型的构造器引用，例如，`int[]::new`，它有一个参数，即数组的长度，等价于 `x -> new int[x]`。

### 6) 变量作用域

有时需要在 lambda 中访问外围方法或类中的变量：

```java
public static void repeatMessage(String text, int delay) {
	ActionListener listener = event -> {
		System.out.println(text);
		Toolkit.getDefaultToolkit().beep();
	};
	new Timer(delay, listener).start();
}
```

lambda 访问外围方法的参数变量 `text`，但实际上它可能在方法 `repeatMessage` 调用返回很久后才执行，而那时 `text` 早已不存在，为什么还能够访问？

lambda 有 3 个部分：

- 一个代码块
- 参数
- 自由变量（不是参数且不在代码中定义的变量）的值

上例中，`text` 将存储在 lambda 对应的数据结构中，称为 **捕获**(captured)，代码块连同自由变量称为 **闭包**(closure)。

为了确保捕获的值是明确定义的，lambda 只能引用值不会改变的变量。

```java
ActionListener listener = event -> {
	start--; // ERROR: lambda 表达式中使用的变量应为 final 或有效 final
	System.out.println(start);
};
```

此限制的原因：如果在 lambda 中更改变量的值，并发执行多个动作时将不安全。

另外，lambda 引用的变量在外部也不能改变。

```java
for (int i = 0; i <= count; i++) {
	ActionListener listener = event -> {
		System.out.println(i); // ERROR
	};
}
```

lambda 捕获的变量必须是 **事实最终变量**(effectively final)，它是指，对变量初始化之后就不再为其重新赋值（即使没有标记 final）。

lambda 的块遵循嵌套块的作用域规则，同样适用命名冲突与覆盖。

在 lambda 中使用 `this` 时，指的是创建此表达式的方法的 `this` 参数。

```java
class Application {
    public void init() {
        ActionListener listener = event -> {
            System.out.println(this.toString());
            // ...
        };
        // ...
    }
}
```

表达式 `this.toString()` 将调用 `Application` 类型对象的 `toString()`，而不是 `ActionListener` 实例 `listener` 的 `toString()`。

### 7) 处理 lambda 表达式

使用 lambda 的优点是 **延迟执行**(deferred execution)，之所以想要以后再执行，有很多可能的原因：

- 在一个单独的线程中运行代码。
- 多次运行代码。
- 在算法的适当位置运行代码（如排序算法的比较操作）。
- 发生某种情况时运行代码。
- 只在必要时运行代码。

例如，将一个动作重复 $n$ 次，可以编写 `repeat()`，接收重复次数与重复动作，后者用 lambda 表达。要接收此 lambda，需要选择一个函数式接口，此例中使用 `Runnable`：

```java
public static void repeat(int n, Runnable action) {
	for (int i = 0; i < n; i++) action.run();
}

repeat(10, () -> System.out.println("Hi!"));
```

> 常用的函数式接口见 P263 表 6-2

现在需要指定在具体的哪一次迭代中执行动作，为此需要选择一个合适的函数式接口，其方法要有一个 `int` 参数且返回值类型为 `void`，一般用 **接口 `IntConsumer`** 处理 `int` 值。

```java
public interface IntConsumer {
	void accept(int value);
}
```

```java
public static void repeat(int n, IntConsumer action) {
	for (int i = 0; i < n; i++) action.accept(i);
}

repeat(10, i -> System.out.println("Countdown: " + (9 - i)));
```

> 基本类型的特殊化接口见 P264 表 6-3

### 8) 再谈 `Comparator`

> P266-267

## 3. 内部类

**内部类**(inner class) 是定义在另一个类中的类，使用内部类的可能原因：

- 内部类可以对同一个包中的其他类隐藏。
- 内部类方法可以访问定义这些方法的作用域中的数据，包括原本私有的数据。

### 1) 使用内部类访问对象状态

```java
class TalkingClock {
    private int interval;
    private boolean beep;
	
    public TalkingClock(int interval, boolean beep) {
        // ...
    }
	
    public void start() {
        // ...
    }
	
    public class TimePrinter implements ActionListener {
        public void actionPerformed(ActionEvent event) {
            System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
            if (beep) Toolkit.getDefaultToolkit().beep();
        }
    }
}
```

可以看到，一个内部类方法可以访问自身的实例字段，也可以访问创建它的外部类对象的实例字段（此例中为 `beep`）。

因此，内部类的对象总有一个隐式引用，指向创建它的外部类对象，该引用在内部类的定义中不可见。

为了说明此概念，将该引用称为 `outer`，`TimePrinter` 类改写为：

```java
class TimePrinter implements ActionListener {
    public void actionPerformed(ActionEvent event) {
        System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
        if (outer.beep) Toolkit.getDefaultToolkit().beep(); // outer.beep
    }
}
```

外部类的引用在构造器中设置，编译器将为内部类的所有构造器添加一个对应外部类引用的参数，此例中 `TimePrinter` 类没有定义构造器，编译器为其生成了一个无参数的构造器：

```java
public TimePrinter(TalkingClock clock) {
	outer = clock;
}
```

再次强调，`outer` 并不是 Java 的关键字，此处只是引入它来说明该引用的概念。

在 `start()` 中构造一个 `TimePrinter` 类型对象后，编译器将当前 `TalkingClock` 类型对象的 `this` 引用传递给它：

```java
TimePrinter listener = new TimePrinter(this);
```

如果 `TimePrinter` 类是一个普通的类，它就需要通过 `TalkingClock` 类的公共方法访问 `beep` 标志，而使用内部类就避免了提供只对另外一个类有用的访问器。

可以将 `TimePrinter` 类标记 `private`，这样一来只有 `TalkingClock` 类的方法能构造 `TimePrinter` 对象。只有内部类可以为私有，常规类只能为包可见或公共可见。

### 2) 内部类的特殊语法规则

上一节引入 `outer` 来解释内部类对外部类的引用，实际上，该引用的正规语法是 `*OuterClass.this`。

于是内部类 `TimePrinter` 可以改写为：

```java
class TimePrinter implements ActionListener {
    public void actionPerformed(ActionEvent event) {
        // ...
        if (TalkingClock.this.beep) Toolkit.getDefaultToolkit().beep();
    }
}
```

反过来，可以用以下语法更明确地编写内部类对象的构造器：

```java
*outerObject.new *InnerClass(*constructionParams)
```

```java
ActionListener listener = this.new TimePrinter();
```

构造的 `TimePrinter` 对象的外部类引用被设置为构造它的方法的 `this` 引用，所以限定符 `this` 是多余的，不过也有可能通过显式地命名将外部类引用设置为其他对象：

```java
TalkingClock jabberer = new TalkingClock(1000, true);
TalkingClock.TimePrinter listener = jabberer.new TimePrinter();
```

在外部类的作用域之外，可以用 `*OuterClass.*InnerClass` 引用内部类。

### 3) 内部类是否有用、必要和安全

内部类将转换为常规的类文件，用美元符号(`$`)分隔外部类名与内部类名，例如上例中的 `TalkingClock` 类内部的 `TimePrinter` 类将转换为类文件 `TalkingClock$TimePrinter.class`。

内部类可以访问外部类的私有数据，比常规类功能更强。

> 编译器处理类的嵌套关系、其原理及历史见 P272-273

### 4) 局部内部类

在上例中，类名 `TimePrinter` 只在 `TalkingClock` 类的定义中出现了一次（即创建其对象时）。

在类似的情况下，可以在一个方法中局部地定义此内部类：

```java
class TalkingClock {
    private int interval;
    private boolean beep;
	
    public TalkingClock(int interval, boolean beep) {
        // ...
    }
	
    public void start() {
        class TimePrinter implements ActionListener {
            public void actionPerformed(ActionEvent event) {
                System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
                if (beep) Toolkit.getDefaultToolkit().beep();
            }
        }
    }
}
```

声明局部内部类时不能标记其访问属性，其作用域为声明它的块。

局部内部类的优点：

- 对外部完全隐藏，此例中，除 `start()` 以外，没有任何方法知道 `TimePrinter` 类的存在。
- 不仅能访问外部类的字段，还能访问局部变量（不过只能是事实最终变量）。

### 5) 由外部方法访问变量

> P274-275

### 6) 匿名内部类

有时只想创建类的一个对象，甚至不需要为类命名，这样的类称为 **匿名内部类**(anonymous inner class)。

```java
public void start(int interval, boolean beep) {
	ActionListener listener = new ActionListener() {
		public void actionPerformed(ActionEvent event) {
			System.out.println("At the tone, the time is " + Instant.ofEpochMilli(event.getWhen()));
			if (beep) Toolkit.getDefaultToolkit().beep();
		}
	};
	Timer timer = new Timer(interval, listener);
	timer.start();
}
```

它的含义是：创建一个类的对象，该类实现了 `ActionListener` 接口，需要实现的 `actionPerformed()` 在花括号中定义。

格式：

```java
new *SuperType(*construction params) {
	*(inner class methods and data)
}
```

`*SuperType` 可以是接口（如果是，内部类需要实现该接口），也可以是类（如果是，内部类需要扩展该类）。

构造器的名称必须与类名相同，而匿名内部类没有类名，所以它不能有构造器，其构造参数传递给父类构造器；如果它实现一个接口，就不能有构造参数，不过仍需提供小括号(`()`)。

尽管没有构造器，匿名内部类可以提供一个对象初始化块：

```java
Person count = new Person("Dracula") {
	{ //initialization
	// ...
	}
}
```

如果构造参数列表的右括号(`)`)后跟左花括号(`{`)，就是在定义匿名内部类。

通常使用匿名内部类的解决方案比较简短；对于事件监听器和其他回调的实现，最好还是使用 lambda 表达式。

如果将一个匿名内部类实例存储在声明为 `var` 的变量中：

```java
var bob = new Object() { String name = "Bob"; }
System.out.println(bob.name);
```

`new Object() { String name = "Bob"; }` 构造的对象类型为“有一个 `String name` 字段的 `Object`”，它是一个 **不可指示的**(nondenotable)类型，即无法用 Java 语法表示，不过编译器可以理解并将 `bob` 设置为此类型。但如果将 `bob` 声明为 `Object` 类型，`bob.name` 将编译失败。

对于 `equals()`，一般如下实现：

```java
if (getClass() != other.getClass()) return false;
```

但对于匿名内部子类，这个测试不可用。

### 7) 静态内部类

> P278-281

## 4. 服务加载器

> P281-283

## 5. 代理

> P283-289