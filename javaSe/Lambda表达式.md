### 1.函数式接口

#### 1.1函数式接口初体验

```java
1.函数式接口：有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。
2.函数式接口可以被隐式转换为lambda表达式。
 示例：
 //如下定义一个函数式接口
 	@FunctionalInterface //标注为函数式接口，如果多于一个抽象方法就会报错
    interface GreetingService {
        void sayMessage(String message);
    }
    
//java8之前可以使用匿名内部类来实现该接口，如下：
GreetingService greetingService = new GreetingService(){
            @Override
            public void sayMessage(String message) {
                System.out.println("Hello " + message);
            }
        };

//java8以后可以使用lambda表达式来标识该接口的一个实现（必须为函数式接口）
GreetingService greetService1 = message -> System.out.println("Hello " + message);

//说明：lambda表达式中 “=” 右边的参数只能传给一个方法，因此如果不是函数式接口，则参数不知道该传给哪个方法； “->”右边的代码为函数式接口的抽象方法的重写实现。
```

#### 1.2函数式接口实例

```java
//说明：Predicate <T> 接口是一个函数式接口，它接受一个输入参数 T，返回一个布尔值结果。
public static void main(String args[]){
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);

        // Predicate<Integer> predicate = n -> true
        // n 是一个参数传递到 Predicate 接口的 test 方法
        // n 如果存在则 test 方法返回 true

        System.out.println("输出所有数据:");

        // 传递参数 n
        eval(list, n->true);

        // Predicate<Integer> predicate1 = n -> n%2 == 0
        // n 是一个参数传递到 Predicate 接口的 test 方法
        // 如果 n%2 为 0 test 方法返回 true

        System.out.println("输出所有偶数:");
        eval(list, n-> n%2 == 0 );

        // Predicate<Integer> predicate2 = n -> n > 3
        // n 是一个参数传递到 Predicate 接口的 test 方法
        // 如果 n 大于 3 test 方法返回 true

        System.out.println("输出大于 3 的所有数字:");
        eval(list, n-> n > 3 );
    }

    public static void eval(List<Integer> list, Predicate<Integer> predicate) {
        for(Integer n: list) {

            if(predicate.test(n)) {
                System.out.println(n + " ");
            }
        }
    }
```

### 2.lambda表达式

#### 2.1语法及特性

```java
1.语法
(parameters) -> expression
或
(parameters) -> {statements};

2.lambda表达式重要特性
	1）可选类型声明：不需要声明参数类型，编译期可统一识别参数值；
	2）可选参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号；
	3）可选大括号：如果主题包含一个语句，就不需要使用大括号；
	4）可选返回关键字：如果主题只有一个表达式返回值则编译器会自动返回值，大括号需要指明表达式返回了一个数值。
```

#### 2.2lambda表达式实例

Lambda表达式的简单例子：

```java
// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

lambda表达式实例

```java
public static void main(String[] args) {
        ProductController productController = new ProductController();
        //类型声明
        productController.test(10,5, (int n, int m) -> n*m);
        //不用声明类型
        productController.test(10,5,(n, m) -> n-m);
        //大括号里返回语句
        productController.test(10, 5, (n, m) -> {return n/m;});
        //参数不带括号（一个参数可以不带）
        GreetingService greet = message -> System.out.println("hello" + message);
        greet.sayMessage("fangyan");
        //参数带括号（多个参数必须带）
        GreetingService greet1 = (message) -> System.out.println("hello" + message);
        greet.sayMessage("zyy");
    }

    interface MathOperation {
        int operation(int a, int b);
    }
    interface GreetingService {
        void sayMessage(String message);
    }
    public void test(int i, int j, MathOperation mathOperation){
        System.out.println(mathOperation.operation(i,j));
    }

//总结：lambda表达式(parameters) -> {statement};表示一个函数式接口实现；其中，parameters是函数式接口的唯一抽象方法的参数，statement是唯一抽象方法的重写。
```

#### 2.3变量作用域

```java
//lambda表达式只能引用标记了final的外层局部变量，这就是说不能在lambda表达式内部修改在域外的局部变量，否则会编译错误。

public class Java8Tester {
   final static String salutation = "Hello! ";
   public static void main(String args[]){
      GreetingService greetService1 = message -> System.out.println(salutation + message);
      greetService1.sayMessage("Runoob");
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
}

//我们也可以直接在lambda表达式中访问外层的局部变量
public class Java8Tester {
    public static void main(String args[]) {
        final int num = 1;
        Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
        s.convert(2);  // 输出结果为 3
    }
 
    public interface Converter<T1, T2> {
        void convert(int i);
    }
}

//lambda表达式的局部变量可以不用声明为final，但必须不可被后面的代码修改（即隐性具有final的语义）
int num = 1;  
Converter<Integer, String> s = (param) -> System.out.println(String.valueOf(param + num));
s.convert(2);
num = 5;  
//报错信息：Local variable num defined in an enclosing scope must be final or effectively 
 final
     
//在lambda表达式中不允许声明一个与局部变量同名的参数或者局部变量
String first = "";  
Comparator<String> comparator = (first, second) -> Integer.compare(first.length(), second.length());  //编译会出错
```

