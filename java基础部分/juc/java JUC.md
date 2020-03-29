### 1.java JUC简介

```java
	在java5.0提供了java.util.concurrent(简称JUC包)，在此包中增加了在并发编程中很常用的使用工具类，用于定义类似于线程自定义子系统，包括线程池、异步IO和轻量级任务框架。提供可调的、灵活的线程池。还提供了设计用于多线程上下文中的Collection实现等。
```

### 2.volatile关键字-内存可见性

#### 2.1内存可见性

```java
1.内存可见性：当某个线程正在使用对象状态而另一个线程在同时修改该状态，需要确保当一个线程修改了对象状态后，其他线程能够看到发生的状态变化。
2.可见性错误是指当读操作与写操作在不同的线程中执行时，我们无法确保执行读操作的线程能实时的看到其他线程写入的值，有时甚至根本不可能的事。
3.我们可以通过同步来保证对象被安全的发布。除此之外我们也可以使用一种更加轻量级的volatile变量。
```

#### 2.2volatile关键字

```java
	java提供了一种稍弱的同步机制，即volatile变量，用来确保将变量的更新操作通知到其他线程。可以将volatile看作一个轻量级的锁。但是又与锁有些不同：
	1）对于多线程不是一种互斥关系，而sychronize锁对于线程是互斥关系；
	2）不能保证变量状态的“原子性”操作；
```

```java
/*
 * 一、volatile 关键字：当多个线程进行操作共享数据时，可以保证内存中的数据可见。
 *                   相较于 synchronized 是一种较为轻量级的同步策略。
 * 
 * 注意：
 * 1. volatile 不具备“互斥性”
 * 2. volatile 不能保证变量的“原子性”
 */
public class TestVolatile {
   
   public static void main(String[] args) {
      ThreadDemo td = new ThreadDemo();
      new Thread(td).start();
      
      while(true){
         if(td.isFlag()){
            System.out.println("------------------");
            break;
         }
      }
      
   }

}

class ThreadDemo implements Runnable {

   private volatile boolean flag = false;

   @Override
   public void run() {
      
      try {
         Thread.sleep(200);
      } catch (InterruptedException e) {
      }

      flag = true;
      
      System.out.println("flag=" + isFlag());

   }

   public boolean isFlag() {
      return flag;
   }

   public void setFlag(boolean flag) {
      this.flag = flag;
   }

}

//说明：1.对于flag是共享变量，共有两个线程操作它，thread线程进行写操作，main线程进行读操作；
//		2.flag是在主存中，而每个线程操作时会将共享变量首先放入自己线程的内存中取进行操作，操作完成后再放回到主存中，因此造成内存不可见，如果没有volatile关键字的话，main线程操作的flag变量一直都是false；
//		3.volatile关键字保证每次变量更改时通知到其他线程进行及时更新，因此解决了内存可见性。
```

### 3.原子变量-CAS算法

#### 3.1CAS算法

```java
1. CAS (Compare-And-Swap) 是一种硬件对并发的支持，针对多处理器操作而设计的处理器中的一种特殊指令，用于管理对共享数据的并发访问；
2.CAS 是一种无锁的非阻塞算法的实现。
3.CAS包含了3个操作数：
	1）需要读写的内存值；
	2）进行比较的值A；//设置新值时读取的内存值，看是否被更改了
	3）拟写入的新值B；
4.当且仅当V值等于A值时，CAS通过原子方式用新值B来更新V的值，否则不会执行任何操作。
```

模拟CAS（CompareAndSwap）算法代码：

```java
/*
 * 模拟 CAS 算法
 */
public class TestCompareAndSwap {

	public static void main(String[] args) {
		final CompareAndSwap cas = new CompareAndSwap();
		
		for (int i = 0; i < 10; i++) {
			new Thread(new Runnable() {
				
				@Override
				public void run() {
					int expectedValue = cas.get(); //预估值，即是设置新值之前再次读取的内存值
					boolean b = cas.compareAndSet(expectedValue, (int)(Math.random() * 101));
					System.out.println(b);
				}
			}).start();
		}
		
	}
	
}

class CompareAndSwap{
	private int value;
	
	//获取内存值
	public synchronized int get(){
		return value;
	}
	
	//比较
	public synchronized int compareAndSwap(int expectedValue, int newValue){
		int oldValue = value;
		
		if(oldValue == expectedValue){
			this.value = newValue;
		}
		
		return oldValue;
	}
	
	//设置
	public synchronized boolean compareAndSet(int expectedValue, int newValue){
		return expectedValue == compareAndSwap(expectedValue, newValue);
	}
}

```



#### 3.2原子变量

```java
1.类的小工具包，支持在单个变量上解除锁的线程安全编程。事实上，此包中的类可将 volatile 值、字段和数组元素的概念扩展到那些也提供原子条件更新操作的类。
2.类 AtomicBoolean、AtomicInteger、AtomicLong 和 AtomicReference 的实例各自提供对相应类型单个变量的访问和更新。每个类也为该类型提供适当的实用工具方法。
3.AtomicIntegerArray、AtomicLongArray 和 AtomicReferenceArray 类进一步扩展了原子操作，对这些类型的数组提供了支持。这些类在为其数组元素提供 volatile 访问语义方面也引人注目，这对于普通数组来说是不受支持的。
4.核心方法：boolean compareAndSet(expectedValue, updateValue)；
5.java.util.concurrent.atomic 包下提供了一些原子操作的常用类:
 AtomicBoolean 、AtomicInteger 、AtomicLong 、 AtomicReference
 AtomicIntegerArray 、AtomicLongArray
 AtomicMarkableReference
 AtomicReferenceArray
 AtomicStampedReference
```

#### 3.3代码说明

```java
/*
 * 一、i++ 的原子性问题：i++ 的操作实际上分为三个步骤“读-改-写”
 * 		  int i = 10;
 * 		  i = i++; //10
 * 
 * 		  int temp = i;
 * 		  i = i + 1;
 * 		  i = temp;
 * 
 * 二、原子变量：在 java.util.concurrent.atomic 包下提供了一些原子变量。
 * 		1. volatile 保证内存可见性
 * 		2. CAS（Compare-And-Swap） 算法保证数据变量的原子性
 * 			CAS 算法是硬件对于并发操作的支持
 * 			CAS 包含了三个操作数：
 * 			①内存值  V
 * 			②预估值  A
 * 			③更新值  B
 * 			当且仅当 V == A 时， V = B; 否则，不会执行任何操作。
 */
public class TestAtomicDemo {
	public static void main(String[] args) {
		AtomicDemo ad = new AtomicDemo();
		
		for (int i = 0; i < 10; i++) {
			new Thread(ad).start();
		}
	}
}

class AtomicDemo implements Runnable{
	//如果两个线程前后读取了变量，第一个线程进行++操作后，写入，第二个线程还是在原来基础上++,因此写入的值都是一样的，导致原子性问题；
//	private volatile int serialNumber = 0;
	private AtomicInteger serialNumber = new AtomicInteger(0);
	@Override
	public void run() {
		try {
			Thread.sleep(200);
		} catch (InterruptedException e) {
		}
		System.out.println(getSerialNumber());
	}
	public int getSerialNumber(){
		return serialNumber.getAndIncrement();
	}
}
```

### 4.ConcurrentHashMap

```java 
1.java5.0在java.util.concurrent包中提供了多种并发容器类来改进同步容器的性能；
2.ConcurrentHashMap同步容器类是java5增加的一个线程安全的哈希表。对于多线程的操作，介于HashMap和HashTable之间。内部采用锁分段机制替代HashTable的独占锁。进而提高性能；
3.此包还提供了设计用于多线程上下文中的Collection实现：
	ConcurrentHashMap/ConcurrentSkipListMap/ConcurrentSkipListSet/CopyOnWriteArrayList和CopyOnWriteArraySet.
    *当期望许多线程访问一个给定Collection时，ConcurrentHashMap通常优于同步的HashMap, ConcurrentSkipListMap通常优于同步的TreeMap;
	*当期望的读数和遍历远远大于列表的更新数时，CopyOnWriteArrayList优于同步的ArrayList.
```

### 5.CountDownLatch(闭锁)

```java
1.CountDownLatch是一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。
2.闭锁可以延迟一个线程的进度直到其到达终止状态，闭锁可以用来确保某些活动直到其他活动都完成才继续执行。
	应用场景：
	1）确保某个计算在其需要的所有资源都被初始化之后才继续执行；
	2）确保某个服务在其依赖的所有其他服务都已经启动之后才启动；
	3）等待直到某个操作所有参与者都准备就绪再继续执行；
```

代码说明：

```java
/**
 * 说明：1.如果不用闭锁是不能计算出所有线程运行完成时间的，因为计算运行时间的是主线程，和我们创建的50个线程是并行执行的
 *       2.闭锁CountDownLatch使用步骤：
 *          1）创建一个有初始值的CountDownLatch对象，初始值和创建的线程数相等；
 *          2）将CountDownLatch对象作为参数，传入要进行多线程执行类的构造方法中；
 *          3）每有一个线程进入后，调用countDownLatch.countDown();方法减一直至所有线程加入，则countDownLatch的值减为0
 *          4）所有线程加入后，调用CountDownLatch的await()方法，让主线程进行等待加入CountDownLatch的线程执行完后再执行主线程；
 **/
public class TestCountDownLatch {
    public static void main(String[] args) {
        //1）创建一个有初始值的CountDownLatch对象，初始值和创建的线程数相等；
        CountDownLatch la = new CountDownLatch(50);
        //2）将CountDownLatch对象作为参数，传入要进行多线程执行类的构造方法中；
        CountDownDemo countDownDemo = new CountDownDemo(la);

        long start = System.currentTimeMillis();

        for(int i = 0; i < 50; i++){
            new Thread(countDownDemo).start();
        }
        try {
            //4）所有线程加入后，调用CountDownLatch的await()方法，让主线程进行等待加入CountDownLatch的线程执行完后再执行主线程；
            la.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        long end = System.currentTimeMillis();
        System.out.println("耗费的时间是" + (end - start));
    }
}

class CountDownDemo implements Runnable{
    private CountDownLatch countDownLatch;
    public CountDownDemo(CountDownLatch countDownLatch){
        this.countDownLatch = countDownLatch;
    }
    @Override
    public void run() {
        try {
            for(int i = 0; i < 5000; i++){
                if(i % 2 == 0){
                    System.out.println(i);
                }
            }
        }finally {
            //3）每有一个线程进入后，调用countDownLatch.countDown();方法减一直至所有线程加入，则countDownLatch的值减为0
            countDownLatch.countDown();
        }

    }
}
```

### 6.线程的创建方式三：实现Callable接口

```java
1.java5.0在java.util.concurrent包中提供了一个新的创建线程的方式：实现Callable接口；
2.Callable接口类似于Runnable,两者都是为那些其实例可能被另一个线程执行的类设计的。但是Runnable不会反回结果，并且无法抛出经过检查的异常。
3.Callable需要依赖FutureTask, FutureTask有闭锁的功能。

//创建步骤：
1.创建一个类实现Callable接口，重写call()方法，相当于Runnable接口中run方法，不同的是call方法有返回值；
2.创建一个实现Callable接口类的实现类td;
3.创建一个FutureTask实现类ft，将上述td作为参数传入构造方法中；
4.将FutureTask实现类作为参数传入Thread的构造方法中，创建Thread实现类；thread.start()启动线程；
5.ft.get();可以获取call()方法的返回值；
//注意：由于FutureTask类有闭锁的功能，因此，在此多线程执行完之前，主线程一值再等待；
//应用场景：如果一个类中有大量的计算，速度太慢，且主线程必须拿到计算结果才能继续进行，则可以使用实现Callable接口的线程；   如果主线程不需要依赖计算结果的，则可以直接使用实现Runnable接口的线程；
```

代码实现：

```java
/*
 * 一、创建执行线程的方式三：实现 Callable 接口。 相较于实现 Runnable 接口的方式，方法可以有返回值，并且可以抛出异常。
 * 
 * 二、执行 Callable 方式，需要 FutureTask 实现类的支持，用于接收运算结果。  FutureTask 是  Future 接口的实现类
 */
public class TestCallable {
	
	public static void main(String[] args) {
		ThreadDemo td = new ThreadDemo();
		
		//1.执行 Callable 方式，需要 FutureTask 实现类的支持，用于接收运算结果。
		FutureTask<Integer> result = new FutureTask<>(td);
		
		new Thread(result).start();
		
		//2.接收线程运算后的结果
		try {
			Integer sum = result.get();  //FutureTask 可用于 闭锁
			System.out.println(sum);
			System.out.println("------------------------------------");
		} catch (InterruptedException | ExecutionException e) {
			e.printStackTrace();
		}
	}

}

class ThreadDemo implements Callable<Integer>{
	@Override
	public Integer call() throws Exception {
		int sum = 0;
		for (int i = 0; i <= 100000; i++) {
			sum += i;
		}
		return sum;
	}	
}
```

