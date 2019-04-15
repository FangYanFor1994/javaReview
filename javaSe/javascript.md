### 一、JavaScript基本语法

#### 1.数据类型

```javascript
1.基本数据类型
Number、String、Boolean、Undefined、Null五种；
2.引用数据类型
JavaScript中对象用json对象表示：
var student = {id:1,name:"张三",age:18};
document.write(student.id);//注：js中document代表HTML文档，document.write()可直接向HTML页面输出内容
3.数据类型
var a = [1,2,3,4];
4.关系运算符
与java一致！
```

#### 2.函数、事件、正则表达式

```JavaScript
1.函数
    函数定义：
    function functionName(parameters){
        //执行的代码
    }
2.弹窗函数
    2.1只能点击确定的弹窗
        alert("你好！");//无返回值
    2.2可以点击确定或取消的弹窗
        confirm("你好！")//点击确定返回true,点击“取消”或“x”返回false;
    2.3可以输入文本内容的弹窗
        prompt("你爱学习吗？","爱");
        //第一个参数是提示信息 ，第二个参数是用户输入的默认值；
        //点击确定时返回用户输入的内容，点击取消或关闭时，返回null；
3.事件
	onchange、onclick、onmouseover(用户在一个HTML元素上移动鼠标)、onmouseout(用户从一个HTML元素上移开鼠标)、onkeydown(用户按下键盘)、onload(浏览器已完成页面的加载)
4.正则表达式
	语法：var patt1 = new RegExp("e");
	document.write(patt1.test("The best thing in life are free"));
	//返回true 
	//另patt1.exec("The best thing in life are free"),返回被检索到的值，即e，如果没有，则返回null;
```

#### 3.javaScript的DOM

```JavaScript
1.概述：
	DOM即文档对象模型(Document Object Model)。
    1.1通过可编辑的对象模型，JavaScript有足够的能力创建动态的HTML。
    1）JavaScript能改变页面的所有HTML元素；
    2）JavaScript能改变页面所有HTM属性；
    3）JavaScript能改变页面所有CSS样式；
    4）JavaScript能对页面所有事件做出反应；
2.查找HTML元素
	2.1通过id查找
    	var a = document.getElementById("intro");
	2.2通过标签名查找
    	var b = document.getElementsByTagName("p");
	2.3类名查找
    	var c = document.getElementByClassName("intro");
3.改变HTML
	3.1改变HTML元素
		改变HTML输出流
    		document.write();//直接向HTML输出流写出内容
        改变HTML内容
    		document.getElementById(id).innerHTML="abc";
	3.2改变HTML属性
    	document.getElementById(id).attribute=新属性值;
		//例：document.getElementById("image").src="2.jpg";
	3.3改变CSS样式
    	document.getElementById(id).style.property=新样式
		//document.getElementById("p2").style.color="blue";
	3.4对事件做出反应
    document.getElementById(id).onclick=function(){
        //执行代码
    }
```

