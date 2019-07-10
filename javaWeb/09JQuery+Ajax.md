### 1.jQuery

#### 1.1jquey概述

```js
概述：jQuery是一个js库，里面封装了很多js的方法，相当于是一个外部js文件；
1.快速入门：
	1）导库
    2）在script中直接使用即可：
    	<script type="text/javascript" src="../js/jquery-1.8.3.js" ></script>
        <script>
        $(
        function(){
        $("#span1").html("hello jquery!");
        }
        );
        </script>
```

#### 1.2jQuery的语法格式

```js
1.jQuery的页面加载函数：
    $(function(){

    });
	//等价于js的window.onload=function(){}
2.jquery对象和dom对象之间的转换
	1)dom转jQuery：
    	var sp1 = docunment.getElementById("sp1");
		$(sp1).html("hello jquery");
	2)jquery转dom
    	$("#sp1").get(0).innerHTML="hello!";
3.选择器
	1)id选择器：$("#btn1");
	2）元素选择器：$("div");
	3）类选择器：$(".mini");
	4）所有选择器：$("*")
	5）合并多个选择器：$("div,.mini");
	6）后代选择器：$("爷爷 后代")： 后代包括儿子和孙子
    			$("爷爷 > 儿子")：只有儿子，没有孙子
                $("哥哥 + 弟弟")：弟弟必须是紧挨着哥哥的，其他的远房弟弟不算
                $("哥哥 ~ 弟弟")：弟弟可以不是紧挨着的，只有是同辈的。
      7）基本过滤选择器：
      		first(): 选择第一个
            	$("body div").first().css("background-color","yellow");
            last():选择最后一个
            	$("body div").last().css("background-color","red");
            even: 选择索引是偶数的元素
            	$("body div:even").css("background-color","blue");
            odd：选择索引是奇数的元素
            	$("body div:odd").css("background-color","green");
	8）属性选择器
    	$("input[type='password']")
4.jqery遍历：
	 $.each(数据集,回调函数(i,n))；//i:角标  n:元素
	示例：
    var a = ["方岩","赵雅静","周星驰","梁朝伟"];
    $.each(a, function(i,n) {
        alert(i+";"+n);
    });
```

### 2.json在java中的解析

```java
1.fastjson解析
导入fastjsonjar包；
	1）json字符串转java对象：JSON.parseObject(json1, User.class);
	2)java对象转json字符串：JSON.toJSONString(user3);
	3）把数组类型的JSON字符串转化为java数组或集合：JSON.parseArray( )
2.示例：
	String json = "{'name':'方岩','age':25,'addr':'杭州','dog':[{'name':'旺财','type':'田园犬','age':1}]}";
		User user = JSON.parseObject(json, User.class);
		String jsonString = JSON.toJSONString(user);
		System.out.println(user);
		System.out.println(jsonString);
```

### 3.JQuery中ajax技术

####3.1ajax()方法

```js
//发送ajax的通用方式
			$.ajax({
				url:"login.do",
				data:param,//传到服务器的参数
				type:"post",//请求的方式post|get
				dataType:"text",//返回值类型html|xml|text|json|script|jsonp
				cache:false,//浏览器是否缓存
				async:true,//是否异步提交
				success:function(rec){//交互成功
					alert(rec);
				},
				error:function(){//交互失败
					alert("error");
				}
			});	
```

#### 3.2load()方法

```java
语法：
$(selector).load(URL,data,callback);
可以在特定的标签元素中load数据(一般是从服务器得到的数据)

URL：必须值，参数规定您希望加载的 URL。
data：可选值， 参数规定与请求一同发送的查询字符串键/值对集合。
callback（responseTxt，statusTXT，xhr）：可选值， 参数是 load() 方法完成后所执行的函数名称。
其中callback参数的意义：
1.responseTxt：包含调用成功时的结果内容
2.statusTXT：包含调用的状态
3.xhr：XMLHttpRequest对像
```

#### 3.3post()方法

```java
$.post(URL,data,callback);
URL：必须值， 参数规定您希望请求的 URL。
callback（data,status）：可选值， 参数是请求成功后所执行的函数名。
	其中data:表示返回值,文本类型
	status:表示状态值success|error
data:可选值，表示发送到服务器中的参数。
```

#### 3.4get()方法

```
$.get(URL?data,callback);
URL：必须值， 参数规定您希望请求的 URL。
callback（data,status）：可选值， 参数是请求成功后所执行的函数名。
	其中data:表示返回值,文本类型
	status:表示状态值success|error
发送到服务器的参数，一般作为查询字符串拼接在URL后面。
```

