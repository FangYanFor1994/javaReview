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

### 九、springmvc数据传递

```
数据传递：把控制器数据传递到jsp页面等视图中；
```

####1.常用封装数据的对象

```java
1.HttpServletRequest
2.ModelAndView
3.Model
4.ModelMap
5.Map

说明：ModelMap本质是Map集合
	Model是接口，其实现类ExtendedModelMap,而ExtendedModelMap是ModelMap的子类，所以ExtendedModelMap本质也是Map集合，而ModelAndView对象也封装了ModelMap对象，最终，数据都是存放在Map中。
```

#### 2.Model等对象和HttpServletRequest的联系（源码）

```java
//原码如下：
//说明：源码中遍历Map集合，把里面的元素全部放进httpServletRequest域对象中，才得以传递到前端
protected void exposeModelAsRequestAttributes(Map<String, Object> model, HttpServletRequest request) throws Exception {
    	//去迭代 model中的数据
		for (Map.Entry<String, Object> entry : model.entrySet()) {
			String modelName = entry.getKey();
			Object modelValue = entry.getValue();
			if (modelValue != null) {
                //封装到request域中
				request.setAttribute(modelName, modelValue);
				if (logger.isDebugEnabled()) {
					logger.debug("Added model object '" + modelName + "' of type [" + modelValue.getClass().getName() +
							"] to request in view with name '" + getBeanName() + "'");
				}
			}
			else {
				request.removeAttribute(modelName);
				if (logger.isDebugEnabled()) {
					logger.debug("Removed model object '" + modelName +
							"' from request in view with name '" + getBeanName() + "'");
				}
			}
		}
	}
```

### 十、springmvc参数绑定

#### 1.参数绑定介绍

```java
在springmvc中，接收页面提交的数据时通过方法形参来接收的，从客户端请求的key/value数据，经过参数绑定，将key/value数据绑定到controller方法的形参上，然后就可以在controller中使用该参数了。
原理：使用Convert组件完成，且springmvc提供了很多converter转换器（进行任意类型转换）
```

#### 2.参数绑定的类型

```java
1.内置类型参数绑定  
	在控制器方法的形参上使用该类型的变量，spring会自动创建相应类型的对象绑定到该形参上（和参数名无关）；  例：HttpServletRequest/HttpServletResponse/HttpSession/Model/ModelMap/Map
2.简单类型参数绑定
	8种基本类型数据（及包装类）及String类型数据；  请求参数名必须和方法形参名称一致，则会自动绑定数据到该方法形参上。
3.数据类型参数绑定
	请求参数名必须和方法形参名称一致，则自动绑定数据到该方法形参上。
	public String doBiz2(String[] arr)
    http://localhost:8080/day12_springmvc/user4/bind2.do?arr=admin&arr=user
4.pojo类型参数绑定
	http请求中的key要和pojo的setter方法的后缀一致，即可完成参数绑定；
5.复杂pojo类型参数绑定
	pojo中有属性为addr的pojo类型属性，且addr中有id属性，通过addr.id可以为其绑定值
	http://localhost:8080/day12_springmvc/user4/bind5.do?id=1&addr.id=1000
6.集合参数绑定list
	springmvc不支持集合类型参数直接绑定形参上，可以把集合类型封装到pojo中，则可以完成参数绑定
	 //集合类型属性
	private List<Address> addrs;
	地址1：<input type="text" name="addrs[0].addrname">
	说明：addrs为pojo中list集合的属性名称，通过addrs[0].addrname可以为集合的第一个元素中的addrname属性绑定值
7.集合参数绑定Map
	   //集合类型属性
	private Map<String,Address> map;
	 <!--map集合的key可以自定义-->   
	map[0]：<input type="text" name="map['key1'].addrname">
```

**8.特殊类型参数绑定**

```java
//需求：
如果传到控制器方法日期格式与springmvc默认日期不一致，会导致参数绑定失败，这式需要自定义自己的日期参数绑定器。
//步骤
1.定义参数绑定器类，该类要实现convert接口，重写绑定规则
2.使用FormattingConversionServiceFactoryBean来创建convert对象
```

```java
//自定义参数绑定器
public class MyDateConverter implements Converter<String, Date>{

	@Override
	public Date convert(String source) {
		//source表示传递到控制器的参数，一般String类型
		System.out.println(source);
		try {
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
			Date date = sdf.parse(source);
			return date;
		} catch (Exception e) {
			return new Date();
		}
		
	}
}
```

```xml
//在springmvc.xml文件中配置参数绑定器
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

	

	<!--把自定义参数绑定器载入springmvc环境：请使用conversion-service属性-->
	<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
	
	<!-- 自定义参数绑定器 -->
	<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
		<property name="converters">
			<set>
				<bean class="util.MyDateConverter"></bean>
			</set>
		</property>
	</bean>
</beans>
```

```java
//控制器
	/*
	 * 9.日期类型:自定义参数绑定器
	 */
	@RequestMapping("/bind9.do")
	public String doBiz9(Date date){
		
		System.out.println(date);
		
		return "index";
	}
	
```

```java
//请求的url
http://localhost:8080/day12_springmvc/user4/bind9.do?date=2010-10-10
```

**注意：如果只是关于日期的转化，可以使用@DateTimeFormat注解重新定义日期格式**

```java
public class User {
	
	@DateTimeFormat(pattern="yyyy-MM-dd")
	private Date birthday;
  
    //省略getter和setter方法
}
```

```java
   //控制器
	/*
	 * 10.日期类型:把日期类型封装到pojo中
	 */
	@RequestMapping("/bind10.do")
	public String doBiz10(User user){
		
		System.out.println(user.getBirthday());
		
		return "index";
	}
```

```java
//请求的url
http://localhost:8080/day12_springmvc/user4/bind9.do?birthday=2010-10-10
```

### 十一、RESTful风格编程

#### 1.RESTful概念

```java
REST:即Representational State Transfer , (资源)表现层状态转化,是目前最流行的一种互联网软件架构。它结构清晰、符合标注、易于理解、方便扩展，所以越来越多的网站采用！
具体说，就是HTTP协议里面,四个表示操作方式的动词:GET POST PUT DELETE
它们分别代表着四种基本操作（一般放在请求头中）:
1.GET用来获取资源
2.POST用来创建新资源
3.PUT用来更新资源
4.DELETE用来删除资源
另外，RESTful对URL链接也有要求：
URL链接上不能出现动词，也不能出现除了/和名词之外的其他字符，如.?&=都不允许出现！
所以，一个符合标准RESTful风格的url如下：
http://localhost:8080/工程名/资源位置/参数1/参数2/
```

#### 2.实现步骤（get方式）

```
1.DispatcherServlet的拦截路径使用/表示
2.组织符合RESTful风格的URL链接
3.在controller的方法上使用注解@PathVariable注解标注参数
```

**代码**

```xml
<!--DispatcherServlet的拦截路径使用/表示-->

<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

```java
@Controller
@RequestMapping("/rest")
public class RestController {

    //@PathVariable作用：把路径变量和方法形参进行绑定
	@RequestMapping(value="/test1/{id}/{username}",method=RequestMethod.GET)
	public String doRest(@PathVariable("id")int id,@PathVariable("username")String username){
		
		System.out.println(id+","+username);
		
		return "index";
	}
}
```

```java
//符合RESTful风格的url请求（发送的是get请求）：
http://localhost:8080/day13_springmvc/rest/test1/1/admin
```

#### 3.实现步骤（post方式）

```xml
1.在web.xml文件配置过滤器
    <filter>
    	<filter-name>HiddenHttpMethodFilter</filter-name>
    	<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
    	<filter-name>HiddenHttpMethodFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping>	
2.使用表单，且表单的method="post"发送http请求

3.在表单中必须有隐藏域：
	<input type="hidden" name="_method" value="GET|POST|PUT|DELETE"></input>
```

**代码**

```java
@Controller
@RequestMapping("/rest2")
public class RestController2 {
	
	@RequestMapping("/torest")
	public String toRest(){
		
		return "rest";
	}
	
	// 查寻
	@RequestMapping(method = RequestMethod.GET)
	public String doGet(int id) {

		System.out.println("get");
		System.out.println(id);

		return "index";
	}
	// 增
	@RequestMapping(method = RequestMethod.POST)
	public String doPost(int id, String username) {

		System.out.println("post");
		System.out.println(id + "," + username);

		return "index";
	}
	// 删
	@RequestMapping(method = RequestMethod.DELETE)
	public String doDelete(int id) {

		System.out.println("delete");
		System.out.println(id);

		return "index";
	}

	// 改
	@RequestMapping(method = RequestMethod.PUT)
	public String doDelete(int id, String username) {

		System.out.println("put");
		System.out.println(id + "," + username);

		return "index";
	}
}
```

```xml

<!-- get请求 -->
<form action="${pageContext.request.contextPath}/rest2" method="post">
	ID:<input type="text" name="id" >
    <!--隐藏域的name必须为_method-->
	<input type="hidden" name="_method" value="GET"></input>
	
	<input type="submit" value="get">
</form>

<!-- post请求 -->
<form action="${pageContext.request.contextPath}/rest2" method="post">
	ID：<input type="text" name="id" >
	USRNAME:<input type="text" name="username" >
    <!--隐藏域的name必须为_method-->
	<input type="hidden" name="_method" value="POST"></input>
	<input type="submit" value="post">
</form>

<!-- put请求 -->
<form action="${pageContext.request.contextPath}/rest2" method="post">
	ID：<input type="text" name="id" >
	USRNAME:<input type="text" name="username" >
    <!--隐藏域的name必须为_method-->
	<input type="hidden" name="_method" value="PUT"></input>
	
	<input type="submit" value="put">
</form>

<!-- delete请求 -->
<form action="${pageContext.request.contextPath}/rest2" method="post">
	ID：<input type="text" name="id" >
    <!--隐藏域的name必须为_method-->
	<input type="hidden" name="_method" value="DELETE"></input>
	
	<input type="submit" value="delete">
</form>
```

### 十二、静态资源处理

```xml
当DispatcherServlet使用/来匹配URL时，会拦截静态资源，我们可以在springmvc.xml配置<mvc:resources>来处理
```

```xml
<!--静态资源处理：location匹配URL中的字符，mapping表示直接映射到相应文件夹下的资源-->
<mvc:resources location="/static/" mapping="/static/**"></mvc:resources>
<mvc:resources location="/imgs/" mapping="/imgs/**">
</mvc:resources>
<mvc:resources location="/css/" mapping="/css/**">
</mvc:resources>
<mvc:resources location="/js/" mapping="/js/**">
</mvc:resources>
```

### 十三、数据自动回显

```
Springmvc对在控制器方法形参绑定pojo类型，默认会把该pojo对象放在request域中，其中key为该pojo类型类名的小写；
```

```java
@RequestMapping("/login.do")
	public String doLogin(User u){
		//springmvc默认会执行下面操作：key为User类名的小写
		//model.addAttribute("user", u);
		return "login";
	}
```

```jsp
<form action="${pageContext.request.contextPath}/user/login.do" method="post">

	用户名：<input type="text" name="username" value="${user.username}">
	<br>
	密码：<input type="password" name="password" value="${user.password}">
	<br>
	<input type="submit" value="登录">
</form>
```

### 十四、JSON数据处理

```xml
<!--springmvc进行json数据处理时，需要以下插件包-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.8.9</version>
</dependency>

也可以引入下面包：
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.2.4</version>
</dependency>
```

```java
使用步骤：
前端通过ajax请求，发送json或键=值的数据到后台，后台通过参数绑定可以获取前端传来的值；
后端加@Responsbody注解，可以返回一个java对象或者json字符串，前端ajax接收dataType："text",可以接收字符串，dataType:"json"，可以将后端的json字符串或者java对象转化为json对象；
```

### 十五、异常处理

```
SpringMVC通过HandlerExceptionResolver的实现类进行异常的统一处理：
1.DefaultHandlerExceptionResolver(默认被始用)
2.SimpleMappingExceptionResolver
3.AnnotationMethodHandlerExceptionResolver(默认被始用)
4.ResponseStatusExceptionResolver(默认被始用)
```

#### 1.DefaultHandlerExceptionResolver进行异常处理

```xml
只需要在web.xml配置异常映射即可：
<error-page>
    <error>404</error>
    <location>/WEB-INF/jsp/404.jsp</location>
</error-page>

<error-page>
    <error>500</error>
    <location>/WEB-INF/jsp/500.jsp</location>
</error-page>
```

#### 2.AnnotationMethodHandlerExceptionResolver进行异常处理

```
支持@ExceptionHandler
    1.该注解标注在控制器的方法上
	局部异常处理：只会对该控制器的所有方法发生的异常进行处理，对别的控制器发生的异常不会处理
	2.可以标注在普通的异常处理类的相应方法上
	全局异常处理：对所有控制器内部的方法产生的异常都会做统一处理。
```

##### 2.1局部异常处理

```java
@Controller
@RequestMapping("/exce")
public class ExceptionController {

	@RequestMapping("/test.do")
	public String doBiz(){
		
		System.out.println(1/0);
		
		return "index";
	}
	
    //只对该控制器中方法抛出的异常进行统一处理。
	@ExceptionHandler
	public String HandlerException(Exception e){
		
		e.printStackTrace();
		
		return "500";//逻辑视图名
	}
}
```

##### 2.2全局异常处理

```java
//自定义类来处理全局异常，一般该类使用@ControllerAdvice来标注

/**
 * 自定义全局异常处理组件
 * @author Administrator
 */
@ControllerAdvice
public class ExceHandler {

	@ExceptionHandler
	public String HandlerException(Exception e){
		
		e.printStackTrace();

		return "100";//逻辑视图名
	}
}

```

```
如果有全局异常处理器和局部异常处理方法同时对同一异常进行处理，则优先级如下：
局部异常处理>全局异常处理

```

###十六、拦截器

#### 1.执行原理

```java
springMVC的拦截器类似于servlet中的过滤器，需要先定义一个类实现HandlerIntercepter接口！
添加未实现的方法，在springmvc配置中配置！
除了.jsp以外的资源都可以进行拦截！
```

#### 2.实现步骤

```
1.创建java类实现HandlerIntercepter接口，重写方法；
2.在springmvc.xml文件中配置该拦截器对象；
```

```java
//当访问控制器时，拦截器会被自动调用
public class LoginIntercepter implements HandlerInterceptor{
    /**
	 * 到达控制器之前执行的方法
	 * true表示调用下一个拦截器或控制器
	 * false将不会向下调用，即被拦截
	 */
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		
		/*
		 * 业务需求：访问login.jsp无需处于登录状态，但其他资源的访问需要用户处于登录状态。
		 */
		//判断该用户是否处于登录状态
		User sessionUser = (User) request.getSession().getAttribute("sessionUser");
		
		if(sessionUser !=null){
			return true;
		}
		System.out.println("preHandle");
		//重定向到login.jsp页面，让用户去登录
		response.sendRedirect(request.getContextPath()+"/user/tologin.do");
		
		return false;
	}

	/**
	 * 控制器方法执行完后将要执行的方法
	 */
	@Override
	public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			ModelAndView modelAndView) throws Exception {
		
		System.out.println("postHandle");
		
	}

	/**
	 * 控制器方法执行完后，返回返回值后将要执行的方法
	 */
	@Override
	public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
		
		System.out.println("afterCompletion");
	}
    
}
```

```xml
<!--在springmvc.xml文件中配置拦截器-->
<!-- 配置自定义拦截器 -->
	<mvc:interceptors>
		<mvc:interceptor>
			<!-- 
				/**:表示匹配所有url
				/user/*:匹配特定路径下的url
				/user/**:匹配特定路径下,更深路径的url
			 -->
			<mvc:mapping path="/**"/>
            <!--exclude-mapping表示对该url不做拦截-->
			<mvc:exclude-mapping path="/static/**"/>
			<mvc:exclude-mapping path="/imgs/**"/>
			<mvc:exclude-mapping path="/css/**"/>
			<mvc:exclude-mapping path="/js/**"/>
			<mvc:exclude-mapping path="/user/tologin.do"/>
			<mvc:exclude-mapping path="/user/login.do"/>
			<!-- 拦截器 -->
			<bean class="interceptor.LoginInterceptor"></bean>
		</mvc:interceptor>
	</mvc:interceptors>
```

```xml
<!--web.xml文件配置-->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">
	<display-name>day11_springmvc</display-name>

	

	<!-- 前端处理器 -->
	<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>


</web-app>
```

#### 3.过滤器和拦截器比较

```java
1.拦截范畴不同：
	拦截器是框架中的概念，过滤器是servlet中的概念！
2.实现原理不同：
	拦截器是基于AOP原理实现的，过滤器是基本的方法回调；
3.拦截范围不同：
	拦截器会拦截除了.jsp以为的资源，过滤器会拦截所有路径，包括jsp资源；
4.拦截粒度不同：
	过滤器是粗粒度过滤，而拦截器粒度更细，可以拦特定方法；
```

### 十七、文件上传

```java
springmvc为文件上传提供了直接支持，这种支持是通过即插即用的MultipartResolver实现，spring使用Jakarta Commons FileUpload技术实现了一个MultipartResolver实现类:CommonsMultipartResolver。
在SpringMVC上下文中默认没有装配MultipartResolver,因此默认情况下不能处理文件上传工作。如果想使用
Spring的文件上传功能,则需要先在上下文中配置MultipartResolver。
```

#### 1.实现步骤

```xml
1.引入依赖包
<dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
	</dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.4</version>
    </dependency>
2.在springMVC.xml文件中配置CommonsMutipartResolver对象，该对象名字必须是multipartResolver
<bean name="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>

3.form表单，表单提交的方法method="post",设置enctype="multipart/form-data"	使用<input type="file" name="img"></input>标签进行文件上传操作；

4.在控制器的方法形参上绑定参数MultipartFile,该参数是对文件上传内容的抽象。
@RequestMapping("/upload.do")
public String doUpload(MultipartFile img){
	//文件上传操作
	img.transferTo("");
}
```

#### 2.单个文件上传代码

```xml
<!--springmvc.xml文件配置-->

	<!-- 注意：该bean的名字必须为multipartResolver，否则会导致上传失败 -->
	<bean name="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 限制文件上传字节数 -->
		<property name="maxUploadSize" value="1000000"></property>
		<!-- 文本文件字符编码 -->
		<property name="defaultEncoding" value="utf-8"></property>
	</bean>
	
```

```java
	//控制器

	//注意：方法形参MultipartFile img，变量名img必须和file表单的name属性值一致。
	@RequestMapping("/upload.do")
	public String doUpload(MultipartFile img,HttpServletRequest request) throws IllegalStateException, IOException{
		
		//表单控制的name属性的值
		System.out.println(img.getName());
		//文件的名字
		System.out.println(img.getOriginalFilename());
		//文件大小
		System.out.println(img.getSize());
		//文件类型
		System.out.println(img.getContentType());
		
		//上传操作D盘根目录下
		//File file = new File("D:/"+img.getOriginalFilename());
		
		//上传到服务器中WebApp/uploads文件夹下
		String savepath = request.getServletContext().getRealPath("/uploads/"+img.getOriginalFilename());
		File file = new File(savepath);
        //使用transferTo方法上传到指定位置
		img.transferTo(file);
		
		return "index";
	}

//注意：上传路径中：
如果是windows系统 new File("a/b/c").getAbsolutePath()获得是相对路径 
	new File("/a/b/c").getAbsolutePath()获得的是绝对路径
如果是Linux系统new File("a/b/c").getAbsolutePath()获得是相对路径。
 	new File("/a/b/c").getAbsolutePath() 获得的是绝对路径
```

```jsp
//JSP页面
<!--文件上传时，必须设置表单method="post",enctype="multipart/form-data"-->
<form action="${pageContext.request.contextPath}/file/upload.do" method="post" enctype="multipart/form-data">

	<input type="file" name="img">
	<br>
	<input type="submit" value="文件上传">
</form>
```

#### 3.多文件上传

```java
//model类
public class User {

	private String username;
	private String password;
	//把文件封装到User类中，以集合的方式存放
	private List<MultipartFile> img;
	
    //省略getter和setter
｝
```

```java
//控制器
	@RequestMapping("/upload2.do")
	public String doUpload2(User user) throws IllegalStateException, IOException{
		
        //从user对像中取出集合数据，进行迭代上传即可
		System.out.println(user.getImg());
        
        //上传步骤省略
		
		return "index";
	}

```

```jsp
//JSP页面

<form action="${pageContext.request.contextPath}/file/upload2.do" method="post" enctype="multipart/form-data">

    <!--表单的name必须一致-->
	<input type="file" name="img">
	<br>
	<input type="file" name="img">
	<br>
	<input type="file" name="img">
	<br>
	<input type="submit" value="文件上传">
</form>

```

### 十八、文件下载

#### 1.步骤

```java
1.获取 文件File
2.设置响应头HttpHeaders
	HttpHeaders headers = new HttpHeaders();
3.设置头中文件响应方式
	headers.add("Content-Disposition","attchement;filenam=下载文件名");
4.设置头中响应状态码
	HttpStatus statusCode = HttpStatus.OK;
5.控制器返回ResponseEntity<byte[]>对象，让spring组件去处理。
	ResponseEntity<byte[]> entity = new ResponseEntity<byte[]>(byte[],headers,statusCode);
```

#### 2.代码

```java
   @ResponseBody
    @RequestMapping("download")
    public void download(String id, HttpServletResponse response){
        //从数据库表中读取相应文件位置
        String location = clueTransferFeedbackMapper.findbyFrist("id", id, ClueTransferFeedbackFormMap.class).getStr("filePath");
        File file = new File(location);
        //获取文件名
        String[] split = location.split("/");
        String filename = split[split.length - 1];
        if(file.exists()){
            response.setContentType("application/force-download");
            //文件名乱码要手动编码
            response.addHeader("Content-Disposition", "attachement;filename=" +
                    new String((filename).getBytes("utf-8"), "ISO-8859-1"));
            //创建输入流，读取相应文件
            byte[] b = new byte[1024];
            FileInputStream is = null;
            BufferedInputStream bis = null;
            try {
                is = new FileInputStream(file);
                bis = new BufferedInputStream(is);
                ServletOutputStream os = response.getOutputStream();
                int i = bis.read(b);
                while(i != -1){
                    os.write(b, 0, i);
                    i = bis.read(b);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }finally {
                bis.close();
                is.close();
            }
        }
    }
```

```jsp
<!--jsp-->
<body>
	<a href="${pageContext.request.contextPath}/file2/download.do">下载</a>
</body>
```

