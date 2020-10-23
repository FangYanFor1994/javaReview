### 1.介绍

```java
1.servlet是服务器上的一个小java程序，实现在服务器上部署一段java代码；
2.servlet能动态生成web资源；
```

```java
访问servlet资源和配置：
servlet：
public class ServletDemo extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().write("我是方岩！");
	}
}

web.xml配置
<servlet>
    <servlet-name>ServletDemo</servlet-name>
    <servlet-class>com.fy.ServletDemo</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ServletDemo</servlet-name>
    <url-pattern>/servletDemo</url-pattern>
  </servlet-mapping>
  
访问：http://localhost:8080/项目名/servletDemo
```

### 2.servlet两种创建方式

```javascript
1.实现Servlet接口
2.继承HttpServlet类
3.利用eclipse组件创建，右键包，new servlet，将会自动创建servlet并且自动配置web.xml;

servlet接口API
init() :servlet对象创建时执行的方法；
service()  ：访问servlet执行的方法；
destroy() ：servlet对象销毁时执行的方法；
说明：对于init()，一般情况只会在第一次访问servlet时执行一次，再访问servlet时，会用之前创建好的servlet
	service()，每次访问都会执行；（核心业务方法）
	destroy(),在服务器被关闭或者web应用被卸载时执行；
```

### 3.servlet使用

```java
1.获取请求参数
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String name = request.getParameter("name");
		String pwd = request.getParameter("pwd");
		System.out.println(request.getRemoteAddr()+"发来"+name+":"+pwd);
	}
2.处理中文参数
	1）为什么表单中会产生中文乱码？
		客户端以UTF-8的编码方式传输数据到服务器端，而服务器端的request对象使用的是ISO8859-1的编码来接收数据，服务器和客户端的编码不一致导致的中文乱码；
	2）解决get中文乱码
		String name = request.getParameter("name");
		name = new String(name.getBytes("ISO8859-1"),"UTF-8");
		//String类的构造器，先用iso8859-1解码成字节数组，再用UTF-8编码返回到客户端
		注意：tomcat7.0及以下的版本get请求会乱码，tomcat8.0以后对请求URL做统一编码处理，不会乱码
	3）解决post中文乱码
		request.setCharacterEncoding("UTF-8");
		可以调用request从ServletRequest接口继承而来的“setCharacterEncoding(chaset)”对request内的参数进行统一编码；
	4)servlet向客户端输出中文
		response.setContentType("text/html;charset=UTF-8");

实例：
		request.setCharacterEncoding("utf-8");//请求参数编码处理
		String name = request.getParameter("name");
		String pwd = request.getParameter("pwd");
		response.setContentType("text/html;charset=utf-8");//输出数据编码处理
		response.getWriter().write(name+";"+pwd);
```

### 4.servlet跳转和路径

```java
1.重定向
	response.sendRedirect("register.html")
//重定向可以指定任何资源，包括当前应用程序中的其他资源，同一个站点上的其他应用程序中的资源，其他站点的资源；
    注意：传递给HttpServletResponse.sendRedirect 方法的相对URL以“/”开头，它是相对于整个WEB站点的根目录
2.请求转发
	request.getRequestDispatcher("LoginServlet").forward(request,response);
	//转发只能将请求转发给同一个WEB应用中的组件
注意：如果创建RequestDispatcher 对象时指定的相对URL以“/”开头，它是相对于当前WEB应用程序的根目录。
```

