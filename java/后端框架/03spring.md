### 1.spring简介

#### 1.1简介

```
1.spring是开源框架。
2.使用IOC及AOP原理来构建javaee项目。
3.分离程序中组件间的依赖关系（解耦）。
4.简化企业级开发的框架。

总结：spring是一个集成性服务性的框架，可以服务于其他框架。（可以理解为硬件上的主板）
```

#### 1.2下载

```
官网：
http://spring.io/

maven网址：
http://repo.spring.io/release/org/springframework/spring/
```

#### 1.3spring解决的问题

```
1.方便解耦，简化开发：
Spring 就是一个大工厂，可以将所有对象创建和依赖关系维护，交给 Spring 管理
2.AOP 编程的支持：
Spring 提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
3.声明式事务的支持：
只需要通过配置就可以完成对事务的管理，而无需手动编程
4.方便程序的测试：
Spring 对 Junit4 支持，可以通过注解方便的测试 Spring 程序
5.方便集成各种优秀框架：
Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、
Hibernate、MyBatis、Quartz 等）的直接支持
6.降低 JavaEE API 的使用难度：
Spring对 JavaEE 开发中非常难用的API（JDBC、JavaMail、远程调用等），都提供了封装，使这些 API 应用难度大大降低
```

#### 1.4spring的组成（架构）

```
spring每个jar包都具有相对独立功能，这些jar包大致分为以下模块：
Spring框架包含的功能大约由20个模块组成。这些模块按组可分为核心容器、数据访问/集成，Web，AOP(面向切面编程)、设备、消息和测试。
```

```
1.spring核心模块：
(1).spring-core：
	依赖注入IoC与DI的最基本实现。
	这个jar文件包含Spring框架基本的核心工具类，Spring其它组件要都要使用到这个包里的类，是其它组件的基本	 核心，当然你也可以在自己的应用系统中使用这些工具类
(2)spring-beans：
	Bean工厂与bean的装配
	这个jar文件是所有应用都要用到的，它包含访问配置文件、创建和管理bean以及进行Inversion of Control /
	Dependency Injection（IoC/DI）操作相关的所有类。如果应用只需基本的IoC/DI支持，引入spring-	core.jar及spring- beans.jar文件就可以了
	
(3)spring-context：
	spring的context上下文即IoC容器。
	Spring核心提供了大量扩展，这样使得由 Core 和 Beans 提供的基础功能增强：这意味着Spring 工程能以框架模式访问对象。Context 模块继承了Beans 模块的特性并增加了对国际化（例如资源绑定）、事件传播、资源加载和
context 透明化（例如 Servlet container）。同时，也支持JAVA EE 特性，例如 EJB、 JMX 和 基本的远程访问。Context 模块的关键是 ApplicationContext 接口。spring-context-support 则提供了对第三方库集成到 Spring-context 的支持，比如缓存（EhCache, Guava, JCache）、邮件（JavaMail）、调度（CommonJ, Quartz）、模板引擎（FreeMarker, JasperReports, Velocity）等。

(4)spring-context-support：

(5)spring-expression：
	spring表达式语言。
	为在运行时查询和操作对象图提供了强大的表达式语言。它是JSP2.1规范中定义的统一表达式语言的扩展，支持
	set 和 get 属性值、属性赋值、方法调用、访问数组集合及索引的内容、逻辑算术运算、命名变量、通过名字从
	Spring IoC容器检索对象，还支持列表的投影、选择以及聚合等。
```

```
2.数据访问层模块：

（1）spring-jdbc
提供了 JDBC抽象层，它消除了冗长的 JDBC 编码和对数据库供应商特定错误代码的解析。
（2）spring-tx
支持编程式事务和声明式事务，可用于实现了特定接口的类和所有的 POJO 对象。编程式事务需要自己写
beginTransaction()、commit()、rollback()等事务管理方法，声明式事务是通过注解或配置由 spring 自动处理，
编程式事务粒度更细。
（3）spring-orm
提供了对流行的对象关系映射 API的集成，包括 JPA、JDO 和 Hibernate 等。通过此模块可以让这些 ORM 框架和
spring 的其它功能整合，比如前面提及的事务管理。
（4）spring-oxm
模块提供了对 OXM 实现的支持，比如JAXB、Castor、XML Beans、JiBX、XStream等。
（5）spring-jms
模块包含生产（produce）和消费（consume）消息的功能。从Spring 4.1开始，集成了 spring-messaging 模块
```

```
3.web处理层模块：

（1）spring-web
提供面向 web 的基本功能和面向 web 的应用上下文，比如 multipart 文件上传功能、使用 Servlet 监听器初始化
IoC 容器等。它还包括 HTTP 客户端以及 Spring 远程调用中与 web 相关的部分
（2）spring-webmvc
为 web 应用提供了模型视图控制（MVC）和 REST Web 服务的实现。Spring 的 MVC 框架可以使领域模型代码和
web 表单完全地分离，且可以与 Spring 框架的其它所有功能进行集成
（3）spring-webmvc-portlet
（即Web-Portlet模块）提供了用于 Portlet 环境的 MVC 实现，并反映了 pring-webmvc 模块的功能
```

```
4.aop切面编程实现模块
（1）spring-aop
提供了面向切面编程（AOP）的实现，可以定义诸如方法拦截器和切入点等，从而使实现功能的代码彻底的解耦。
使用源码级的元数据。
（2）spring-aspects
提供了对 AspectJ 的集成。
```

```
5.instrument模块
（1）spring-instrument
模块提供了对检测类的支持和用于特定的应用服务器的类加载器的实现。
（2）spring-instrument-tomcat
模块包含了用于 Tomcat 的Spring 检测代理。
```

```
6.messages消息处理模块
spring-messaging 模块
从 Spring 4 开始集成，从一些 Spring 集成项目的关键抽象中提取出来的。这些项目包括 Message、
MessageChannel、MessageHandler 和其它服务于消息处理的项目。这个模块也包含一系列的注解用于映射消息
到方法
```

```
7.集成单元测试的模块
spring-test 模块
通过 JUnit 和 TestNG 组件支持单元测试和集成测试。它提供了一致性地加载和缓存 Spring 上下文，也提供了用于
单独测试代码的模拟对象（mock object）
```

### 2.IOC原理和应用

####2.1IOC的概念

```
Inversion of Controll(控制反转)；
把对象的创建权交由外部的容器来创建（spring容器），也就是对象创建权由程序主动创建，现在交由spring来创建了。
```

#### 2.2使用原理

```
spring容器--->加载applicationContext.xml（相应类的完全限定名）--->通过反射机制创建对像---->使用容器的getBean（）方法来获取对像。
```

#### 2.3实现步骤

**步骤一：引入jar包**

```xml
<!--core container模块-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.2.8.Release</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.1.2</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.14</version>
</dependency>
```

**步骤二：在maven的resources下创建applicationContext.xml文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- 告诉spring上下文需要创建的类的对像 User user = new User() -->
	<bean id="user" class="ioc.model.User" scope="prototype"></bean>

</beans>	
```

**步骤三：引入日志log4j.properties**

```properties
log4j.rootLogger=debug,A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c]-[%p] %m%n
```

**步骤四：初始化spring容器**

```java
public static void main(String[] args) {
		
		//1.加载ApplicationContext.xml文件，初始化spring上下文
		//ClassPathXmlApplicationContext会去classess文件夹下加载指定文件
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		//2.获取容器中相应bean(spring默认会以单例模式创建对像)
		User user = ac.getBean("user", User.class);
		
		System.out.println(user);
		
		User user2 = ac.getBean("user", User.class);
		
		System.out.println(user2);
	}
```

#### 2.4spring容器的初始化方式

```java
1.使用ClassPathXmlApplicationContext对象来完成初始化，ClassPathXmlApplicationContext会去classess文件夹下加载指定文件，如下：
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
2.使用FileSystemXmlApplicationContext对象来完成初始化，一般指定绝对路径，会到磁盘中相应位置去寻找xml文件进行加载。如下：
ApplicationContext context = new FileSystemXmlApplicationContext("D:/qfworkspace/day06_spring/src/main/resources/applicationContext.xml");
```

#### 2.5bean标签的使用

```java
用来配置类的信息，让spring容器完成对象的创建。
该标签几个常用的属性：
1.id属性：
	用来描述容器bean的唯一性标识，该id值在容器中要唯一存在，不能重复。
2.name属性：
	和id用法相同，只是该值可以重复，也是用来标识该bean的。
3.class属性：
	配置该bean的完全限定名，spring容器能通过反射机制来创建对象。
4.scope属性（spring bean的作用域）：
	singleton：spring容器将以单例的方式创建该对象；（默认）
	prototype：原型模式，spring容器总是创建新的对象；
	request：和请求线程绑定HttpServletRequest,一次请求对一个对象的创建。
	session：和HTTPSession绑定一起，每一个session创建，则对应一个对象被创建。
	global-session：所用session共享一个bean实例；
5.lazy-init属性：（仅对singleton方式有效）
	表示spring容器创建时，要不要初始化该bean，默认不支持懒加载。
	false：容器创建时就初始化该bean；	true：需要时才去创建；
6.init-method属性：表示该bean初始化时想让spring容器执行的方法；
7.destroy-method属性：表示对象销毁前，想让spring容器去执行的方法；
```

##### 1）lazy-init属性实例：

**applicationContext.xml**

```xml
 <!--lazy-init只对scope="singleton"有效，默认情况不支持懒加载。-->
	<bean name="user3" id="user3" class="ioc.model.User" scope="singleton" lazy-init="true"></bean>
```

**测试**

```java
@Test
	public void testIOC3(){
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		//当不主动使用该User对像时，发现容器并没有创建User对像。当放开下面注释时，发现容器才去创建。
		//User user = ac.getBean("user3", User.class);
	}
```

##### 2）init-method和destroy-method属性

说明：init-method="类中某方法"：类初始化后调用的方法 

​	    destroy-method=“类中某方法”：类创建的对象被销毁之前调用的方法

```java
//由spring容器管理的bean
public class User {
	private int id;
	private String username;
	private String password;
	private Date birthday;
	private Address address;
	public User(){
		System.out.println("User()");
	}
	//省略setter和getter方法
	public void initInfo(){
		System.out.println("初始化后将要调用的方法");
	}
	public void destoryInfo(){
		System.out.println("对像被销毁前要调用的方法");
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	
	<bean id="user4" class="ioc.model.User" init-method="initInfo" destroy-method="destoryInfo"></bean>
	
</beans>
```

```java
@Test
	public void testIOC4(){
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
        //发现User类中initinfo()方法被调用
		User user = ac.getBean("user4", User.class);
		user = null;
	}
```

#### 2.6对象创建的几种方式

```
1.无参构造方法实例化bean:
   spring容器默认使用的方法，会去对像寻找无参构造方法。所以无参构造方法一般必须提供。 
2.指定使用有参构造方法实例化bean:
	使用bean标签中的子标签<constructor-arg></constructor-arg>来指定有参构造方法
3.静态工厂方式实例化bean:
	手动提供静态工厂，然后让spring容器去使用该工厂即可。
4.非静态工厂方式实例化bean:
	手动提供非静态工厂，然后让spring容器去使用该工厂即可。
```

##### 1）静态工厂实例化bean

```java
//手动指定静态工厂
public class StaticFactory {
	public static Object getObj(){
		User user = new User();
		return user;
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- 使用静态工厂创建bean -->
	<!-- class指定是工厂类的完全限定名，factory-method表示工厂中的静态方法 -->
	<bean id="user5" class="factory.StaticFactory" factory-method="getObj"></bean>
	
</beans>
```

```java
@Test
	public void testIOC5(){
		//静态工厂
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		User obj = ac.getBean("user5",User.class);
		System.out.println(obj);
	}
```

##### 2）非静态工厂实例化bean

```java
//手动定义非静态工厂
public class NoStaticFactory {
	public Object getObj(){
		User user = new User();
		return user;
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	
	<!-- 使用非静态工厂创建bean -->
	<bean id="fac" class="factory.NoStaticFactory"></bean>
	
	<!-- factory-bean指定是工厂的实例，factory-method指非静态工厂中的方法 -->
	<bean id="user6"  factory-bean="fac" factory-method="getObj"></bean>
	
</beans>

说明：和java原生创建非静态bean的理念一值，调用非静态方法，必须先创建该方法所属的类的对象。
```

```java
@Test
	public void testIOC6(){
		//非静态工厂
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		User obj = ac.getBean("user6",User.class);
		System.out.println(obj);
	}
```

##### 3）调用无参构造方法实例化bean（spring默认）

#####4）调用有参构造实例化bean（详见DI内容，属于依赖注入）

### 3.DI应用

#### 3.1setter方法属性注入

##### 1）简单类型属性注入

```java
//User
public class User {
	private int id;
	private String username;
	private String password;
	private Date birthday;
    //省略setter和getter
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- 使用类中Setter方法进行属性注入 -->

	<!--1.简单类型属性的注入 
 		property:表示属性的名称，默认会调用相应的setter方法进行属性注入
		value:表示赋的简单类型的值
	-->
	<bean id="user" class="di.model.User">
		<property name="id" value="1"></property>
		<property name="username" value="张三"></property>
		<property name="password" value="1234"></property>
		<property name="birthday" value="2018/10/10"></property>
	</bean>
</beans>
```

##### 2）pojo类型属性注入

```java
//User
public class User {

	private int id;
	private String username;
	private String password;
	private Date birthday;
     private Address address;
	
    //省略setter和getter
}

//Address

public class Address {

	private int id;
	private String addrname;
    
     //省略setter和getter 
}
```

```xml
<!--2.pojo属性的注入  -->
	<bean id="user2" class="di.model.User">
		<property name="id" value="1"></property>
		<property name="username" value="张三"></property>
		<property name="password" value="1234"></property>
		<property name="birthday" value="2018/10/10"></property>
		<!-- 对pojo属性进行注入
		   其中ref属性或ref标签表示引入spring容器中相应bean对像（这里能过id或name引入）。
         -->
		<property name="address">
			<ref bean="addr"/>
		</property>
	</bean>

	<bean id="addr" class="di.model.Address">
		<property name="id" value="1001"></property>
		<property name="addrname" value="杭州"></property>
	</bean>
```

##### 3)集合属性注入

```java
//User
public class User {
	private Map<String,String> map;
	private Set<String> set;
	private List<Object> list;
     //省略setter和getter
}
```

```xml
<!--List-->
<bean id="user3" class="di.model.User" >
		<property name="list">
            <!--使用list标签定义list属性注入-->
			<list>
				<value>list1</value>
				<value>list2</value>
				<value>list3</value>
				<!-- 可以引入外部spring子文件中bean -->
				<ref bean="addr"/>
				<!--也可以在内部使用bean标签定义对像，和外部bean标签一样-->
				<bean class="di.model.Order">
					<property name="id" value="1000"></property>
					<property name="ordername" value="100号定单"></property>
					<property name="price" value="2000"></property>
				</bean>
			</list>
		</property>
</bean>

<bean id="addr" class="di.model.Address">
		<property name="id" value="1001"></property>
		<property name="addrname" value="杭州"></property>
	</bean>
```

```xml
<!--set类型属性注入  -->
	<bean id="user4" class="di.model.User" >
		<property name="set">
            <!--使用set标签定义set属性注入-->
			<set>
				<value>set1</value>
				<value>set2</value>
				<value>set3</value>
			</set>
		</property>
	</bean>
```

```xml
<!-- map集合属性注入 -->
	
	<bean id="user5" class="di.model.User">
		 <!--使用map标签定义map属性注入-->
		<property name="map">
			<map>
                <!--entry标签表示键值对-->
				<entry>
					<key><value>key1</value></key>
					<value>val1</value>
				</entry>
				<entry>
					<key><value>key2</value></key>
					<value>val2</value>
				</entry>
				<entry>
					<key><value>key3</value></key>
					<value>val3</value>
				</entry>
			</map>
		</property>
	</bean>
	
```

##### 4)properties对像属性注入

```java
//User
public class User {

	private Properties props;
     //省略setter和getter
}
```

```xml
<!-- properties属性注入 -->
	<bean id="user6" class="di.model.User">
	
		<property name="props">
			<props>
				<prop key="key1">prop1</prop>
				<prop key="key2">prop2</prop>
				<prop key="key3">prop3</prop>
			</props>
		</property>
</bean>
```

####3.2contructor方式属性注入

**1.步骤**

```xml
1.定义有参构造方法（一般还需提供无参构造）
2.在applicationContext.xml文件中使用<constructor-arg>标签设置即可。
```

**2.代码**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- 
		index表示构造方法中参数下标，一般下标从0开始
		constructor-arg标签的数量和构造器参数的数量一致
		type:参数类型,来进一步限定使用的构造方法
		ref:表示引入上下文中其他的bean对像
	 -->
	<bean id="user" class="di.model.User">
		<constructor-arg index="0" value="1"  type="int"></constructor-arg>
		<constructor-arg index="1" value="admin"  type="String"></constructor-arg>
		<constructor-arg index="2" value="1234"  type="String"></constructor-arg>
		<constructor-arg index="3" value="20"  type="int"></constructor-arg>
		<constructor-arg index="4" ref="addr" ></constructor-arg>
	</bean>
	
	<bean id="addr" class="di.model.Address">
		<property name="id" value="100"></property>
		<property name="addrname" value="杭州"></property>
	</bean>
</beans>
```

**3.测试**

```java
	@Test
	public void testDI1(){
		//使用构造器来实例化对像
		
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		User user = ac.getBean("user", User.class);
		System.out.println(user.getId()+","+user.getUsername()+","+user.getPassword()+","+user.getAddr().getAddrname());
		
	}
```

**4.优缺点**

```
优点：
	1.比setter属性注入效率高；
缺点：
	1.业务逻辑复杂，需要重载很多的构造方法，导致代码的书写及维护工作变得很难。
	
注意：一般不推荐使用构造方法方式注入；
```

####3.3p命名空间注入

**1.步骤**

```
1.在applicationContext.xml文件头部加上p命名空间
	"xmlns:p="http://www.springframework.org/schema/p"
2.在bean标签中使用p：属性方式注入；
```

**2.代码**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
<!--引入p命名空间-->
xmlns:p="http://www.springframework.org/schema/p"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

	<!-- P命名空间注入
		简单类型使用p:属性=“值”方式来注入
		pojo类型使用p:属性-ref="值"方式来注入
		注意：属性其实指的是setter方法
	 -->
	<bean id="user2" class="di.model.User" p:id="1" p:username="user" p:password="1234" p:age="20" p:addr-ref="addr"></bean>
	
</beans>
```

####3.4属性自动注入

**1.步骤**

```
注意：只适用于pojo类型的自动注入
1.指定属性自动注入的方式（default|no|byName|byTyep|constructor）
其中no|byName|byType会经常使用
```

**2.byName**

```java
package com.tutorialspoint;
public class TextEditor {
   private SpellChecker spellChecker;
   private String name;
   public void setSpellChecker( SpellChecker spellChecker ){
      this.spellChecker = spellChecker;
   }
   public SpellChecker getSpellChecker() {
      return spellChecker;
   }
   public void setName(String name) {
      this.name = name;
   }
   public String getName() {
      return name;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- 通过找打与属性名相匹配的bean的id名字进行注入 -->
   <bean id="textEditor" class="com.tutorialspoint.TextEditor" 
      autowire="byName">
      <property name="name" value="Generic Text Editor" />
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
   </bean>

</beans>
```

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      TextEditor te = (TextEditor) context.getBean("textEditor");
      te.spellCheck();
   }
}
```

**3.byType**

```java
package com.tutorialspoint;
public class TextEditor {
   private SpellChecker spellChecker;
   private String name;
   public void setSpellChecker( SpellChecker spellChecker ) {
      this.spellChecker = spellChecker;
   }
   public SpellChecker getSpellChecker() {
      return spellChecker;
   }
   public void setName(String name) {
      this.name = name;
   }
   public String getName() {
      return name;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- 通过找打与属性类型相匹配的bean的class类型进行注入 -->
   <bean id="textEditor" class="com.tutorialspoint.TextEditor" 
      autowire="byType">
      <property name="name" value="Generic Text Editor" />
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id="SpellChecker" class="com.tutorialspoint.SpellChecker">
   </bean>

</beans>
```

**4.constructor**

```
原理：与byType原理一样，通过找到相同的类型进行注入，只是注入到constructor的参数中，而非属性中；
```

```java
package com.tutorialspoint;
public class TextEditor {
   private SpellChecker spellChecker;
   private String name;
   public TextEditor( SpellChecker spellChecker, String name ) {
      this.spellChecker = spellChecker;
      this.name = name;
   }
   public SpellChecker getSpellChecker() {
      return spellChecker;
   }
   public String getName() {
      return name;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- Definition for textEditor bean -->
   <bean id="textEditor" class="com.tutorialspoint.TextEditor" 
      autowire="constructor">
      <constructor-arg value="Generic Text Editor"/>
   </bean>

   <!-- Definition for spellChecker bean -->
   <bean id="SpellChecker" class="com.tutorialspoint.SpellChecker">
   </bean>

</beans>
```

### 4.注解的使用

##### 4.1常用IOC注解

```
1.@Component:
  一般使用在类上，spring会扫描该注解对应类，并为该类创建对像。
  MVC分层时，一般该注解应用于Controller层，Service层，Dao层
2.@Controller:
	和@Component用法一样
	MVC分层时，一般该注解应用于Controller层
3.@Service
	和@Component用法一样
	MVC分层时，一般该注解应用于Service层
4.@Respository:
	和@Component用法一样
	MVC分层时，一般该注解应用于Dao层
5.@Autowire:
	一般用于pojo类型属性上，会从spring容器中织入对像到相应的属性上去（按byType来织入）。
6.@Qulifier:
	一般用于pojo类型属性上，和@Autowired配合使用，转化为按bean的名称指定（按byName来织入）
7.@Resource:
	一般用于pojo类型属性上，指定bean的名称去织入（按byName来织入）,替代 5和 6注解。
8.@PostConstruct
	一般用于方法上，表示调用构造器之后要执行的方法，等同于init-method
9.@PreDestroy
	一般用于方法上，表示对像销毁前要执行的方法，等同于destory-method
10.@Scope
	一般用于类上，表示对像生命周期（作用域 singleton|prototype|request|session|global session）
	
```

#####4.2使用步骤

```xml
1.在applicationContext.xml文件头部引入命名空间
	xmlns:context="http://www.springframework.org/schema/context"
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd" 
2.在applicationContext.xml中使用<context:component-scan base-package="包名，包名"></context:component-scan>扫描指定包下的类，如果该类上有上述IOC注解，就会为该类创建对象。
3.在类和属性上使用合适的注解即可。
```

##### 4.3代码

**applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd" >

	<!--base-package指定要扫描的包名：多个包之间可用逗号隔开，也可以指定总包名-->
	<context:component-scan base-package="anno"></context:component-scan>

</beans>
```

**DAO层**

```java
//spring容器会创建UserDao类的对像
//其中value属性作用是:为该对像指定name值，即相当于UserDao dao = new UserDao()
//如果没指定value值，则默认会使用类名第一个字母小写的方式作为该bean的name,即userDao为name值。
@Repository(value="dao")
public class UserDao {
	public User selectById(int id){
		User user = new User();
		user.setId(id);
		return user;
	}
}
```

**service层**

```java
//spring容器会创建UserService类的对像
//其中value属性作用：为该容器中该对像指定name值，相当于UserService ser = new UserService()
@Service(value="ser")
public class UserService {
	
    //该注解表示按名称自动织入UserDao对像。name表示容器中UserDao对像的name值。
    //可以替代 @Autowired和@Qualifier注解
	@Resource(name="dao")
    //也可以使用该注解进行属性自动织入（按类型织入）
    //也可和@Resource()注解一起使用 （按名称织入），其中value属性表示bean的name值
    @Autowired
    @Qualifier(value="dao")
	private UserDao dao;
	
	
	public User findById(int id){
		
		return dao.selectById(id);
	}

}

```

**测试**

```java
@Test
	public void testAnno(){
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext-anno.xml");
		UserService ser = ac.getBean("ser", UserService.class);
		User user = ser.findById(1);
		System.out.println(user);
	}
```

###5.spring集成单元测试

#### 5.1.原因

```
一般会把spring容器床架的对象，织入到单元测试中，不需要手动创建对象，使用管理。
```

#### 5.2步骤

```java
1.引入spring-test.jar
2.在单元测试类上使用注解@ContextConfiguration和@RunWith 
@ContextConfiguration用来加载applicationContext.xml文件，相当于ApplicationContext ac = new ClassPathXmlApplicationContext(String lication);
@RunWith用来实例化ApringJUnit4ClassRunner类对象的
3.在单元测试的属性上使用@Autowired织入对象。
```

#### 5.3代码

```java
@RunWith(value=SpringJUnit4ClassRunner.class)
//创建容器
@ContextConfiguration(value={"classpath:applicationContext-anno.xml"})
//指定创建容器时使用哪个配置文件  classpath:该前缀的作用是直接定位到工程中classess文件夹下
public class SpringTest {
	@Autowired
	UserService ser;
	@Test
	public void testDI(){
		User user = ser.findById(1);
		System.out.println(user);
	}
}
```

### 6.Spring之JDBC模板

#### 6.1为什么使用JDBCTemplate

```
Spring JDBC是Spring所提供的持久层技术,它的主要目标是降低使用JDBC API的门槛,以一种更直接,更简洁,更
简单的方式使用JDBC API, 在Spring JDBC里,仅需做那些与业务相关的DML操作,而将资源获取,Statment创建,
资源释放以及异常处理等繁杂而乏味的工作交给Spring JDBC.
	虽然ORM的框架已经成熟丰富,但是JDBC的灵活,直接的特性,依然让他拥有自己的用武之地,如在完全依赖查询
模型动态产生查询语句的综合查询系统中,Hibernaye,MyBatis,JPA等框架都无法使用,这里JDBC是唯一的选择.
```

#### 6.2使用步骤

**1.引入jar包依赖**

```xml
<!--引入jar包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>

<!--数据源-->
<dependency>
	<groupId>com.mchange</groupId>
	<artifactId>c3p0</artifactId>
	<version>0.9.5.2</version>
</dependency>

<!--mysql驱动-->
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.40</version>
</dependency>
```

**2.使用JDBCTemplate模板API**

```JAVA
//Spring操作数据模板类(工具类)
JdbcTemplate 
//DML操作(增删改)
JdbcTemplate.update(sql,ArgsObj....); 
//DDL|DCL操作
JdbcTemplate.execute(sql) 
//DQL查询单个数据
jdbcTemplate.queryForObject(String sql, RowMapper<T> var2, Object... var3);
//查询所有数据List<T>
jdbcTemplate.query(String sql, RowMapper<T> var2, Object... var3);
//返回List<Map<String,Object>>
jdbcTemplate。queryForList（String sql，Object... var3）

//注意：
其中RowWapper<T> 将结果封装的处理器; 得到Result解析成实体类对象即可!
```

**3.引入资源文件**

```properties
driverClass=com.mysql.jdbc.Driver
jdbcUrl=jdbc:mysql://localhost:3306/javaee
user=root
password=123
```

**4.spring上下文配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans  xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:p="http://www.springframework.org/schema/p"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:aop="http://www.springframework.org/schema/aop"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop.xsd">
	
	<!-- 开启包扫描 -->
	<context:component-scan base-package="jdbc"></context:component-scan>
		
	<!-- 加载properties文件 -->
	<context:property-placeholder location="db.properties"/>
 
 	<!-- dataSource数据源 -->
 	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
 		<property name="driverClass" value="${driverClass}"></property>
 		<property name="jdbcUrl" value="${jdbcUrl}"></property>
 		<property name="user" value="${user}"></property>
 		<property name="password" value="${password}"></property>
 	</bean>
 	
    <!--创建模板对像-->
 	<bean id="jdbcTemp" class="org.springframework.jdbc.core.JdbcTemplate">
 		<property name="dataSource" ref="dataSource"></property>
 	</bean>
</beans>
```

**5.把模板对象注入到DAO层**

```java
@Repository
public class StudentDao {
	//注意：JdbcTemplate是线程安全的，可以直接作为全局变量使用
	@Autowired
	private JdbcTemplate temp;

	//增
	public void insert(Student stu){
		temp.update("insert into student(sname) values(?)", new Object[]{stu.getSname()});
	}
	//删
	public void delete(int id){
		temp.update("delete from student where id = ?", id);
	}
	//改
	public void update(Student stu){
		temp.update("update student set sname=? where id=?", stu.getSname(),stu.getId());
	}
	//查单个对像
	public Student selectById(Integer id){
		
		Student stu = temp.queryForObject("select * from student where id = ?",new RowMapper<Student>(){
			@Override
			public Student mapRow(ResultSet resultset, int i) throws SQLException {
				
				Student stu = beanHandler(resultset);
				
				return stu;
			}},id);
		
		return stu;
	}
	//查总记录数
	public int selectCount(){
		int count = temp.queryForObject("select count(*) from Student",Integer.class);
		return count;
	}
	//查所有
	public List<Student> selectAll(){
		List<Student> stu = temp.query("select * from student", new RowMapper<Student>(){
			@Override
			public Student mapRow(ResultSet resultset, int i) throws SQLException {
				
				Student stu = beanHandler(resultset);
				
				return stu;
			}	
		});
		return stu;
	}
	
	/**
	 * Bean封装处理器
	 * @param resultset
	 * @return
	 */
	public Student beanHandler(ResultSet rs){
		
		Student stu = new Student();
		try {
			int id = rs.getInt("id");
			String sname = rs.getString("sname");
			stu.setId(id);
			stu.setSname(sname);
		} catch (SQLException e) {
			e.printStackTrace();
		}

		return stu;
	}
}

```

**6.测试**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(value="classpath:applicationContext-jdbc.xml")
public class TestJDBC2 {

	@Autowired
	StudentDao dao;
	
	@Test
	public void testSave(){
		
		Student stu = new Student();
		stu.setSname("张三");
		dao.insert(stu);
	}
	
	@Test
	public void testRemove(){
		
		dao.delete(3);
		
	}
	
	@Test
	public void testModify(){
		
		Student stu = new Student();
		stu.setId(1);
		stu.setSname("张三");
		dao.update(stu);
	}
	
	@Test
	public void testFindById(){
		
		Student stu = dao.selectById(1);
		System.out.println(stu.getSname());
	}
	
	@Test
	public void testFindAll(){
		
		List<Student> list =dao.selectAll();
		
		System.out.println(list);
		
	}
	
	@Test
	public void testFindCount(){
		
		int i = dao.selectCount();
		System.out.println(i);
	}
}
```

### 7.AOP编程原理

#### 7.1AOP概念

```
1.AOP（Aspect Oriented Programming）,即面向切面编程，其目的就是把核心业务和通用功能（切面业务）进行拆分，降低耦合性，提高扩展性。
2.AOP技术是对OOP技术的扩展和补充，OOP技术强调的是抽象，封装，建成统一结构。AOP技术主要是把代码分离。
```

#### 7.2AOP技术原理

```
1.连接点（join point）
	横切业务在核心业务代码中执行的位置。
	spring仅支持方法连接点，即仅能在方法调用前，方法调用后，方法抛出异常是，以及方法调用前后这些程序执行点织入增强；
2.切点（Pointcut）
	独立的连接点，当前该连接点正在被使用并织入切面业务，该连接点被称为切点；
3.增强（advice）
	切面业务逻辑，强调的是切入时机。
	BeforeAdvice前置增强，AfterReturningAdvice主业务方法执行完后织入切面逻辑，ThrowsAdvice抛出异常后织入切面逻辑。
4.目标对象（target）
	核心业务逻辑，即主程序代码；
5.引介（introduction）
	引介是一种特殊的额增强，它为类添加一些属性和方法，这样，即使一个业务类原本没有实现某个接口，通过AOP的引介功能，也可以动态的为该业务类添加接口的实现逻辑，让业务类称为这个接口的实现类。
6.织入（weaving）
	把横切业务切到核心业务上，这个过程叫织入；
	spring主要通过动态代理织入，在运行期为目标类中添加增强生成子类的方式；
	spring采用动态代理织入（JDK动态代理|CGLIB动态代理），而AspectJ采用编译期织入和类装载器织入。
7.代理（proxy）
	一个类被AOP织入增强后，就产生了一个结果类，它是融合了原类功能和增强逻辑功能的代理类，根据不同的代理方式，代理类既可能是和原类具有相同接口的类（JDK动态代理），也可能就是原类的子类（CGLIB动态代理），所以可以采用与原类相同的方法调用代理类。
8.切面（aspect）
	整合了增强（advice）和切面业务的代码，即横切业务。
```

#### 7.3SpringAOP底层技术实现之JDK动态代理

##### 1.JDK动态代理实现步骤

```java
1.目标类和代理类具有相同的接口
2.使用JDK提供的proxy类来生成动态代理对象，
3.实现接口InvocationHandler接口  
 	回调目标类方法并且加入横切方法
4.依赖spring-aop.jar
```

##### 2.代码

```java
//接口
public interface IMainBiz{
    public void run();
}
```

```java 
//目标类|核心业务类|被代理类
public class MainBiz implements IMainBiz{
    @Override
	public void run() {
		System.out.println("核心业务方法");
	}
}
```

```java
//InvocationHandler接口实现类对象（实现invoke方法回调主业务方法，并进行增强）
public class ProxyService implements InvocationHandler{
	Object target;
	//核心业务类对象
	public ProxyService(Object target) {
		this.target=target;
	}
	
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		//回调核心业务类方法
		Object invoke = method.invoke(target, args);
		//后置增强
		System.out.println("输出日志...........");
		return invoke;
	}
	
	
	//生成代理类对象
	public Object getProxyService() {
		return Proxy.newProxyInstance(target.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
	}
	
}

```

```java
//测试类
public void test() {
 		//创建被代理类对象
		MainBiz biz = new MainBiz();
    	//代理类对象传入InvocationHandler接口的实现类，供方法回调和增强
		ProxyService service = new ProxyService(biz);
    	//生成的代理类因为和被代理类实现相同的接口，因此可以用接口类型接收代理对象
		IMainBiz mainBiz = (IMainBiz) service.getProxyService();
    	//调用代理类的run()方法，会进行增强
		mainBiz.run();
	}
```

```java
个人理解：
主要两方面：1）利用Proxy生成代理类对象；2）InvocationHandler实现类对方法进行增强
	1.
	Proxy.newProxyInstance(ClassLoader,Interfaces,h);生成代理对象；
	--classLoader参数：被代理类的类加载器
	--Interfaces参数：被代理类实现的所有接口
	--h参数：InvocationHandler接口的实现类
	
	2.
    new InvocationHandler(){
    	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		//回调核心业务类方法
		Object invoke = method.invoke(target, args);
		//后置增强
		System.out.println("输出日志...........");
		return invoke;
	}
    }
    
注意：JDK动态代理要求被代理类必须实现接口，且增实现接口内的方法，因为代理类也实现该接口，仅能调用该接口方法
```

#### 7.4SpringAOP底层技术实现之CGLIB动态代理

##### 1.执行原理

```java
如果一个类没有实现接口，又想为这个类创建代理对象，JDK动态代理的方式就不可行。这时，一般使用继承目标类的方式，来扩展功能。CGLIB就是通过主动继承目标类生成子类的方式来扩展相应切面功能的。
当我们主动执行目标类相应的方法时，则CGLIB会拦截所有对目标类方法的执行，转向到执行子类中（代理对象）的方法，则切面业务方法得以执行，然后子类（代理对象）会去执行父类中相应的方法。
```

##### 2.实现步骤

```
1.创建目标类；
2.创建拦截器类，该类实现MethodInterceptor
3.使用字节码增强技术，即在内存中动态生成子类字节码文件，让子类（代理类）继承父类（被代理类）。
4.获取代理对象，调用相应方法即可
```

##### 3.代码

```xml
<!--AOP实现-->
	<dependency>
		<groupId>aopalliance</groupId>
		<artifactId>aopalliance</artifactId>
		<version>1.0</version>
	</dependency>
	<!--切面实现-->
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjweaver</artifactId>
		<version>1.8.10</version>
	</dependency>
	<!--spring对切面的支持-->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aspects</artifactId>
		<version>4.2.8.RELEASE</version>
	</dependency>
	<!--spring对aop的支持-->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aop</artifactId>
		<version>4.2.8.RELEASE</version>
	</dependency>
```

```java
//目标类
public class MainBiz{
    public void run(){
        System.out.println("核心业务方法");
    }
}
```

```java 
//拦截类
public class ProxyService implements MethodInterceptor{
    //字节码增强对象
    private Enhancer enhancer = new Enhancer();
    
    /**
     *回调父类中的方法
     *执行切面业务
     *proxy子类对象
     *method方法（不要主动使用method方法，会产生死循环）
     *params方法参数
     *methodProxy复合对象，其中包含父类对象
    */
	@Override
    public Object intercept(Object proxy,Method method,Object[] params,MethodProxy methodProxy) throws Throwable{
        //回调父类中的方法
        Object ret = methodProxy.invokeSuper(proxy,params);
        //切入切面业务
        System.out.println("写日志");
        return ret;
    }
    
    /**
    *获取代理对象
    */
    public Object getProxy(Class target){
        //获取父类结构，生成子类结构
        enhancer.setSuperclass(target);
        //回调MethodInterceptor中的intercept方法
        enhancer.setCallback(this);
        //创建代理对象（子类对象）
        return enhancer.create();
    }
    
}
```

```java 
//测试
public static void main(String[] args){
    ProxyService ser = new ProxyService();
    //代理对象继承了目标类，所以可以用目标类接收
    MainBiz mainBiz = ser.getProxy(MainBiz.class);
    mianBiz.run();
}
```

### 8.SpringAOP实际应用

#### 8.1基于xml的AOP编程

##### 1.实现步骤

```xml
1.在applicationContext.xml头上设置命名空间
	 xmlns:aop="http://www.springframework.org/schema/aop"
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop.xsd" 
2.在applicationContext.xml文件中配置目标类对象和切面类对象
3.使用<aop:config></aop:config>标签配置目标类对象和切面类对象之间的关系
```

##### 2.增强策略

```java
1.aop:before  前置增强
2.aop:after   后置增强（不管是否抛出异常都会执行切面业务）
3.after-returning  后置增强，可以在切面类中获取返回值作进一步处理。（抛出异常将不执行切面业务）
4.after-throwing  后置增强，可以在切面类中获取主业务方法抛出的异常做进一步处理。（不抛出异常切面业务将不会执行）
5.after-round  环绕增强，可以在主业务方法的前后均织入切面业务
```

##### 3.代码

```java
//目标类
public class MainBiz{
    public void doMainBiz(){
        System.out.println("主业务");
    }
}
```

```java 
//切面类
public class LogBiz{
    //1.前置增强
    public void doLogBiz() throws Throwable{
        System.out.println("写日志");
    }
    //后置增强
    public void doLogBiz2() throws Throwable{
        System.out.println("写日志");
    }
    //3.环绕增强
    public void diLogBiz3(ProceedingJoinPoint jp) throws Throwable{
        System.out.println("写日志");
        //调用主业务方法
        Object ret = jp.proceed();
        System.out.println("写日志2");
    }
    //4.后置增强（after-returning方式）
    //注意：方法参数rt的名称必须和标签中的 returning="rt"属性一致
	public void doLogBiz4(Object rt) throws Throwable{
		System.out.println("写日志");
		//可以获取主业务方法的返回值，做进一步业务处理
		System.out.println(rt);
	}
	 //5.后置增强（after-throwing方式）
     //注意：方法参数ex的名称必须和标签中的 throwing="ex"属性一致
	public void doLogBiz5(Exception ex) throws Throwable{
		System.out.println("写日志");
        //可以对主业务方法抛出异常后，做进一点处理
		if(ex instanceof ArithmeticException){
			System.out.println("算术异常");
		}else{
			System.out.println("未知异常");
		}
	}
}
```

```xml
<!--applicationContext.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd" >

	<!-- 核心业务类对像 -->
	<bean id="mainbiz" class="springxml.MainBiz"></bean>
	
	<!-- 切面类对像 -->
	<bean id="logbiz" class="springxml.LogBiz"></bean>
	
	<!-- Spring的AOP编程 -->
	<aop:config>
		<!-- 切入点 
		expression：用来匹配目标类对像中方法
		-->
		<aop:pointcut expression="execution(public * springxml.MainBiz.doMainBiz(..))" id="pointcut"/>
		<!-- 切面,
		ref：引入切面类对像
		method：切面方法
		pointcut-ref：关联切入点
		 -->
		<aop:aspect ref="logbiz">
            <!--前置增强-->
		 	<aop:before method="doLogBiz" pointcut-ref="pointcut"/>
			 <!--后置增强-->
			<aop:after method="doLogBiz2" pointcut-ref="pointcut"/> 
             <!--环绕增强，需要在切面方法中使用ProceedingJoinPoint对像完成对主业务方法的调用-->
			<aop:around method="doLogBiz3" pointcut-ref="pointcut"/>
			 <!--后置增强，需主业务方法无异常抛出-->
			<aop:after-returning method="doLogBiz4" pointcut-ref="pointcut" returning="rt"/>
			 <!--后置增强，需主业务方法有异常抛出-->
			<aop:after-throwing method="doLogBiz5" pointcut-ref="pointcut" throwing="ex"/>
			
			
		</aop:aspect>
	</aop:config>
</beans>
```

```java 
//测试
public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		//代理对像
		MainBiz mainbiz = ac.getBean("mainbiz",MainBiz.class);
		mainbiz.doMainBiz();
	}
```

#### 8.2基于通知类的AOP编程

##### 1.通知接口类型

```java
1.MethodBeforeAdvice    前置增强处理
2.AfterReturningAdvice  后置增强（抛出异常将不执行切面业务）
3.ThrowsAdvice          后置增强（抛出异常才执行切面业务）
如果是环绕增强，则切面类要同时实现1,2,3接口
注意：   使用ThrowsAdvice时，需要手动加入切面方法
		public void afterThrowing(Exception ex){...}
```

##### 2.实现步骤

```xml
1.切面类实现通知接口
2.在applicationContext.xml文件使用<aop:advisor>标签代替<aop:aspect>标签；
```

##### 3.代码

```java
//切面类，实现通知接口
public class LogBiz implements MethodBeforeAdvice,AfterReturningAdvice,ThrowsAdvice{
    
	@Override
	public void before(Method method, Object[] args, Object target) throws Throwable {
		System.out.println("前置日志1");
	}
    
	@Override
	public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
		System.out.println("后置日志正常2");
	}
	
    //需手动加入该方法
	public void afterThrowing(Exception ex){
		System.out.println("后置日志异常2");
	}
}
```

```java
//目标类
public class MainBiz {
	public void doMainBiz(){
		System.out.println("主业务");
	}
}
```

```xml
<!--applicationContext.xml文件配置-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd" >

	<!-- 核心业务类对像 -->
	<bean id="mainbiz" class="springadvice.MainBiz"></bean>
	
	<!-- 切面类对像 -->
	<bean id="logbiz" class="springadvice.LogBiz"></bean>
	
	<!-- Spring的AOP编程 -->
	<aop:config>
		<aop:pointcut expression="execution(public * springadvice.MainBiz.doMainBiz(..))" id="pointcut"/>
		<!-- 实现了通知接口的切面类 -->
		<aop:advisor advice-ref="logbiz" pointcut-ref="pointcut"/>
	</aop:config>
</beans>
```

```java
//测试
public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		MainBiz mainbiz = ac.getBean("mainbiz", MainBiz.class);
		
        //发现主业务方法运行时，会织入增强处理
		mainbiz.doMainBiz();

	}
```

#### 8.3基于注解方式的AOP编程

##### 1.常用AOP注解

```
1.@Aspect   标识一个类为切面类
2.@PointCut 标识一个方法为切入点方法
3.@Before   前置增强策略
4.@After    后置增强策略（不管是否出现异常）
5.@AfterReturning 后置增强（不出现异常的情况）
6.@AfterThrowing  后置增强（出现异常情况）
7.@Around    环绕增强
```

##### 2.实现步骤

```xml
1.在ApplicationContext.xml文件配置支持AOP注解方式驱动
<aop:aspectJ-autoproxy></aop:aspectJ-autoproxy>
2.配合使用IOC注解方式来完成AOP编程；
```

##### 3.代码

```java
//目标类
public class MainBiz{
    public void doMainBiz(){
        System.out.println("主业务");
    }
}
```

```java 
//切面类 @Aspect标识其为切面

@Component
@Aspect
public class LogBiz {
	
    //定义切入点
	@Pointcut(value="execution(public * springanno.MainBiz.doMainBiz(..))")
	public void print(){
	}
	
    //增强策略，引入print()切入点，这里以方法名作为切入点的标识符
    @Around(value="print()")
    //也可以省略@Pointcut注解，直接使用以下简写方式
	//@Around(value="execution(public * springanno.MainBiz.doMainBiz(..))")
	public void doLogBiz() throws Throwable{
		
		System.out.println("写日志");
	 
	}

}
```

```xml
<!--applicationContext.xml配置-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd" >

	<!-- IOC注解 -->
	<context:component-scan base-package="springanno"></context:component-scan>

	<!--支持注解方式AOP编程驱动 -->
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

```java
//测试
public static void main(String[] args){
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		
		MainBiz mainbiz = ac.getBean("biz", MainBiz.class);
		
		mainbiz.doMainBiz();

}
```

### 9.Spring事务管理策略

#### 1.Spring提供事务管理平台

```java
1.DataSourceTransactionManager：JDBC的事务管理平台
2.HibernateTransactionManager:hibernate的事务管理平台
3.JpaTransactionManager:用于分布式事务管理平台
```

#####1.1Spring提供事务管理策略

**在TransactionDefinition类中定义了事务策略常量值**

```java
//事务传播机制
int PROPAGATION_REQUIRED = 0; //支持当前事务，如果不存在，就新建一个
int PROPAGATION_SUPPORTS = 1; //支持当前事务，如果不存在，就不使用事务
int PROPAGATION_MANDATORY = 2; //支持当前事务，如果不存在，就抛出异常
int PROPAGATION_REQUIRES_NEW = 3;//如果有事务存在，挂起当前事务，创建一个新的事物
int PROPAGATION_NOT_SUPPORTED = 4;//以非事务方式运行，如果有事务存在，挂起当前事务
int PROPAGATION_NEVER = 5;//以非事务方式运行，如果有事务存在，就抛出异常
int PROPAGATION_NESTED = 6;//如果有事务存在，则嵌套事务执行

//事务隔离级别
int ISOLATION_DEFAULT = -1;//默认级别，MYSQL: 默认为REPEATABLE_READ级别 SQLSERVER: 默认为
READ_COMMITTED
int ISOLATION_READ_UNCOMMITTED = 1;//读取未提交数据(会出现脏读, 不可重复读) 基本不使用
int ISOLATION_READ_COMMITTED = 2;//读取已提交数据(会出现不可重复读和幻读)
int ISOLATION_REPEATABLE_READ = 4;//可重复读(会出现幻读)
int ISOLATION_SERIALIZABLE = 8;//串行化
int TIMEOUT_DEFAULT = -1;//默认是-1，不超时，单位是秒
//事务的传播行为
int getPropagationBehavior();
//事务的隔离级别
int getIsolationLevel();
//事务超时时间
int getTimeout();
//是否只读
boolean isReadOnly();
String getName();
```

#####1.2事务隔离级别

```java 
1.读未提交
Read uncommitted  //最低级别，以上情况均无法保证
2.读已提交
Read committed  //可避免脏读情况发生
3.可重读读
Repeatable read  //可避免脏读，不可重复读的情况发生，不可避免虚读
4.串行化读
Serializable  //事务只能一个一个执行，避免了脏读等问题，但是执行效率慢，使用时慎重
```

#### 2.AOP切面事务配置

##### 2.1xml文件配置方式事务

```java
//步骤
1.引入jar包；
2.创建service类和dao类；
3.在applicationContext.xml文件配置事务切面；
//xml切面事务配置相当于基于通知类的aop编程，事务切面实现了通知接口MethodBeforeAdvice,AfterReturningAdvice,ThrowsAdvice
```

```xml
<!--jar包-->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.2.8.Release</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.1.2</version>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.14</version>
		</dependency>

		<dependency>
			<groupId>aopalliance</groupId>
			<artifactId>aopalliance</artifactId>
			<version>1.0</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.8.10</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>

		<!--数据源 -->
		<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5.2</version>
		</dependency>

		<!--mysql驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.40</version>
		</dependency>
		
		<!-- spring-tx事务管理 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
		<!--spring-test-->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.2.8.RELEASE</version>
		</dependency>
```

```java
//serivce

@Service
public class AccountService {
	@Autowired
	private AccountDao dao;
    //spring会自动切入事务
	//转账操作
	public void transfer(int fromId,int toId,int money){
		dao.out(fromId, money);
		dao.in(toId, money);
	}
}
```

```java
//dao
@Repository
public class AccountDao {
	
	@Autowired
	private JdbcTemplate temp;

	/**
	 * 转入
	 * @param toId
	 * @param money
	 */
	public void in(int toId,int money){
		
		temp.update("update account set balance=balance+? where id = ?", money,toId);
	}
	/**
	 * 转出
	 * @param fromId
	 * @param money
	 */
	public void out(int fromId,int money){
		
		temp.update("update account set balance=balance-? where id = ?", money,fromId);
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd" >

	<!-- 组件扫描 -->
	<context:component-scan base-package="dao,service"></context:component-scan>
    
	<!-- 加载外部properties属性文件 -->
	<context:property-placeholder location="db.properties"/>
	<!-- C3p0数据源创建 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driver}"></property>
 		<property name="jdbcUrl" value="${jdbc.url}"></property>
 		<property name="user" value="${jdbc.username}"></property>
 		<property name="password" value="${jdbc.password}"></property>
	</bean>
	<!-- JdbcTemplate模板 -->
	<bean id="temp" class="org.springframework.jdbc.core.JdbcTemplate">
		<constructor-arg index="0" ref="dataSource"></constructor-arg>
	</bean>
	
	<!-- JDBC事务管理平台 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
	<!-- 事务切面 -->
	<tx:advice transaction-manager="txManager" id="txAdvice">
		<tx:attributes>
            <!--给哪些方法加事务-->
			<tx:method name="save*" isolation="REPEATABLE_READ" propagation="REQUIRED"/> 
			<tx:method name="add*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
			<tx:method name="remove*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
			<tx:method name="modify*" isolation="REPEATABLE_READ" propagation="REQUIRED"/>
			<tx:method name="transfer*" isolation="REPEATABLE_READ" propagation="NESTED"/>
			<tx:method name="test*" isolation="REPEATABLE_READ" propagation="NOT_SUPPORTED"/>
			<tx:method name="*" read-only="true"/>
		</tx:attributes>
	</tx:advice>
	
	<!-- AOP事务：把事务切面织入到Service层 -->
	<aop:config>
		<aop:pointcut expression="execution(public * xml.service.*.*(..))" id="txPointcut"/>
		<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
	</aop:config>
	
</beans>
```

```java 
//测试

@RunWith(value=SpringJUnit4ClassRunner.class)
@ContextConfiguration(value="classpath:applicationContext.xml")
public class TestAccount {

	@Autowired
	AccountService ser;
	
	@Test
	public void testTransfer(){
        //事务切入成功
		ser.transfer(1, 2, 100);
	}
}
```

##### 2.2tx标签介绍

```xml
<tx:advice transaction-manager="引入事务管理平台" id="txAdvice"></tx:advice>
1.tx:advice  用于配置事务相关信息；  其中tansaction-manager属性是引入对应类型的事务管理；
2.tx:attributes  用于配置哪些方法可以作为事务方法（为后面切点进行补充）
	<tx:method></tx:method>
3.tx：method  设置具体要添加事务的方法和其他属性。
	1）name是必须的，表示与事务属性关联的方法名（业务方法名），对切入点进行细化。通配符*可以用来指定一批关联到相同的事务属性的方法。
       如：get*   handle*   等等...
	2）propagation不是必须的，默认值是REQUIRED表示事务传播行为，包括REQUIRED,SUPPORTS,MANDATORY,REQUIRES_NEW,NOT_SUPPORTED,NEVER,NESTED
	3）isolation 不是必须的  默认值是DEFAULT
	4)timeout 不是必须的 默认值-1（永不超时），表示事务超时时间（以秒为单位）
	5）read-only 不是必须的 默认值false不是只读的，表示事务是否只读事务
	6）rollback-for 不是必须的  表示将被触发进行回滚的Exception；以逗号分开，默认对RunTimeException进行回滚；
	7）no-rollback-for 不是必须的，表示不被触发进行回滚的WException；以逗号分开。
```

##### 2.3注解方式配置事务

```xml
步骤如下：
1.对命名空间进行改造
	xmlns:tx="http://www.springframework.org/schema/tx"
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd
2.开启支持注解事务的驱动
	<tx:annotation-driven transaction-manager="引入事务管理平台"/>
3.在相应service层方法上使用@Transactional进行标注，以说明该方法需要事务
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd" >

	<!-- 组件扫描 -->
	<context:component-scan base-package="anno"></context:component-scan>
	<!-- 加载外部properties属性文件 -->
	<context:property-placeholder location="db.properties"/>
	
	
	<!-- C3p0数据源创建 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driver}"></property>
 		<property name="jdbcUrl" value="${jdbc.url}"></property>
 		<property name="user" value="${jdbc.username}"></property>
 		<property name="password" value="${jdbc.password}"></property>
	</bean>
	
	<!-- JdbcTemplate模板 -->
	<bean id="temp" class="org.springframework.jdbc.core.JdbcTemplate">
		<constructor-arg index="0" ref="dataSource"></constructor-arg>
	</bean>
	
	<!-- JDBC事务管理平台 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
	<!-- 开启支持事务注解的驱动 -->
	<tx:annotation-driven transaction-manager="txManager"/>
</beans>
```

```java
//dao层
@Repository
public class AccountDao {
	
	@Autowired
	private JdbcTemplate temp;

	/**
	 * 转入
	 * @param toId
	 * @param money
	 */
	public void in(int toId,int money){
		
		temp.update("update account set balance=balance+? where id = ?", money,toId);
	}
	/**
	 * 转出
	 * @param fromId
	 * @param money
	 */
	public void out(int fromId,int money){
		
		temp.update("update account set balance=balance-? where id = ?", money,fromId);
	}
}
```

```java
//service

@Service
@Transactional(readonly=true)
public class AccountService {

	@Autowired
	private AccountDao dao;
	
    //注意：方法上的@Transactional会境境覆盖类上的@Transactional配置
	@Transactional(isolation=Isolation.READ_COMMITTED,propagation=Propagation.REQUIRED)
	public void transfer(int fromId,int toId,int money){
		
		dao.out(fromId, money);
		
		System.out.println(1/0);
		
		dao.in(toId, money);
		
	}
	
}
```

```java
//测试
@RunWith(value=SpringJUnit4ClassRunner.class)
@ContextConfiguration(value="classpath:applicationContext-anno.xml")
public class TestAccountAnno {

	@Autowired
	AccountService ser;
	
	@Test
	public void testTransfer(){
		ser.transfer(1, 2, 100);
	}
}
```

