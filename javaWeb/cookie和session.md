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
```

