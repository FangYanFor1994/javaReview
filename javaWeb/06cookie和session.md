### 1.概述

```java
1.cookie：客户端状态管理技术：将状态保存在客户端。
2.session：服务器状态管理技术：将状态保存在服务器端。（服务器传递sessionID时需要使用cookie的方式）；
```

### 2.cookie技术

```java
1.概述：cookie是客户端在会话过程中将会话数据保存到客户端本地的一种技术，不同的客户端cookie是不会共享的。
2.cookie的创建和保存到客户端
	Cookie ck = new Cookie("name","zhaoyajing");
	response.addCookie(ck);
	//注：键值对不能保存中文，由于编码格式不同，会保存，需要解码后保存；
3.客户端怎样携带cookie到此次会话的下个资源，（自动携带）；
	//注：默认情况下cookie在会话结束后被清除。cookie存活在浏览器内存中；
4.服务器获取客户端携带的cookie
	Cookie[] cookies = request.getCookies();
	for(Cookie ck:cookies) {
	if(ck.getName().equals("name")) {
		System.out.println(ck.getName()+"="+ck.getValue());
	}
```

#### 2.1cookie的编解码

```java
1.编码：
//使用URLEncoder类的encode(String str,String encoding)方法；
Cookie ck = new Cookie(URLEncoder.encode("姓名","UTF-8"),URLEncoder.encode("赵雅静","UTF-8"));
		response.addCookie(ck);
2.解码：
//使用URLDecoder类的decode(String str,String encoding)方法；
Cookie[] cookies = request.getCookies();
		for(Cookie ck:cookies) {
			String name = URLDecoder.decode(ck.getName(),"UTF-8");
			String value = URLDecoder.decode(ck.getValue(),"UTF-8");
			System.out.println(name+"="+value);
		}
```

#### 2.2cookie的存活周期

```java
1.默认情况下cookie存放浏览器内存中，当浏览器关闭时会话结束，cookie销毁，所以cookie默认存活周期是一次浏览器会话中；通过setMaxAge()可以设置cookie存活周期，让cookie以文件形式保存在客户端本地；
ck.setMaxAge(-1)；//设置生成时间
取值说明：
>0有效期，单位秒
=0失效
<0内存存储
```

#### 2.3cookie的路径问题

```java
cookie.setPath("/");//默认；客户端访问当前web应用的其他资源时，都会携带该客户端的该web应用存放的cookie，其他web应用的cookie不会携带。
通过cookie的路径来决定客户端在访问怎样的资源的情况下才会携带此cookie；
```

#### 2.4如何修改cookie的值

```java
1.保证cookie的名和路径一致即可修改；
//创建Cookie
Cookie ck=new Cookie("code", code);
ck.setPath("/");//设置Cookie的路径
ck.setMaxAge(-1);//内存存储，取值有三种：>0有效期，单位秒；=0失效；<0内存存储
response.addCookie(ck);//让浏览器添加Cookie
```

#### 2.5cookie的优缺点

```java
优点：
	1.数据持久性：可以设置cookie的存活时间，保证数据持久性，cookie通常是客户端上持续时间最长的数据保留形式；
	2.cookie不需要任何服务器资源存储在客户端并在发送后由服务器读取；
	3.简单性：cookie是 一种基于文本的轻量结构，包含简单的键值对；
	
缺点：
	1.浏览器一般只允许存放300个cookie，每个web站点最多可以存放20个cookie，每个cookie的大小限制为4kb;
	2.浏览器如果禁用cookie，则cookie将不可使用；
```

### 3.session技术（域对象）

#### 3.1session工作原理

```java
服务器端调用request.getSession();服务器开辟一个session空间，并且服务器会检索客户端是否存在带JSESSIONID的cookie，如果存在，服务器会检查找到与该session相匹配的信息，如果存在则直接使用该session，如果不存在则重新生成新的session；
服务器创建新的session同时会自动创建一个cookie存放JSESSIONID，发送给客户端，保存在客户端内存中，以便下次该客户端访问servlet时，携带；
```

#### 3.2session常用API

```java
//获取session对象
HttpSession session = request.getSession();
//获取session的唯一标记(该id就是cookie中JSESSIONID的值)
String id = session.getId();
//获得最大空闲时间(默认为30分钟)
session.getMaxInactiveInterval();
//获取session的创建时间
session.getCreationTime();
//session绑定对象
session.setAttribute(String key,Object value);
//销毁session
session.invalidate();
```

#### 3.3session的生命周期

```java
1.出生：第一次调用request.getSession();
2.销毁：
	2.1服务器非正常关闭；
	2.2默认情况下，session在服务器上保存的时间是30分钟，从回话结束时开始计时；
		可以通过设置web.xml的session-config来修改session的存活时间；
	2.3主动销毁
		调用session的invalidate()方法；
```

#### 3.4session实现自动登录

```java
//首次登陆，将登陆用户名密码等信息放入session中
HttpSession session = request.getSession();
Cookie cookie = new Cookie("JSESSIONID",session.getId());
cookie.setMaxAge(60*60*24*7);
response.addCookie(cookie);
//放一个uname
session.setAttribute("uname", vo.getUname());
response.sendRedirect(request.getContextPath()+"/products.jsp");
//实现原理：第一次访问servlet时创建session，并手动创建cookie放入JSESSIONID，为该cookie设置存活时间，发送给客户端取代服务器自动创建的带JSESSIONID的cookie（设置时间的cookie存在客户端本地文件中，即便是客户端关闭，再次打开仍然携带该cookie），这样客户端在cookie失效时间之前下次访问servlet或jsp文件，携带该JSESSIONID，这样sevlet或jsp就会找到JSESSIONID对应的session并取出里面的数据信息；   另session设置的超时时间应该和手动cookie一致；
```

### 4.session的URL重写

```java
1.概述：之前说到session由服务器创建，返回到客户端，客户端通过cookie保存session的唯一标识JSESSIONID，当客户端下次再访问服务器程序时，服务器会自动识别浏览器的携带的cookie，如果有JSESSIONID，则找到JSESSIONID对应的session，以达到服务器session共享；
2.问题：每个session的大小为4kb，当浏览器中的cookie达到3kb时已经力不从心；或者cookie被禁用；那么浏览器无法生成cookie，如何实现session技术；
3.解决方案：
	1）URL重写；
		response.encodeRedirectURL(java.lang.String url) 用于对sendRedirect方法后的url地址进行重写。
　　		response.encodeURL(java.lang.String url)用于对表单action和超链接的url地址进行重写
　　		原理：response接口中提供的这两个方法可以实现在原来的URL后添加JSESSIONID作为参数；
	表单隐藏字段；
		把服务器返回的JSESSIONID放置到一个隐藏表单中，下次表单提交时会提交该JSESSIONID，以此实现session会话跟踪。
```

