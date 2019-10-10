### 一、SSM整合步骤

```java
1.开发环境
	jdk1.8，tomcat7,mysql,Eclipse
2.创建maven工程，按照MVC设计模式进行模块划分
3.引入jar包
4.在resources目录下引入配置文件
	spring.xml
	springmvc.xml
	mybatis-config.xml
	db.properties
	log4j.properties
5.在web.xml文件中配置
	spring的监听器
	springmvc的前端处理器
```

### 二、整合示例

#### 1.开发环境

```
jdk1.8，tomcat7,mysql,Eclipse
```

#### 2.创建maven工程，按照MVC设计模式进行模块划分

```
model	controller	service		dao		util
```

#### 3.引入jar包

```xml
<!--缺少文件上传及JSON转换的包-->
<dependencies>
<!-- spring -->
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
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<!--与hibernate等orm框架整合时，支持包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<!-- springMVC -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<!--日志包 -->
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
<!--单元测试包 -->	
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.9</version>
    <scope>test</scope>
</dependency>
<!-- servlet相关包 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
<!-- JSTL -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
<!-- mysql -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.40</version>
</dependency>
<!-- c3p0 -->
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.2</version>
</dependency>
<!-- mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.5</version>
</dependency>
<!-- mybatis-spring，spring和mybatis整合的jar包支持-->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.0</version>
</dependency>
<!-- 二级缓存EHCache,支持分布式 -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.1.0</version>
</dependency>
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache-core</artifactId>
    <version>2.6.8</version>
</dependency>
<!-- javax.servlet.jsp-api -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.2</version>
    <scope>provided</scope>
</dependency>
</dependencies>
<!-- tomcat插件 -->
<build>
<plugins>
    <plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
    <url>http://127.0.0.1:8080/manager/text</url>
    <port>8888</port>
    <uriEncoding>utf-8</uriEncoding>
    <path>/ssm_demo</path>
    </configuration>
    </plugin>
</plugins>
</build>
```

#### 4.引入资源文件

**mybatis-config.xml文件**

```xml 
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 全局功能配置 -->
	<settings>
		<!-- 指定日志厂商为LOG4J -->
		<setting name="logImpl" value="LOG4J"/>
	</settings>
	
	<!-- 别名配置 -->
	<typeAliases>
		<package name="model"/>
	</typeAliases>
	
</configuration>
```

**db.properties文件**

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///javaee
jdbc.username=root
jdbc.password=tiger
```

**log4j.properties文件**

```properties
log4j.rootLogger=debug,A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss,SSS} [%t] [%c]-[%p] %m%n
```

**springmvc.xml文件**

```xml
<?xml version='1.0' encoding='UTF-8'?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- 控制器的扫描由springmvc来完成，其他组件扫描由spring完成 -->
	<context:component-scan base-package="controller"></context:component-scan>
	<!--springmvc注解驱动-->
	<mvc:annotation-driven></mvc:annotation-driven>
	<!--springmvc视图解析器-->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 静态资源处理 :location匹配url中现的字符，mapping表示直接映射到相应文件夹下的资源-->
	<mvc:resources location="/static/" mapping="/static/**"></mvc:resources>
	<mvc:resources location="/imgs/" mapping="/imgs/**"></mvc:resources>
	<mvc:resources location="/css/" mapping="/css/**"></mvc:resources>
	<mvc:resources location="/js/" mapping="/js/**"></mvc:resources>
	
</beans>
```

**applicationContext.xml文件**

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
	<context:component-scan base-package="service"></context:component-scan>
	
	<!-- 加载外部properties属性文件 -->
	<context:property-placeholder location="classpath:db.properties"/>
	
	
	<!-- C3p0数据源创建 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driver}"></property>
 		<property name="jdbcUrl" value="${jdbc.url}"></property>
 		<property name="user" value="${jdbc.username}"></property>
 		<property name="password" value="${jdbc.password}"></property>
	</bean>
	
	
	<!-- SqlSessionFactoryBean来创建sqlSession -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!--生成connection对像 -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 引入mybatis-config.xml文件 -->
		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
		<!-- 引入mapper文件 -->
		<property name="mapperLocations">
			<list>
				<value>classpath:mapper/*.xml</value>
			</list>
		</property>
	</bean>
	
	<!-- mapper接口代理对像创建由spring完成 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- 扫描mapper包下接口，生成代理对像 -->
		<property name="basePackage" value="mapper"></property>
		<!-- 引入sqlSessionFacotry -->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
	</bean>
	
	

	<!-- JDBC事务管理平台 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	
<!-- 开启支持事务的驱动proxy-target-class如果true表不CGlib创建代理对像，false表示使用JDK创建 -->
	<tx:annotation-driven transaction-manager="txManager"/>
</beans>
```

**web.xml文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">
  <display-name>day14_ssm</display-name>

  <!-- spring监听器，初始化spring容器 -->
  <listener>
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <context-param>
	  	<param-name>contextConfigLocation</param-name>
	  	<param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  
  <!-- springmvc前端处理器 -->
  <servlet>
  	<servlet-name>dispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>dispatcherServlet</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
  
  
</web-app>
```

#### 5.java代码

**model类**

```java
public class User {

	private Integer id;
	private String username;
	private String password;
	//省略setter和getter
｝
```

**mapper接口**

```java
public interface UserMapper {

	public void insert(User user) throws Exception;
	
	public void delete(User user) throws Exception;
	
	public void update(User user) throws Exception;
	
	public List<User> selectAll() throws Exception;
	
	public User selectById(User user) throws Exception;
	
	public User selectByUsername(User user) throws Exception;
}
```

**mapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper.UserMapper">
	
	<select id="selectById"  resultType="User" parameterType="User">
		select * from USER where id = #{id}
	</select>
	
	<select id="selectByUsername"  resultType="User" parameterType="User">
		select * from USER where username = #{username}
	</select>
	
</mapper>
```

**service接口**

```java

public interface IUserService {

	public void save(User user) throws Exception;
	
	public void remove(User user) throws Exception;
	
	public void modify(User user) throws Exception;
	
	public List<User> findAll() throws Exception;
	
	public User findById(User user) throws Exception;
	
	public User findByUsername(User user) throws Exception;
}

```

**service实现类 **

```java
@Service
@Transactional(isolation=Isolation.READ_COMMITTED,propagation=Propagation.REQUIRED)
public class UserService implements IUserService {
	
	@Autowired
	private UserMapper mapper;

	@Override
	public void save(User user) throws Exception {
		
		mapper.insert(user);

	}

	@Override
	public void remove(User user) throws Exception {
		
		mapper.delete(user);
	}

	@Override
	public void modify(User user) throws Exception {
		
		mapper.update(user);

	}

	@Override
	@Transactional(readOnly=true)
	public List<User> findAll() throws Exception {
		
		return mapper.selectAll();
	}

	@Override
	@Transactional(readOnly=true)
	public User findById(User user) throws Exception {
		
		return mapper.selectById(user);
	}

	@Override
	@Transactional(readOnly=true)
	public User findByUsername(User user) throws Exception {
		
		return mapper.selectByUsername(user);
	}

}

```

**controller控制器**

```java
@Controller
@RequestMapping("/user")
public class UserController {

	@Autowired
	private IUserService ser;
	
	@RequestMapping("/tologin.do")
	public String tologin(){
		
		return "login";
	}
	
	@RequestMapping("/login.do")
	@ResponseBody
	public String doLogin(User user,HttpSession session) throws Exception{
		
		User findUser = ser.findByUsername(user);
		
		if(findUser !=null){
			if(findUser.getPassword().equals(user.getPassword())){
				//允许登录
				session.setAttribute("sessionUser", findUser);
				
				return Constants.LOGIN_SUCCESS;
			}
		}
		
		return Constants.LOGIN_FAIL;
	}
	
	@RequestMapping("/index.do")
	public String toIndex(){
		
		return "index";
	}
}
```

**jsp页面**

```xml
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>

</head>
<body>

<h3></h3>
<form action="#" method="post" id="myform">

	用户名：<input type="text" name="username" value="${username}">
	<br>
	
	密码：<input type="password" name="password" value="${password}">
	
	<br>
	
	<input type="button" value="登录" id="btn">


</form>

<script type="text/javascript" src="${pageContext.request.contextPath}/js/jquery-1.11.1.js"></script>
<script type="text/javascript">

	$(function(){
		
		$("#btn").click(function(){
		
            //序列化表单
			var param = $("#myform").serialize();
			
			$.ajax({
				url:"${pageContext.request.contextPath}/user/login.do",
				data:param,
				dataType:"text",
				type:"post",
				success:function(rec){
					if(rec=="0"){
						location.href="${pageContext.request.contextPath}/user/index.do"
					}else{
						$("h3").html("用户名或密码错误")
					}
				}
				
			});
			
		});
		
	})
</script>
</body>
</html>
```

