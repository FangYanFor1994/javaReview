## 一、设计模式的分类

```java
总体来说设计模式分为三大类：
1.创建型模式：（共五种）
	工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式。
2.结构型模式：（共七种）
	适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
3.行为型模式：（共十一种）
	策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。
	
//一般比较常用需要掌握的有：
	单例模式	工厂模式	抽象工厂模式	建造者模式	适配器模式	装饰器模式	策略模式	代理模式
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

**选用建议**

```
一般不建议使用3.1和3.2懒汉模式
不要求懒加载的话建议使用3.4饿汉式
在明确实现lazy loading效果时，才会使用3.5holder模式
如果有特殊需求，可以考虑使用3.3双检锁方式。
```

### 4.建造者模式（Builder Pattern ）

```java
建造者模式使用多个简单的对象一步步构建成一个复杂的对象。这种类型的设计模式属于创建型模式，它提供一种创建对象的最佳方式。
一个Builder类会一步一步构造最终的对象。该Builder类是独立于其他对象的。
1.介绍：
	1）意图：将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示；
	2）主要解决：主要解决在软件系统中，有时候面临着"一个复杂对象"的创建工作，其通常由各个部分的子对象用一定的算法构成；由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是将它们组合在一起的算法却相对稳定。
	3）使用场景：需要生成的对象具有复杂的内部结构。   需要生成的对象内部属性本身相互依赖。
	4）与工厂模式的区别：建造者模式更加关注与零件装配的顺序。
2.优缺点：
	优点：1）建造者独立，易扩展。 2）便于控制细节风险。
	缺点：1）产品必须有共同点，范围有限制。 2）如内部变化复杂，会有很多建造类。
3.实现：
	我们假设一个快餐店的商业案例，其中，一个典型的套餐可以是一个汉堡（Burger）和一杯冷饮（Cold drink）。汉堡（Burger）可以是素食汉堡（Veg Burger）或鸡肉汉堡（Chicken Burger），它们是包在纸盒中。冷饮（Cold drink）可以是可口可乐（coke）或百事可乐（pepsi），它们是装在瓶子中。
我们将创建一个表示食物条目（比如汉堡和冷饮）的 Item 接口和实现 Item 接口的实体类，以及一个表示食物包装的 Packing 接口和实现 Packing 接口的实体类，汉堡是包在纸盒中，冷饮是装在瓶子中。
然后我们创建一个 Meal 类，带有 Item 的 ArrayList 和一个通过结合 Item 来创建不同类型的 Meal 对象的 MealBuilder。BuilderPatternDemo，我们的演示类使用 MealBuilder 来创建一个 Meal。
```

**代码实现**

*步骤1*  创建一个表示食物条目和食物包装的接口。 

```java
/**
 * @描述: 创建食物条目的接口
 */
public interface Item {
    public String name();
    public Packing packing(); //包装
    public float price();
}

/**
 * @描述: 包装接口
 */
public interface Packing {
    public String pack();
}
```

*步骤2*  创建实现 Packing 接口的实体类。 

```java
/**
 * @描述: 纸盒包装 实现包装接口
 */
public class Wrapper implements Packing {
    @Override
    public String pack() {
        return "wrapper";
    }
}

/**
 * @描述: 瓶子装实现包装接口
 */
public class Bottle implements Packing {
    @Override
    public String pack() {
        return "bottle";
    }
}
```

*步骤3*  创建实现 Item 接口的抽象类，该类提供了默认的功能。 

```java
/**
 * @描述: 汉堡抽象类实现食物条目Item接口
 */
public abstract class Burger implements Item {
    @Override
    public Packing packing() {
        return new Wrapper();
    }
    @Override
    public abstract float price();
}

/**
 * @描述: 冷饮抽象类实现食物条目Item接口
 */
public abstract class ColdDrink implements Item {
    @Override
    public Packing packing() {
        return new Bottle();
    }
    @Override
    public abstract float price();
}
```

*步骤4*  创建扩展了 Burger 和 ColdDrink 的实体类。 

```java
/**
 * @描述: 蔬菜汉堡 继承汉堡类
 */
public class VegBurger extends Burger {
    @Override
    public String name() {
        return "veg burger";
    }
    @Override
    public float price() {
        return 25.0f;
    }
}

/**
 * @描述: 鸡肉汉堡继承汉堡类
 */
public class ChickenBurger extends Burger {
    @Override
    public String name() {
        return "chiken burger";
    }
    @Override
    public float price() {
        return 50.5f;
    }
}

/**
 * @描述: 可乐继承冷饮类
 */
public class Coke extends ColdDrink {
    @Override
    public String name() {
        return "coke";
    }
    @Override
    public float price() {
        return 30.0f;
    }
}

/**
 * @描述: 百世 继承冷饮类
 */
public class Pepsi extends ColdDrink {
    @Override
    public String name() {
        return "pepsi";
    }
    @Override
    public float price() {
        return 35.0f;
    }
}
```

步骤5   创建一个 Meal 类，带有上面定义的 Item 对象。 

```java
/**
 * @描述: 创建 餐Meal类，带上上面定义的Item对象
 */
public class Meal {
    private List<Item> items = new ArrayList<Item>();
    
    public void addItem(Item item) {
        items.add(item);
    }
    
    public float getCost() { //获取总消费
        float cost = 0.0f;
        for (Item item : items) {
            cost += item.price();
        }
        return cost;
    }

    public void showItems() {
        for (Item item : items) {
            System.out.print("Item:" + item.name());
            System.out.print(", Packing:" + item.packing().pack());
            System.out.println(", Price:" + item.price());
        }
    }
}
```

*步骤6* 创建一个 MealBuilder 类，实际的 builder 类负责创建 Meal 对象。 

```java
/**
 * @描述: 创建一个MealBuilder类，实际的builder类负责创建Meal对象
 */
public class MealBuilder {

    public Meal prepareVegMeal() {
        Meal meal = new Meal();
        meal.addItem(new VegBurger());
        meal.addItem(new Coke());
        return meal;
    }

    public Meal prepareNonVegMeal() {
        Meal meal = new Meal();
        meal.addItem(new ChickenBurger());
        meal.addItem(new Pepsi());
        return meal;
    }
}
```

*步骤7* 测试

```java
public class BuilderPatternDemo {
    public static void main(String[] args) {
        MealBuilder mealBuilder = new MealBuilder();

        Meal vegMeal = mealBuilder.prepareVegMeal();
        System.out.println("Veg Meal");
        vegMeal.showItems();
        System.out.println("Total cost:"+ vegMeal.getCost());

        Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
        System.out.println("\n\nNon-Veg Meal");
        nonVegMeal.showItems();
        System.out.println("Total Cost:" + nonVegMeal.getCost());
    }
}


//执行结果
Veg Meal
Item:veg burger, Packing:wrapper, Price:25.0
Item:coke, Packing:bottle, Price:30.0
Total cost:55.0


Non-Veg Meal
Item:chiken burger, Packing:wrapper, Price:50.5
Item:pepsi, Packing:bottle, Price:35.0
Total Cost:85.5
```

### 5.原型模式（Prototype Pattern）

```java 
原型模式是用于创建重复的对象，同时又能保证性能。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
这种模式是实现了一个原型接口，该接口用于创建当前对象的克隆。当直接创建对象的代价比较大时，采用这种模式。例如：一个对象需要在一个高代价的数据库操作之后被创建。我们可以缓存该对象，在下一个请求时返回它的克隆，在需要的时候更新数据库，以此来减少数据库调用。

1.意图：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。
2.使用场景:1)资源优化场景。 2）类初始化需要消耗非常多的资源，这个资源包括数据、硬件等资源。 3）性能和安全要求的场景。 4）通过new产生一个对象需要非常繁琐的数据准备或访问权限，则可以使用原型模式。 5）一个对象多个修改者的场景。 6）一个对象需要提供给其他对象访问，而且各个调用者可能都需要修改其值时，可以考虑使用原型模式拷贝多个对象供调用者使用。 	7）在实际项目中，原型模式很少单独出现，一般是和工厂方法模式一起出现，通过clone方法创建一个对象，然后由工厂方法提供给调用者。原型模式已经与java融为浑然一体，大家可以随手拿来使用。
3.优点：
	1）性能提高；
	2)避免构造函数的出现；
4.缺点：
	1）必须实现Cloneable接口；
	2）配备克隆方法需要对类的功能进行通盘考虑，这对于全新的类不是很难的，但对于已有的类不一定很容易，特别当一个类引用不支持串行化的间接对象，或者引用含有循环结构的时候。
5.注意事项：与通过对一个类进行实例化来构造新对象不同的是，原型模式是通过拷贝一个现有对象的。浅拷贝实现Cloneable接口，重写，深拷贝是通过实现Serializable读取二进制流。
```

**实现**

我们将创建一个抽象类Shape和扩展了Shape类的实体类。下一步定义ShapeCache,该类把shape对象存储在一个Hashtable中，并在请求时返回他们的克隆。

演示类使用ShapeCache类来获取shape对象。

![1590993675580](E:\GIT\学习资料\java基础部分\设计模式\assets\1590993675580.png)

步骤一、创建一个实现Cloneable接口的抽象类

```java
public abstract class Shape implements Cloneable{
    private String id;
    protected String type;

    abstract void draw();

    public String getType() {
        return type;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }

    public Object clone() {
        Object clone = null;
        try{
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return clone;
    }

}
```

步骤二、创建扩展了上面抽象类的实体类

```java
public class Rectangle extends Shape {
    public Rectangle(){
        type = "Rectangle";
    }

    @Override
    void draw() {
        System.out.println("Rectangle中draw()方法。。。");
    }
}
```

```java
public class Square extends Shape {
    public Square() {
        type = "Square";
    }

    @Override
    void draw() {
        System.out.println("Square中的draw()方法。。。");
    }
}
```

```java
public class Circle extends Shape {
    public Circle() {
        type = "Circle";
    }

    @Override
    void draw() {
        System.out.println("Circle中的draw()方法。。。");
    }
}
```

步骤三、（核心步骤）创建一个类，从数据库获取实体类，并把他们存储在一个Hashtable中。

```java
public class ShapeCache {
    //创建一个hashMap，用来存储Shape实体类
    private static HashMap<String, Shape> shapeMap = new HashMap<String, Shape>();

    //提供获取对象的方法
    public static Shape getShape(String shapeId) {
        Shape shape = shapeMap.get(shapeId);
        return (Shape) shape.clone();  //这里获取对象不是直接从map取出返回，而是返回克隆的对象
    }

    //模拟从数据库中查询三种图形并放入hashMap中
    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);

        Square square = new Square();
        square.setId("2");
        shapeMap.put(square.getId(), square);

        Rectangle rectangle = new Rectangle();
        rectangle.setId("3");
        shapeMap.put(rectangle.getId(), rectangle);
    }


    @Test
    public void test() {
        HashMap<String, Circle> map = new HashMap<String, Circle>();
        Circle circle = new Circle();
        map.put("circle", circle);
        //Circle circle1 = (Circle) circle.clone();
        System.out.println("第一次" + map.get("circle"));
        System.out.println("第二次" + map.get("circle"));
    }
}
```

步骤四、测试使用 *ShapeCache* 类来获取存储在 *Hashtable* 中的形状的克隆。 

```java
public class PrototypePatternDemo {
    public static void main(String[] args) {
        //模拟从数据中查询的数据，封装到map中
        ShapeCache.loadCache();

        Shape shape = ShapeCache.getShape("1");
        System.out.println("shape:" + shape.getType());

        Shape shape1 = ShapeCache.getShape("2");
        System.out.println("shape:" + shape1.getType());

        Shape shape2 = ShapeCache.getShape("3");
        System.out.println("shape:" + shape2.getType());
    }
}

//总结：原型模式的核心其实就是第三步，将已有对象保存在hashtable中，需要时就clone一个返回代替new一个节约资源。    问：为什么不直接返回hashtable里面的对象而要克隆呢？  问的好。每次都直接从hashtable里面返回其实是一个同一个对象，属于单例模式。克隆出来的才是原型模式。
```

### 6.适配器模式（Adapter  Pattern）

```java 
	适配器模式是作为两个不兼容的接口之间的桥梁。这种设计模式属于结构型模式，它结合了两个独立接口的功能。这种模式涉及到一个单一的类，该类负责加入独立的或不兼容的接口功能（该类即为适配器类）。
1.意图：将一个类的接口转换为客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
2.关键代码：适配器继承或依赖已有对象，实现想要的目标接口。
3.优点：	
	1）可以让任何没有关联的类一起运行。
	2）提高了类的复用。
	3）增加了类的透明度。
	4）灵活性好。
4.缺点：
	1）过多的使用适配器会让系统非常的凌乱，不易整体进行把握。比如明明看到调用的是A接口，其内部被适配成了B接口的实现，一个系统如果出现太多这种情况，无异于异常灾难。因此如果不是很有必要，可以不适用适配器，而是直接对系统进行重构。
	2）由于java至多继承一个类，所以至多只能适配一个适配者类，而且目标类必须是抽象类。
5.使用场景：
	有动机的修改一个正常运行的系统的接口，这时应该考虑使用适配器模式。
6.注意事项：适配器不是在详细设计时添加的，而是解决正在服役的项目问题。

//实现：
	我们有一个MediaPlayer接口和一个实现MediaPlayer接口的实体类AudioPlayer.默认情况下，AudioPlayer可以播放mp3格式音频文件。
	我们还有一个接口AdvanceMediaPlayer和实现了AdvanceMediaPlayer接口的实体类。该类可以播放vlc和mp4格式的文件。
	我们想让AudioPlayer播放其他格式的音频文件。为了实现这个功能，我们需要创建一个实现了MediaPlayer接口的适配器类MediaAdapter，并使用AdvancedMediaPlayer对象来播放所需的格式。
	AudioPlayer使用适配器类MediaAdapter传递所需音乐类型，不需要知道能播放所需格式音频的实际类。
```

![1591324509003](E:\GIT\学习资料\java基础部分\设计模式\assets\1591324509003.png)

**代码实现**

步骤一、为媒体播放器和更高级的媒体播放器创建接口

```java
/**
*媒体播放器接口
*/
public interface MediaPlayer {
    public void play(String audioType, String name);
}

/**
*更高级的媒体播放器接口
*/
public interface AdvancedMediaPlayer {
    public void playVlc(String fileName);
    public void playMp4(String fileName);
}
```

步骤二、创建实现了AdvancedMediaPlayer接口的实体类

```java
/**
*播放mp4的实体类
*/
public class Mp4Player implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        //nothing to do
    }

    @Override
    public void playMp4(String fileName) {
        System.out.println("play mp4 file,name:" + fileName);
    }
}

/**
*播放vlc的实体类
*/
public class VlcPlayer implements AdvancedMediaPlayer {
    @Override
    public void playVlc(String fileName) {
        System.out.println("play vlc file,name:" + fileName);
    }

    @Override
    public void playMp4(String fileName) {
        //nothing to do
    }
}
```

步骤三、创建实现MediaPlayer接口的适配器类（核心）

```java
/**
*适配器类特点：
*1.实现目标接口
*2.依赖实现其他功能的接口实体类
*3.重写接口方法，实现其他功能
*/
public class MediaAdapter implements MediaPlayer {

    AdvancedMediaPlayer advancedMediaPlayer;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer = new VlcPlayer();
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer = new Mp4Player();
        }
    }

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer.playVlc(fileName);
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer.playMp4(fileName);
        }
    }
}
```

步骤四、创建实现MediaPlayer接口的实体类（核心）

```java
/**
 * 1.该类为目标接口的实现类
 * 2.该类依赖了适配器类，通过适配器类来实现其他接口的功能。又因为实现了目标接口，因此也可以使用本来的功能
 */
public class AudioPlayer implements MediaPlayer {

    MediaAdapter mediaAdapter;

    @Override
    public void play(String audioType, String fileName) {
        //播放mp3音乐文件的内置支持
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("play mp3 file , name:" + fileName);
        } //mediaAdapter 提供了播放其他文件格式的支持
        else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")){
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        }else{
            System.out.println("不支持该格式的文件播放");
        }
    }
}
```

步骤五、测试

```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();
        audioPlayer.play("mp3", "月亮之上");
        audioPlayer.play("mp4", "无间道");
        audioPlayer.play("vlc", "教父");
        audioPlayer.play("VCD", "十二大美女");
    }
}
//这样我们就可以通过AudioPlayer实现不同文件类型的播放
```

### 7.装饰模式（Decorator）

```java 
装饰模式就是给一个对象增加一些新的功能，而且是动态的。
要求：装饰对象和被装饰对象实现同一个接口。
	装饰对象持有被装饰对象的实例。
	
/**
 * @描述: 装饰对象和被装饰对象共同的接口
 */
public interface Sourceable {
    public void method();
}

/**
 * @描述: 被装饰类
 */
public class Source implements Sourceable {
    @Override
    public void method() {
        System.out.println("the original method!");
    }
}

/**
 * @描述: 装饰类，需要满足两个条件：
 *                  ①装饰对象和被装饰对象实现同一接口
 *                  ②装饰对象持有被装饰对象的实例
 */
public class Decorator implements Sourceable {
    private Source source;
    public Decorator(Source source){
        this.source = source;
    }
    @Override
    public void method() {
        //扩展新方法
        System.out.println("new methods 1");
        //调用原来的方法
        source.method();
        //扩展新方法
        System.out.println("new methods 2");
    }
}

/**
 * @描述: 测试类
 */
public class DecoratorTest {
    public static void main(String[] args) {
        Sourceable source = new Source();
        Sourceable decorator = new Decorator(source);
        decorator.method();
    }
}

/*
控制台输出结果：
new methods 1
the original method!
new methods 2
*/


//装饰器模式应用的场景：
	1.需要动态扩展一个类的功能；		
	2.动态的为一个对象增加功能还能动态撤销。（继承不能做到这一点，继承的功能是静态的，不能动态增删）
```

### 8.代理模式（Proxy）

```java
和装饰模式类似，对实现了共同接口的类方法改进。

/**
 * @描述: 共同接口
 */
public interface Sourceable {
    public void method();
}

/**
 * @描述:被代理类
 */
public class Source implements Sourceable {
    @Override
    public void method() {
        System.out.println("the original method");
    }
}

/**
 * @描述:代理类
 */
public class Proxy implements Sourceable {
    private Source source;

    public Proxy(){
        super();
        this.source = new Source();
    }

    @Override
    public void method() {
        before();
        source.method();
        after();
    }

    private void before(){
        System.out.println("the before method");
    }

    private void after(){
        System.out.println("the after method");
    }
}

/**
 * @描述:测试代理模式
 */
public class ProxyTest {
    public static void main(String[] args) {
        Proxy proxy = new Proxy();
        proxy.method();
    }
}


//代理模式的应用场景：
如果已有的方法在使用的时候需要对原有的方法进行改进，此时有两种办法：
1、修改原有的方法来适应。这样违反了“对扩展开放，对修改关闭”的原则。
2、就是采用一个代理类调用原有的方法，且对产生的结果进行控制。这种方法就是代理模式。
使用代理模式，可以将功能划分的更加清晰，有助于后期维护！
```

### 9.外观模式（Facade）

```java
1.意图：为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。
2.主要解决：降低访问复杂系统的内部子系统时的复杂度，简化客户端与之的接口。
3.例子:开启一个电脑，需要依次启动CPU、memory、disk等，提供一个外观类computer,启动computer的时候自动启动CPU、memory、disk。

/**
 * @描述:CPU
 */
public class CPU {
    public void strtup(){
        System.out.println("CPU startup...");
    }

    public void shutdown(){
        System.out.println("CPU shutdown...");
    }
}

/**
 * @描述:memory
 */
public class Memory {

    public void startup(){
        System.out.println("Memory startup...");
    }

    public void shutdown(){
        System.out.println("Memory shutdown...");
    }
}

/**
 * @描述:disk
 */
public class Disk {

    public void startup(){
        System.out.println("disk startup");
    }

    public void shutdown(){
        System.out.println("disk shutdown");
    }
}

/**
 * @描述:外观类，computer
 */
public class Computer {
    private CPU cpu;
    private Memory memory;
    private Disk disk;

    public Computer(){
        cpu = new CPU();
        memory = new Memory();
        disk = new Disk();
    }

    public void startup(){
        System.out.println("start the computer...");
        cpu.strtup();
        memory.startup();
        disk.startup();
        System.out.println("start computer finished...");
    }

    public void shutdown(){
        System.out.println("begin to close the computer...");
        cpu.shutdown();
        memory.shutdown();
        disk.shutdown();
        System.out.println("computer closed!");
    }
}

/**
 * @描述:外观模式测试类
 */
public class FacadeTest {
    public static void main(String[] args) {
        Computer computer = new Computer();
        computer.startup();
        computer.shutdown();
    }
}
```

### 10.桥接模式（Bridge）

```java
桥接模式就是把事物和其具体实现分开，使他们可以各自独立的变化。桥接的用意是：将抽象化与实现化解耦，使得二者可以独立变化，像我们常用的JDBC桥DriverManager一样，JDBC进行连接数据库的时候，在各个数据库之间进行切换，基本不需要动太多的代码，甚至丝毫不用动，原因就是JDBC提供统一接口，每个数据库提供各自的实现，用一个叫做数据库驱动的程序来桥接就行了。

/**
 * @描述:接口
 */
public interface Sourceable {
    public void method();
}

/**
 * @描述:实现类1	
 */
public class SourceSub1 implements Sourceable {
    @Override
    public void method() {
        System.out.println("this is the first sub!");
    }
}

/**
 * @描述:实现类2
 */
public class SourceSub2 implements Sourceable {
    @Override
    public void method() {
        System.out.println("this is the second sub!");
    }
}

/**
 * @描述:定义一个桥接，持有Sourceable的一个实例
 */
public class Bridge {
    private Sourceable source;

    public void method(){
        source.method();
    }

    public Sourceable getSource(){
        return source;
    }
    public void setSource(Sourceable source){
        this.source = source;
    }
}

/**
 * @描述:测试类
 */
public class BridgeTest {
    public static void main(String[] args) {
        Bridge bridge = new Bridge();

        //调用第一个对象
        Sourceable sourceSub1 = new SourceSub1();
        bridge.setSource(sourceSub1);
        bridge.method();

        //调用第二个对象
        Sourceable sourceSub2 = new SourceSub2();
        bridge.setSource(sourceSub2);
        bridge.method();
    }
}
```

