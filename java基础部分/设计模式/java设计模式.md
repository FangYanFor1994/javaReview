## 一、设计模式的分类

```java
总体来说设计模式分为三大类：
1.创建型模式：（共五种）
	工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
2.结构型模式：（共七种）
	适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
3.行为型模式：（共十一种）
	策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。
```

## 二、设计模式的六大原则

```java
1.开闭原则（open close principle）
	开闭原则就是说 /*对外扩展开发，对内修改关闭*/。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个 热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。
2.里氏代换原则（Liskov Subsitution Principle）
	里氏代换原则（Liskov Subsitution Principle LSP）是面向对象设计的基本原则之一。里氏代换原则中说，/*任何基类可以出现的地方，子类一定可以出现*/。LSP是集成复用的基石，只有当衍生类可以替换调基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对“开闭原则”的补充。实现“开闭原则”的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对抽象化的具体步骤的规范。
3.依赖倒转原则（Dependence Inversion Principle）
	这个是开闭原则的基础，具体内容：/*针对接口编程，依赖于抽象而不依赖于具体。*/
4.接口隔离原则（Interface Segregation Principle）
	/*使用多个隔离的接口，比使用单个接口要好。还是一个降低类之间的耦合度的意思，*/从这我们可以看出，其实设计模式就是一个软件的设计思想，从大型软件架构出发，为了升级和维护方便。所以上文中多次出现：降低依赖，降低耦合。
5.迪米特法则（最少知道原则）（Demeter Principle）
	为什么叫最少知道原则，就是说：/*一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。*/
6.合成复用原则（Composite Reuse Principle）
	原则是尽量使用合成/聚合的方式，而不是使用继承。
```

## 三、java中23中设计模式

### 1.工厂方法模式（Factory Method）

#### 1.1普通工厂模式

就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。首先看下关系图：

![1590652171028](E:\GIT\学习资料\java基础部分\设计模式\assets\1590652171028.png)

举例如下：（我们举一个用email和手机发送消息的例子）

```java
/**
 * @描述:首先创建二者共同接口
 */
public interface Sender {
    public void send();
}

/**
 * @描述:创建两个实现类
 */
public class MailSender implements Sender {
    @Override
    public void send() {
        System.out.println("这是eMail发送的消息");
    }
}

public class PhoneSender implements Sender {
    @Override
    public void send() {
        System.out.println("这是手机发送的消息");
    }
}

/**
 * @描述:创建工厂类
 */
public class SendFactory {
    public Sender produce(String type) {
        if ("mail".equals(type)) {
            return new MailSender();
        } else if ("phone".equals(type)) {
            return new PhoneSender();
        } else {
            System.out.println("请输入正确的类型");
            return null;
        }
    }
}

/**
 * @描述:测试
 */
public class TestDemo {
    @Test
    public void test() {
        //获取工厂
        SendFactory sendFactory = new SendFactory();
        //根据发送类型获取消息发送实例
        Sender sender = sendFactory.produce("phone");
        sender.send();
    }
}
```

#### 1.2多个工厂方法模式

是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，二多个工厂方法模式是提供多个工厂方法，分别创建对象。关系图如下：

![1590652555429](E:\GIT\学习资料\java基础部分\设计模式\assets\1590652555429.png)

将上面的代码做如下修改，改动下SendFactory类就行，如下：

```java
/**
 * @描述:修改后的SendFactory
 */
public class SendFactory {
    
    public Sender produceMail() {
        return new MailSender();
    }

    public Sender producePhone() {
        return new PhoneSender();
    }
}

//测试
 @Test
    public void test1() {
        SendFactory sendFactory = new SendFactory();
        Sender sender = sendFactory.produceMail();
        sender.send();
    }
```

#### 1.3静态工厂方法模式

将上面的多个工厂方法模式里的方法置为静态的，不需要创建实例，直接调用即可。

```java
/**
 * @描述:修改后的SendFactory 静态的工厂方法
 */
public class SendFactory {
    
    public static Sender produceMail() {
        return new MailSender();
    }

    public static Sender producePhone() {
        return new PhoneSender();
    }
}

//测试
 @Test
    public void test1() {
        Sender sender = SendFactory.produceMail();
        sender.send();
    }
```

#### 1.4工厂方法模式总结

```java
	总体来说，工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。在以上三种模式中，第一种如果传入的字符串有误，不能正确创建对象，第三种相对于第二种，不需要实例化工厂，所以，大多数情况下，我们会选用第三种--静态工厂方法模式。
```

### 2.抽象工厂模式（Abstract Factory）

工厂方法模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则，所以，从设计角度考虑，有一定的问题，如何解决？  就用到了抽象工厂模式，创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。 图如下：

![1590655304577](E:\GIT\学习资料\java基础部分\设计模式\assets\1590655304577.png)

代码示例：

- 创建发消息接口

```java
public interface Sender {
    void send();
}
```

- 手机发消息和mail发消息实体类

```java
public class MailSender implements Sender {
    @Override
    public void send() {
        System.out.println("这是用email发的消息");
    }
}
```

```java
public class PhoneSender implements Sender {
    @Override
    public void send() {
        System.out.println("这是用手机发的消息");
    }
}
```

- 工厂接口

```java
public interface Factory {
    Sender produce();
}
```

- 两个工厂实现类

```java
public class MailFactory implements Factory {
    @Override
    public Sender produce() {
        return new MailSender();
    }
}
```

```java
public class PhoneFactory implements Factory {
    @Override
    public Sender produce() {
        return new PhoneSender();
    }
}
```

- 测试

```java
public class TestDemo {
    @Test
    public void test() {
        MailFactory mailFactory = new MailFactory();
        Sender sender = mailFactory.produce();
        sender.send();
    }
}
```

- 优点

  这个模式的好处是，如果你现在想增加一个功能：用电脑发送消息，则只需要做一个实现类，实现Sender接口，同时做一个工厂类，实现Factory接口，就可以了，无需去改动现成的代码。这样做，拓展性较好。

### 3.单例模式（Singleton）

单例对象（Singleton）是一种常见的设计模式。在java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。这样的模式有几个好处：

1. 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。
2. 省去了new 操作符，降低了系统内存的使用频率，减轻GC压力。
3. 有些类如交易所的核心交易引擎，控制着交易流程，如果这个类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱做一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。

#### 3.1懒汉式（线程不安全）

```java
public class Singleton1 {
    private static Singleton1 singleton = null;

    private Singleton1() {
    }

    public static Singleton1 getInstance() {
        if (singleton == null) {
            singleton = new Singleton1();
        }
        return singleton;
    }
}

//优点：懒加载启动快，资源占用小，使用时才实例化，无锁。
//缺点：非线程安全。
```

#### 3.2懒汉式（线程安全）

```java
public class Singleton1 {
    //1.定义一个变量来存储创建好的类实例
    private static Singleton1 singleton = null;

    //2.私有化构造方法，只在内部控制创建实例
    private Singleton1() {
    }

    //3.获取类实例方法
    public static synchronized Singleton1 getInstance() {
        if (singleton == null) {
            singleton = new Singleton1();
        }
        return singleton;
    }
}

//优点：同上，但加锁了。
//缺点：synchronized为独占排它锁，并发性能差。即使在创建成功以后，获取实例仍然是串行化操作。
```

#### 3.3.懒汉式（双重校验锁 Double Check Lock）

```java 
public class Singleton1 {

    //对保存实例的变量增加volatile关键字
    private volatile static Singleton1 singleton = null;

    //2.私有化构造方法，只在内部控制创建实例
    private Singleton1() {
    }

    //3.获取类实例方法
    public static Singleton1 getInstance() {
        //先检查实例是否存在，如果不存在才进入下面的同步块
        if (singleton == null) {
            synchronized (Singleton1.class) {
                if (singleton == null) {
                    singleton = new Singleton1();
                }
            }
        }
        return singleton;
    }
}

//优点：懒加载，线程安全。 解决了3.2中创建实例后仍然是串行化操作问题。
注：实例必须有volatile关键字修饰，保证初始化完全。
```

#### 3.4饿汉式

```java
public class Singleton {

    //1.直接创建实例，只会创建一次
    private static Singleton singleton = new Singleton();

    //2.私有化构造方法，只在内部控制创建实例
    private Singleton() {
    }

    //3.获取类实例方法
    public static Singleton getInstance() {
        return singleton;
    }
}

//优点：饿汉式天生是线程安全的，使用时没有延迟。
//缺点：启动时即创建实例，启动慢，有可能造成资源浪费。
```

#### 3.5Holder模式（推荐）

```java
public class Singleton {

    /**
     * 静态内部类
     * 1.延迟加载：静态内部类不随外部类的加载而加载，而是在首次调用的时候加载
     * 2.单例唯一：静态仅在调用时加载一次，因此只创建一个实例
     */
    private static class SingletonHolder {
        private static Singleton instance = new Singleton();
    }

    /**
     * 私有化构造器
     */
    private Singleton(){
    }

    /**
     * 获取实例
     */
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}

//优点：将懒加载和线程安全完美结合的一种方式（无锁）。（推荐！！！）
```

