## 1. Immutable模式

String类没有修改字符串内容的方法，String实例表示的字符串内容永远不会改变，因此一个String实例无论由多少线程同时访问，也无需用synchronized保护。

Immutable模式，意为无法改变，其中存在确保实例状态不发生改变的Immutable类，String就是一个Immutable类。

## 2. 示例

|类名|说明|
|-|-|
|Person|表示人|
|Main|测试程序行为|
|PrintPersonThread|显示Person实例的线程|

```java
public final class Person
{
    private final String name;
    private final String address;
    public Person(String name, String address)
	{
        this.name = name;
        this.address = address;
    }
    public String getName()
	{
        return name;
    }
    public String getAddress()
	{
        return address;
    }
    public String toString()
	{
        return "[ Person: name = " + name + ", address = " + address + " ]";
    }
}
```

```java
public class Main
{
    public static void main(String[] args)
	{
        Person alice = new Person("Alice", "Alaska");
        new PrintPersonThread(alice).start();
        new PrintPersonThread(alice).start();
        new PrintPersonThread(alice).start();
    }
}
```

```java
public class PrintPersonThread extends Thread
{
    private Person person;
    public PrintPersonThread(Person person)
	{
        this.person = person;
    }
    public void run()
	{
        while (true)
		{
            System.out.println(Thread.currentThread().getName() + " prints " + person);
        }
    }
}
```

Person类设有两个字段的getter，但没有setter，且其字段均声明为final，因此一旦创建Person示例，字段值就无法改变。将字段声明为final不是Immutable模式必需的，不过可以明确“这些字段不应该被修改”的意图。

此外，Person声明为final类，这也不是Immutable模式必需的，不过可以防止其子类修改字段值。

## 3. Immutable模式中的角色

Immutable模式中的角色为Immutable类（上例中为Person），其成员字段值不能修改，且创建实例后状态无法改变。无需对其用synchronized保护。

## 4. 拓展思路的要点

### 1) 何时使用Immutable模式

- 实例创建后状态不再改变时。
- 实例是共享的，且被频繁访问时。

### 2) 成对的mutable类与immutable类

如果程序可以拆分为使用某类字段的setter与不使用该setter两部分，则可以分出一个immutable类，应用于后者。

### 3) 确保不可变性

有些修改可能隐式地破坏不可变性：

- 将存储在字段内的实例直接作为getter的返回值。
- 将传入构造器参数的实例直接赋给字段。

### 4) 标准类库中的immutable类

- java.lang.String
- java.math.BigInteger
- java.lang.Integer等包装器类

## 5. 练习题

### 2-1

TFTFF

### 2-2

replace的操作是新建一个字符串，将原字符串的指定字符替换为目标字符后返回新字符串。

### 2-3

答案：约为2倍。

### 2-4

答案：虽然字段`info`声明为final，其指向的StringBuffer对象不会改变，但后者属于mutable类，含有改变实例状态的方法，所以该类也不是immutable类。

### 2-5

答案：第二个构造器将外部传入的实例引用直接赋给了字段，如果该外部实例状态改变，那么虽然该字段仍指向它，但其状态也发生改变，所以不是immutable类。

### 2-6

答案：两个字段的赋值没有保证synchronized。