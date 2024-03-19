## 1. Guarded Suspension模式

在家更衣时，邮递员来送邮件了，所以只能请邮递员稍等，换好衣服再打开门。

Guarded Suspension模式，如果执行当前的处理将导致问题，则让执行处理的线程进行等待，以保证程序的安全性。

## 2. 示例

|类名|说明|
|-|-|
|Request|表示请求|
|RequestQueue|依次存放请求|
|ClientThread|发送请求|
|ServerThread|接收请求|
|Main|测试程序行为|

```java
public class Request
{
    private final String name;
    public Request(String name)
	{
        this.name = name;
    }
    public String getName()
	{
        return name;
    }
    public String toString()
	{
        return "[ Request " + name + " ]";
    }
}
```

```java
import java.util.Queue;
import java.util.LinkedList;

public class RequestQueue
{
    private final Queue<Request> queue = new LinkedList<Request>();
    public synchronized Request getRequest()
	{
        while (queue.peek() == null)
		{
            try
			{
                wait();
            }
            catch (InterruptedException e)
			{
            }
        }
        return queue.remove();
    }
    public synchronized void putRequest(Request request)
	{
        queue.offer(request);
        notifyAll();
    }
}
```

```java
import java.util.Random;

public class ClientThread extends Thread
{
    private final Random random;
    private final RequestQueue requestQueue;
    public ClientThread(RequestQueue requestQueue, String name, long seed)
    {
        super(name);
        this.requestQueue = requestQueue;
        this.random = new Random(seed);
    }
    public void run()
	{
        for (int i = 0; i < 10000; i++)
		{
            Request request = new Request("No." + i);
            System.out.println(Thread.currentThread().getName() + " requests " + request);
            requestQueue.putRequest(request);
            try
			{
                Thread.sleep(random.nextInt(1000));
            }
			catch (InterruptedException e)
			{
            }
        }
    }
}
```

```java
import java.util.Random;

public class ServerThread extends Thread
{
    private final Random random;
    private final RequestQueue requestQueue;
    public ServerThread(RequestQueue requestQueue, String name, long seed)
    {
        super(name);
        this.requestQueue = requestQueue;
        this.random = new Random(seed);
    }
    public void run()
    {
        for (int i = 0; i < 10000; i++)
        {
            Request request = requestQueue.getRequest();
            System.out.println(Thread.currentThread().getName() + " handles  " + request);
            try
            {
                Thread.sleep(random.nextInt(1000));
            }
            catch (InterruptedException e) 
            {
            }
        }
    }
}
```

```java
public class Main
{
    public static void main(String[] args)
    {
        RequestQueue requestQueue = new RequestQueue();
        new ClientThread(requestQueue, "Alice", 3141592L).start();
        new ServerThread(requestQueue, "Bobby", 6535897L).start();
    }
}
```

方法getRequest：

```java
public synchronized Request getRequest()
{
	while (queue.peek() == null)
	{
		try
		{
			wait();
		}
		catch (InterruptedException e)
		{
		}
	}
	return queue.remove();
}
```

它的目的是从队列中取出一个Request实例，这需要满足条件`queue.peek() != null`，称为守护条件，结构如下：

```java
while (!/*守护条件*/)
	/*调用wait*/
/*执行目标处理*/
```

## 3. Guarded Suspension模式中的角色

Guarded Suspension模式中的角色为GuardedObject，即被守护的对象，它是一个持有被守护的方法(guardedMethod，上例中为getRequest)的类，上例中为RequestQueue。

## 4. 拓展思路的要点

### 1) 附加条件的synchronized

在STE模式中，只要有一个线程进入临界区，其他线程就无法进入，只能等待。而Guarded Suspension模式中，线程是否等待取决于守护条件是否成立，类似于附加了条件的STE模式。

### 2) 生存性

如果没有修改对象状态的处理，无论调用多少次notify/notifyAll，线程都会因守护条件不满足而随着while再次等待，失去生存性。

### 3) 可复用性

wait/notifyAll只出现在RequestQueue中，将Guarded Suspension的实现细节封装在一个类中，使得它可复用，其他任何类只需调用它的getRequest、putRequest即可。