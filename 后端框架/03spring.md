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
	singleton：spring容器将以单例的方式创建该对象；
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

