### 1.概述

```java
1.xml指可扩展标记语言；类似于HTML；设计宗旨是传输数据而非显示数据；标签没有预定义；是w3c的推荐标准；
2.与HTML区别：HTML设计用来显示数据，其焦点是数据的外观；xml旨在传输数据；
3.xml的意义：1）可以用来存储和传输java的对象数据；
			2）在Javaee中更多的是将配置信息记录在xml中；
```

### 2.用dom4j解析xml文件

```java
//解析代码说明：1）获取文件输入流此处用Thread.currentThread().getContextClassLoader().getResource("name.xml");   不能用new FileInputStream("name.xml")
//2)SAXReader为dom4j的核心类，用于读取文件的输入流，得到文档对象

public class XmlDemo {
	public static void main(String[] args) throws FileNotFoundException, DocumentException {
		//读取xml文件
		SAXReader reader = new SAXReader();
		Document document = reader.read(Thread.currentThread().getContextClassLoader().getResource("Dogs.xml"));
		//获取根节点
		Element rootElement = document.getRootElement();
		//获取跟标签的所有子标签
		List<Element> elements = rootElement.elements();
		List<Dog> list = new ArrayList<Dog>();
		//遍历节点内容放进对象中
		for (Element element : elements) {
			Dog dog = new Dog();
			dog.setId(Integer.parseInt(element.attribute("id").getText()));
			dog.setName(element.elementText("name"));
			dog.setSex(element.elementText("sex"));
			list.add(dog);
		}
		System.out.println(list);
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<dogs>
	<dog id="1">
		<name>旺财</name>
		<sex>公</sex>
	</dog>
	<dog id="2">
		<name>如花</name>
		<sex>母</sex>
	</dog>
</dogs>
```

```java
public class Dog {

	private int id;
	private String name;
	private String sex;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSex() {
		return sex;
	}
	public void setSex(String sex) {
		this.sex = sex;
	}
	@Override
	public String toString() {
		return "Dog [id=" + id + ", name=" + name + ", sex=" + sex + "]";
	}
}
```

###3.对象生成xml文件

```java
File file = new File("create.xml");
//创建文档对象
Document document = DocumentHelper.createDocument();
Element root = document.addElement("dogs");
for(Dog dog = dogs){
    Element dg = root.addElement("dog");
    dg.addElement("id").setText(dog.getId()+"");
    dg.addElement("name").setText(dog.getName);
    dg.addElement("type").setText(dog.getType);
}
XMLWriter writer = new XMLWriter(new FileOutputStream(file));
writer.write(document);
return file.getAbsolutePath();
```

