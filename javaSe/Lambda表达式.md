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
                System.out.println(message);
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

