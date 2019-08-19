### 一、SpringMVC的介绍

#### 1.SpringMVC的介绍

```java
1.对servlet轻量级封装，底层完全基于servlet的实现；
2.是web层应用的框架，基于MVC设计理念，采用了松散耦合，可插拔的组件结构，代码具有良好的扩展性和可维护性；
3.是spring框架的一个单独模块；
```

#### 2.SpringMVC的重要组件

```java
1.DispatcherServlet
	前端控制器，对URL请求进行统一调度和分发，是整个SpringMVC框架的控制中心，（不做请求的处理）
2.HandlerMapping
	处理器映射器，根据一定的映射规则，用来映射后端控制器（Controller|Handler），把请求交由后端控制器处理；
3.HandlerAdaptor
	处理器是适配器，调用后端控制器来处理Http请求，通过扩展处理器适配器，支持更多类型的处理器；
4.ViewResolver
	视图解析器，用来解析ModelAndView对象（封装逻辑视图名及模型数据），返回真正的视图，渲染数据；
```

#### 3.SpringMVC的执行流程

```java
1.整个过程始于客户端发出的一个HTTP请求，web应用服务器接收到这个请求，如果匹配DispatcherServlet的请求映射路径（web.xml中指定），则web容器将该请求转交给DispatcherServlet处理、分发；
2.接收到这个请求后，将根据请求中的信息（包括URL，HTTP方法，请求头，请求参数，cookie等）及HandlerMapping的配置找到处理请求的Handler（处理器）；
可将HandlerMapping看作路由控制器，将Handler看作目标主机；
3. 当DispatcherServlet根据HandlerMapping得到对应当前请求的Handler后,通过HandlerAdapter对Handler
的封装,再以统一的适配器接口调用Handler。HandlerAdapter是Spring MVC的框架级接口,顾名思
义,HandlerAdapter是一个适配器,它用统一的接口对各种Handler的方法进行调用.
4. 处理器完成业务逻辑的处理后将返回一个ModelAndView给DispatcherServlet,ModelAndView包含了视图逻
辑名和模型数据信息。
5. ModelAndView中包含的是"逻辑视图名"而并非真正的视图对象,DispatcherServlet借由ViewResolver完成逻
辑视图名到真实视图对象的解析工作。
6. 当得到真实的视图对象View后,DispatcherServlet就使用这个View对象对ModelAndView中的模型数据进行视
图渲染。
7. 最终客户端得到的响应信息可能是一个普通的HTML页面,也可能是一个XML或者JSON串,甚至是一张图片或者
一个PDF文档等不同的媒体形式。
```

### 二、springmvc环境搭建

#### 1.组件开发

```java
1.dispatchservlet（框架提供）：作用：接收请求，响应结果；相当于转发器，中央处理器。
2.handlerMapping（框架提供）：根据请求的URL查找handler，handlerMapping负责根据用户请求找到handler，SpringMVC提供了不同的映射器实现不同的映射方式，例如：配置文件方式、实现接口方式、注解方式；
3.handlerAdaptor（框架提供）：作用：按特定的规则去执行handler；
4.handler(需要工程师开发)：编写handler时按照handlerAdaptor的要求去做，这样适配器才能正常执行Handler,handler是后端控制器，对具体的用户请求做处理；
5.view resolver(框架提供)：作用：进行视图解析，把逻辑视图名解析成真正的视图（view）
6.view（需要工程师开发）：view是一个接口，实现类支持不同的view类型（jsp、freemarker、PDF...）
```

#### 2.步骤

```xml
1.引入spring-webmvc.jar包
2.在web.xml文件中配置DispatcherServlet对象
3.在springmvc.xml文件中配置HandlerMapping,HandlerAdaptor,ViewResolver
4.编写后端控制器
5.编写视图
```

##### 2.1引入jar包

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>3.8.1</version>
    <scope>test</scope>
</dependency>
<!--servlet-api-->
<dependency>
    <groupId>javax.servlet</groupId>
	<artifactId>javax.servlet-api</artifactId>
	<version>3.1.0</version>
	<scope>provided</scope>
</dependency>
<!-- 添加Spring包 -->
	<!--springmvc的jar包-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<!-- 为了方便进行单元测试，添加spring-test包 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>4.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>4.2.8.RELEASE</version>
    </dependency>
<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjweaver</artifactId>
		<version>1.8.10</version>
</dependency>
<!-- jstl -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

##### 2.2在web.xml文件中配置dispatchServlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" version="2.5">
  <display-name>day11_springmvc</display-name>
  
  
  <!-- 前端处理器 -->
  <servlet>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--加载springmvc.xml文件-->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  </servlet>
  
  <!-- url-pattern匹配路径规则：
  1  *.后缀名:用来匹配特定后缀名的url，如*.do,*.htm,*.action
  2  /: 用来匹配除了.jsp以外的其他所有url
  3  /xxx/*.后缀：用来匹配某个特定路径下相应后缀名的url
   注意：不能使用/*,因为会匹配.jsp后缀，被误认为是控制器-->
  <servlet-mapping>
  	<servlet-name>DispatcherServlet</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>

</web-app>
```

##### 2.3在springmvc.xml文件中配置组件

```xml
<?xml version='1.0' encoding='UTF-8'?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">


	<!-- HandlerMapping处理器映射器:会根据url的后缀去匹配相应控制器的name-->
	<bean id="handlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
	
	<!-- HandlerAdaptor处理器适配器：调用实现了Controller接口的后端控制器 -->
	<bean id="handlerAdapter" class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
	
	<!-- ViewResolver视图解析器  index逻辑视图名，会拼接前缀和后缀，生成/WEB-INF/jsp/index.jsp真正的视图 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- controller对像 -->
	<bean name="/index.do" class="controller.IndexController"></bean>

</beans>
```

##### 2.4后端控制器

```java
//控制器需实现Controller接口，会被SimpleControllerHandlerAdapter适配器适配到
public class IndexController implements Controller{

	@Override
	public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
		
		System.out.println("IndexController被访问");
		
		ModelAndView mv = new ModelAndView("index");//逻辑视图

		//封装数据
		mv.addObject("username", "admin");
		
		return mv;
	}
}
```

##### 2.5视图

```jsp
<!--在WEB-INF文件夹下创建index.jsp页面-->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
欢迎你${username}
</body>
</html>
```

##### 2.6发送HTTP请求

```xml
在浏览器输入http://localhost:8080/day11_springmvc/index.do
其中/index.do的后缀会被DispathcerServlet对像匹配到，按照BeanNameUrlHandlerMapping规则去映射后端控制器，url最后的字符串/index.do会被作为后端控制器的bean的name去匹配。如果在spring容器中有bean的名字，如下<bean name="/index.do" class="controller.IndexController"></bean>
则能成功映射到控制器，否则，会抛出404异常。
```

### 三、springmvc环境搭建二

#### 1.步骤

```java
1.引入spring-webmvc.jar包（同上）
2.在web.xml文件中配置DispatcherServlet对象（同上）
3.在Springmvc.xml文件中配置HandlerMapping,HandlerAdaptor,ViewResolver
4.编写后端控制器(实现HttpRequestHandler)
5.编写视图（同上）
```

##### 1.1在springmvc.xml文件中配置组件

```xml
<?xml version='1.0' encoding='UTF-8'?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">


	<!-- HandlerMapping处理器映射器:映射url最后的字符串，映射到key -->
	<bean id="handlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
                <!--myindex指的是控制器bean的name值-->
				<prop key="/index.do">myindex</prop>
				<prop key="/index2.do">myindex</prop>
				<prop key="/index3.do">myindex</prop>
			</props>
		</property>
	</bean>
	
	<!-- HandlerAdaptor处理器适配器:控制器需实现HttpRequestHandler接口 -->
	<bean id="handlerAdapter" class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
	
	<!-- ViewResolver视图解析器  index逻辑视图名:/WEB-INF/jsp/index.jsp真正的视图 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 后端控制器 -->
	<bean name="myindex" class="controller.IndexController2"></bean>
	
</beans>
```

##### 1.2控制器

```java
//该控制器必须实现HttpRequestHandler接口
//返回Servlet编程,视图解析器将不会生效
public class IndexController2 implements HttpRequestHandler{

	@Override
	public void handleRequest(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		
		System.out.println("IndexController2被访问");

		request.setAttribute("username", "admin");
		
		request.getRequestDispatcher("/WEB-INF/jsp/index.jsp").forward(request, response);
	}

}
```

##### 1.3发送http请求

```
http://localhost:8080/day11_springmvc/index.do
http://localhost:8080/day11_springmvc/index2.do
http://localhost:8080/day11_springmvc/index3.do
都可以请求到统一个控制器
```

###四、springmvc环境搭建三（注解方式）

#### 1.步骤

```
1.引入Spring-webmvc.jar(同上)
2.在web.xml文件中配置DispatcherServlet对象（同上）
3.在Springmvc.xml文件中配置HandlerMapping、HandlerAdapter,ViewResolver
4.编写后端控制器
5.编写视图（同上）
```

##### 1.1在Springmvc.xml文件中配置组件

```xml
<?xml version='1.0' encoding='UTF-8'?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="
http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">

    <!--注意：使用Springmvc注解时，必须同时使用IOC注解来创建Controller对像-->
	<context:component-scan base-package="controller"></context:component-scan>
	
	<!-- HandlerMapping处理器映射器:支持springmvc注解-->
	<bean id="handlerMapping" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
	
	<!-- HandlerAdaptor处理器适配器:支持springmvc注解 -->
	<bean id="handlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
	
	<!-- ViewResolver视图解析器  index逻辑视图名:/WEB-INF/jsp/index.jsp真正的视图 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
</beans>
```

**也可以使用下面更简单的驱动方式**

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

	<!-- IOC -->
	<context:component-scan base-package="controller"></context:component-scan>
    
<!-- Springmvc注解驱动:会创建RequestMappingHandlerMapping和RequestMappingHandlerAdapter对像-->
	<mvc:annotation-driven></mvc:annotation-driven>
	
	<!-- ViewResolver视图解析器  index逻辑视图名:/WEB-INF/jsp/index.jsp真正的视图 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
</beans>
```

##### 1.2控制器

```java
//使用注解方式时，普通的类都可以作为Springmvc的控制器
@Controller
//一般这里配置模块名
@RequestMapping("/user")
public class IndexController3 {
	
    //进一步窄化请求
	@RequestMapping("/dobiz.do")
	public ModelAndView doBiz(){
		ModelAndView mv = new ModelAndView();
		mv.setViewName("index");     //这两步可以合并为ModelAndView mv = new ModelAndView("index");
		mv.addObject("username", "admin");
		System.out.println("doBiz()方法被访问");
		return mv;
	}
	
	@RequestMapping("/dobiz2.do")
	public ModelAndView doBiz2(){
		ModelAndView mv = new ModelAndView();
		mv.setViewName("index");
		mv.addObject("username", "admin2");
		System.out.println("doBiz2()方法被访问");
		return mv;
	}
}
```

##### 1.3发送HTTP请求

```
http://localhost:8080/day11_springmvc/user/dobiz.do
http://localhost:8080/day11_springmvc/user/dobiz2.do
可以分别访问到上述控制器中的两个方法
```

### 五、springmvc源码解析

```java
//前端控制器分派方法
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws
Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    int interceptorIndex = -1;
try {
	ModelAndView mv;
	boolean errorView = false;
try {
    //检查是否是请求是否是multipart（如文件上传），如果是将通过MultipartResolver解析
    processedRequest = checkMultipart(request);
    //步骤2、请求到处理器（页面控制器）的映射，通过HandlerMapping进行映射
    mappedHandler = getHandler(processedRequest, false);
    if (mappedHandler == null || mappedHandler.getHandler() == null) {
    	noHandlerFound(processedRequest, response);
   		return;
    }
    //步骤3、处理器适配，即将我们的处理器包装成相应的适配器（从而支持多种类型的处理器）
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
    // 304 Not Modified缓存支持
    //此处省略具体代码
    // 执行处理器相关的拦截器的预处理（HandlerInterceptor.preHandle）
    //此处省略具体代码
    // 步骤4、由适配器执行处理器（调用处理器相应功能处理方法）
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
    // Do we need view name translation?
    if (mv != null && !mv.hasView()) {
    	mv.setViewName(getDefaultViewName(request));
    }
	// 执行处理器相关的拦截器的后处理（HandlerInterceptor.postHandle）
	//此处省略具体代码
}
catch (ModelAndViewDefiningException ex) {
	logger.debug("ModelAndViewDefiningException encountered", ex);
	mv = ex.getModelAndView();
}
catch (Exception ex) {
    Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
    //处理控制器发生的异常
    mv = processHandlerException(processedRequest, response, handler, ex);
    errorView = (mv != null);
}
//步骤5 步骤6、解析视图并进行视图的渲染
//步骤5 由ViewResolver解析View（viewResolver.resolveViewName(viewName, locale)）
//步骤6 视图在渲染时会把Model传入（view.render(mv.getModelInternal(), request,response);）
if (mv != null && !mv.wasCleared()) {
    //处理ModelAndView
	render(mv, processedRequest, response);
if (errorView) {
	WebUtils.clearErrorRequestAttributes(request);
}
}
else {
    if (logger.isDebugEnabled()) {
        logger.debug("Null ModelAndView returned to DispatcherServlet with name '" +
        getServletName() +
        "': assuming HandlerAdapter completed request handling");
    }
}
// 执行处理器相关的拦截器的完成后处理（HandlerInterceptor.afterCompletion）
//此处省略具体代码
catch (Exception ex) {
// Trigger after-completion for thrown exception.
	triggerAfterCompletion(mappedHandler, interceptorIndex, processedRequest, response,
ex);
	throw ex;
}
catch (Error err) {
	ServletException ex = new NestedServletException("Handler processing failed", err);
	triggerAfterCompletion(mappedHandler, interceptorIndex, processedRequest, response,
ex);
	throw ex;
}
finally {
    if (processedRequest != request) {
    	cleanupMultipart(processedRequest);
    }
}
}
```

### 六、springmvc常用注解

#### 1.常用注解

```java
1.@RequestMapping	用来匹配Url请求
2.@RequestParam	做表单的参数处理
3.@PathValue	获取路径参数（处理RESTful风格的URL请求）
4.@CookieValue	取出cookie中的值
5.@RequestHeader	设置请求头
6.@ResponseBody	把java对象转化为json字符串
7.@RequestBody	把json字符串转化为java对象
```

#### 2.@RequestMapping

##### 2.1常用属性

```
1.value属性
	value="/匹配url的字符串"，一般value字会串可以省略，可以直接使用值,如下
	@RequestMapping(value="/user")等同于@RequestMapping("/user")
2.method属性
	用来限制对该方法的请求方式（post|get|delete|put）
3.params属性
	可以对请求的参数作限定，语法如下：
	param:表示请求必须包含名为param的请求参数
 	!param:表示请求中不能包含名为param的参数
    param != value:表示请求中包含param的请求参数,但是值不能为value
    param == value:表示请求中包含param的请求参数,但是值为value
```

#### 3.@RequestParam

用在控制器方法形参前面进行标注

##### 3.1常用属性

```
1.value属性
	如果方法形参名和请求中的参数名相同,无需使用@RequestParam注解!
	否则，则使用value属性重新指定参数名，和方法中的形参进行绑定。
2.defaultValue属性
	如果请求的参数值为null，可以为该参数付默认值。
3.required属性
	用来限制请求中的参数是否必须出现。true表示该请求中必须有该参数，false表示不作强制要求
	注意：如果使用defaultValue，则required将不会起到限制作用。
```

#### 4.@CookieValue

##### 4.1使用步骤

```java
1.标注在方法形参前面
2.通过value属性获对应cookie的值
```

##### 4.2代码

```java
//向浏览器写入cookie
@RequestMapping("/addcookie.do")
	public ModelAndView doBiz(HttpSession session){
		//会向cookie写入JSESSIONID标识
		session.setAttribute("username", "user");
		ModelAndView mv = new ModelAndView("index");
		return mv;
	}

//获取浏览器中指定的cookie
@RequestMapping("/cookie.do")
	public ModelAndView doBiz2(@CookieValue(value="JSESSIONID") String val){
		System.out.println(val);
		ModelAndView mv = new ModelAndView("index");
		mv.addObject("username", "admin");
		return mv;
	}
```

#### 5.@RequestHeader

##### 5.1使用步骤

```java 
1.标注在控制器方法形参上，用来获取请求头
2.通过key获取value值，和@CookieValue用法相似
```

##### 5.2代码

```java
//获取http请求头中的信息
public ModelAndView doBiz(@RequestHeader(value="Cookie") String val){
    System.out.println(val);
    ModelAndView mv = new ModelAndView("index");
    mv.addObject("username","admin");
    return mv;
}
```

### 七、返回值类型

#### 1.常用的返回值类型

```java
1.void	返回servlet编程，页面跳转只能通过请求转发或重定向
2.String	一般返回逻辑视图名，如果加@ResponseBody则返回字符串，一般用Model封装数据
3.Pojo类型及集合类型	返回json数据
```

### 八、springmvc乱码解决

#### 1.get请求乱码解决

```xml
1.服务器编码，工程编码，jsp视图编码一致，则一般不会产生get乱码。
	服务器编程设置，server.xml文件如下设置即可：
<Connector URIEncoding="utf-8" connectionTimeout="20000" port="8080"  protocol="HTTP/1.1" redirectPort="8443"/>
也可以使用 String newStr = new String(byte[] ,"编码格式");
```

#### 2.post请求乱码

```xml
在web.xml文件配置过滤器
  <!-- Spring进行编码管理的过滤器 -->
  <filter>
  	<filter-name>encodingFilter</filter-name>
  	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	<init-param>
        <!--设置编码格式-->
  		<param-name>encoding</param-name>
  		<param-value>utf-8</param-value>
  	</init-param>
  	<init-param>
  		<param-name>forceEncoding</param-name>
  		<param-value>true</param-value>
  	</init-param>
  </filter>
  
  <filter-mapping>
  	<filter-name>encodingFilter</filter-name>
     <!--拦截所有路径-->
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
  
```

