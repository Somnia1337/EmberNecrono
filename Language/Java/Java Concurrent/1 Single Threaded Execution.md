## 1. Single Threaded Execution模式

有一根很细的独木桥，每次只允许一个人通过，如果桥上同时有两个人就会垮掉。

Single Threaded Execution模式，意即“以一个线程执行”，确保同一时间内只有一个线程执行操作，这是多线程程序设计的基础。

有时它被称为临界区(critical section)或临界域(critical region)，STE这个名称侧重于执行处理的线程（过桥者），而别名侧重于执行范围（桥）。

## 2. 示例1：不使用STE模式的程序

模拟一个每次只允许一人通过的门，有三个人频繁地通过它，每次通过时增加总通过次数，并记录通行者信息。

|类名|说明|
|-|-|
|Main|创建门，并让三人不断通过|
|Gate|表示门，在有人通过时记录其信息|
|UserThread|表示人|

```java
public class Main
{
    public static void main(String[] args)
	{
        System.out.println("Testing Gate, hit CTRL+C to exit.");
        Gate gate = new Gate();
        new UserThread(gate, "Alice", "Alaska").start();
        new UserThread(gate, "Bobby", "Brazil").start();
        new UserThread(gate, "Chris", "Canada").start();
    }
}
```

```java
public class Gate
{
    private int counter = 0;
    private String name = "Nobody";
    private String address = "Nowhere";
    public void pass(String name, String address)
	{
        this.counter++;
        this.name = name;
        this.address = address;
        check();
    }
    public String toString()
	{
        return "No." + counter + ": " + name + ", " + address;
    }
    private void check()
	{
        if (name.charAt(0) != address.charAt(0))
		{
            System.out.println("***** BROKEN ***** " + toString());
        }
    }
}
```

```java
public class UserThread extends Thread
{
    private final Gate gate;
    private final String myname;
    private final String myaddress;
    public UserThread(Gate gate, String myname, String myaddress)
	{
        this.gate = gate;
        this.myname = myname;
        this.myaddress = myaddress;
    }
    public void run()
	{
        System.out.println(myname + " BEGIN");
        int i = 0;
        while (i < 10000)
		{
            gate.pass(myname, myaddress);
            i++;
        }
    }
}
```

```text
Testing Gate, hit CTRL+C to exit.
Alice BEGIN
Bobby BEGIN
Chris BEGIN
***** BROKEN ***** No.1277: Alice, Alaska
***** BROKEN ***** No.3163: Alice, Alaska
***** BROKEN ***** No.11619: Bobby, Brazil
***** BROKEN ***** No.17147: Bobby, Canada
***** BROKEN ***** No.18155: Bobby, Brazil
***** BROKEN ***** No.18648: Bobby, Brazil
***** BROKEN ***** No.19087: Bobby, Brazil
***** BROKEN ***** No.19569: Bobby, Brazil
***** BROKEN ***** No.20008: Bobby, Brazil
```

运行结果显示，在第1277次有人通过门时首次出现了错误（姓名与国籍首字母不同）。

多线程的难点之一就是，如果检查出错误说明程序不安全，但如果没有检查出错误，并不说明程序安全，因为测试的次数与时间点也会影响错误的检查。

另外，如果打印调试信息的代码本身就不是线程安全的，那么调试信息也可能是错误的，此处，输出结果中只有一条信息`***** BROKEN ***** No.17147: Bobby, Canada`的姓名与国籍首字母不同，而其他信息都看不出问题所在。

出错的原因是多个线程同时调用方法pass：

```java
public class Gate
{
	public void pass(String name, String address)
	{
        this.counter++;
        this.name = name;
        this.address = address;
        check();
    }
}
```

例如Alice的线程执行完前3句后、调用方法check之前，Bobby的线程可能刚好执行了前2句，导致姓名与国籍首字母不同，check打印错误信息。

## 3. 示例2：使用STE模式的程序

要将上文不安全的的程序修改为使用STE模式的程序，只需将Gate.pass与Gate.toString声明为synchronized方法：

```java
public class Gate
{
    private int counter = 0;
    private String name = "Nobody";
    private String address = "Nowhere";
    public synchronized void pass(String name, String address)
	{
        this.counter++;
        this.name = name;
        this.address = address;
        check();
    }
    public synchronized String toString()
	{
        return "No." + counter + ": " + name + ", " + address;
    }
    private void check()
	{
        if (name.charAt(0) != address.charAt(0))
		{
            System.out.println("***** BROKEN ***** " + toString());
        }
    }
}
```

现在，运行结果中不会再有错误信息。

## 4. STE模式中的角色

STE模式中有一个发挥SharedResource(共享资源)作用的类（上例中为Gate）。

SharedResource是可被多个线程访问的类，它包含的方法主要分为：

- safeMethod：多个线程同时调用时也不会出现问题的方法。
- unsafeMethod：多个线程同时调用时会出现问题的方法。

STE模式的核心就是保护unsafeMethod，保证其在同一时间只能由一个线程访问，也就是声明为synchronized。

只允许单个线程执行的程序范围称为临界区。

## 5. 拓展思路的要点

### 1) 何时使用STE模式

- 多线程时：在单线程程序中使用STE模式当然也安全，但效率稍低，好比一个独居的人用厕所时要锁门，既无必要也麻烦。
- 多个线程访问时：即使是多线程程序，如果各个线程的操作完全独立（称为线程互不干涉），也无需使用STE模式。
- 状态可能变化时：如果创建实例后其状态再也不会改变，就无需使用STE模式。

### 2) 生存性与死锁

使用STE模式时可能发生死锁，死锁是指两个线程分别持有锁、并互相等待对方释放锁的现象，死锁时程序失去生存性。

可以将死锁形象理解为，有两个同时吃饭的人，餐桌上只有一双筷子，他们分别持有其中一根，且都在等对方放下另一根，这种情况下没有人能够吃饭。

在STE模式中，满足下列条件时将发生死锁：

- 存在多个SharedResource角色。
- 线程在持有某个SharedResource角色的锁的同时，试图获得其他SharedResource角色的锁。
- SharedResource角色是对称的，好比获取两根筷子的顺序不会影响吃饭。

只要破坏其中一个条件，就可以避免死锁（见课后习题）。

## 6. 关于synchronized

### 1) 保护的完整性

在多处使用一个字段时，要考虑是否在所有地方都对其进行了保护，可以形象理解为，如果只锁了门、而敞开窗户，那么屋里的东西仍然不安全。

上例中，pass与toString声明为synchronized，check中也使用了`name`与`address`字段、却没有声明为synchronized，不过这是安全的，因为只有pass调用了check，且check声明为private，所以对字段的保护是完整的。

### 2) 保护的单位

如果为Gate添加两个方法：

```java
public class Gate
{
	...
	public synchronized void setName(String name)
	{
		this.name = name;
	}
	public synchronized void setAddress(String address)
	{
		this.address = address;
	}
}
```

它们都声明为synchronized，但却不安全，因为保护的目标是同时为`name`与`address`字段赋值，但这两个setter分别保护两个字段，它们可能在同一时间被不同线程调用，并未达成保护的目标。

### 3) 保护的锁

不同实例的锁不同，在保护时需明确使用的时哪一个锁。尤其是synchronized代码块，需指定实例名称。如果获取了错误的实例的锁，就好比要保护自己家，却锁住了邻居家的门。

### 4) 原子操作

原子操作：不可分割的操作，从多线程的角度来看，声明为synchronized的块在同一时间只允许一个线程访问，所以被synchronized保护的一系列操作为一个原子操作。

## 7. 练习题

### 1-1

答案：延长临界区可以提高检查出错误的可能性，此例中，可以在字段`name`与`address`的赋值之间调用sleep：

```java
public synchronized void pass(String name, String address)
	{
        this.counter++;
        this.name = name;
        try
		{
			Thread.sleep(1000);
		}
		catch (InterruptedException e)
		{
		}
        this.address = address;
        check();
    }
```

运行结果：

```text
Testing Gate, hit CTRL+C to exit.
Bobby BEGIN
Alice BEGIN
Chris BEGIN
***** BROKEN ***** No.3: Chris, Brazil
***** BROKEN ***** No.3: Chris, Brazil
***** BROKEN ***** No.6: Bobby, Canada
***** BROKEN ***** No.7: Chris, Brazil
***** BROKEN ***** No.7: Chris, Alaska
***** BROKEN ***** No.9: Alice, Canada
***** BROKEN ***** No.10: Chris, Alaska
***** BROKEN ***** No.10: Chris, Brazil
***** BROKEN ***** No.12: Bobby, Canada
***** BROKEN ***** No.13: Chris, Brazil
***** BROKEN ***** No.13: Chris, Alaska
```

### 1-2

答案：将字段声明为private是为了便于安全性检查，此时只需检查本类内部的安全性，如果声明为protected，还需检查子类以及同包类，如果声明为public，还需检查一切访问字段的类。

### 1-3

答案：某个线程调用`name`、还未调用`address`时，可能有其他线程修改`address`，导致首字母不一致。虽然不将toString声明为synchronized也不会降低安全性，但是一般来说，多个线程共享的字段必须使用synchronized保护。

### 1-4

TTTTT

4：F，未阅读的部分可能有不安全的方法。

5：F，即使读完也无法判断生存性，因为其他使用Point类的类可能发生死锁。

### 1-5

答案：不安全，因为虽然计数器的更改语句都只有一行，但这不是原子操作，实际操作顺序：

- 读取`counter`的值。
- 该值 + 1。
- 将新值赋给`counter`。

如果在某个线程操作`counter++`的过程中，其他线程操作了`counter--`，那么最终`counter--`的效果将被覆盖。

### 1-6

答案：有两种方法：

- 必须按一定顺序拿取餐具（如必须先拿叉子），这破坏了死锁条件3（SharedResource角色是对称的）。
- 餐具必须成对拿取，这破坏了死锁条件1（存在多个SharedResource角色）。

### 1-7

答案：见书。