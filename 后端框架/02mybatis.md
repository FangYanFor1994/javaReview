### 1.什么是框架

```
框架其本质是半成品程序，提供相关规范，并且提供大量可重用的组件。
目的：让开发者开发出结构比较良好，可读性较强，扩展性和可维护性较好的工程。
```

### 2.mybatis开发环境搭建

####2.1开发步骤

```
1.创建maven工程，在pom.xml文件中引入mybatis、MySQL、log4j的依赖；
2.配置mybatis-config.xml文件，文件里主要是数据库的连接信息，以及mapper.xml的映射信息；
3.编写dao层代码，并配置mapper.xml文件；
4.使用SqlSessionFactoryBuilder解析mybatis-config.xml文件，获取SQLSession对象；使用sqlSession的API进行CURD操作；
```

#### 2.2示例

**步骤一**：新建maven工程，在pom.xml文件中引入jar包依赖

```xml
<dependencies>
    	<!--mybatis-->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.6</version>
		</dependency>
		<!--mysql-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.40</version>
		</dependency>
		<!--log4j-->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.17</version>
		</dependency>

	</dependencies>
```

**步骤二：在resource目录下，mybatis-config.xml文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 读取数据库连接信息properties标签读取db.properties文件 -->
	<properties resource="db.properties"></properties>
	<!-- 数据连接环境 -->
	<environments default="development">
		<environment id="development">
			<!-- 使用JDBC事务进行事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据源配置：POOLED表示使用连接池来管理连接对像 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<!-- 加载mapper文件 -->
	<mappers>
		<mapper resource="com/fy/dao/UserMapper.xml"/>
	</mappers>
</configuration>
```

**db.properties数据库信息**

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///javaee
jdbc.username=root
jdbc.password=123
```

**log4j.properties日志配置文件**

```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# MyBatis logging configuration...
#log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

#说明：DEBUG代表日志级别，日志级别从高到低：ERROR、WARN、INFO、DEBUG
#指定了输出级别为DEBUG，则所有高于DEBUG级别的日志都会被打印出来
```

**步骤三：在dao层创建Mapper.xml文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace表示命名空间，用来标识该Mapper文件-->
<mapper namespace="com.fy.dao.UserMapper">
	
    <!--id为该Sql语句的唯一标识，resultType表示查询出的结果要封装成的java类型-->
	<select id="selectAll" resultType="com.fy.model.User">
		select * from USER
	</select>
</mapper>
```

**步骤四：使用sqlSession的API进行CRUD操作**

```java
public static void main(String[] args) throws IOException {
		//1.找到mybatis的配置文件
		String resource = "mybatis-config.xml";
		//2.获取输入流
		InputStream stream = Resources.getResourceAsStream(resource);
		//3.创建sqlSessionFactory
		SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(stream);
		//4.创建sqlSession
		SqlSession sqlSession = sessionFactory.openSession();
		//5.利用sqlSession进行数据库操作
		List<User> list = sqlSession.selectList("com.fy.dao.UserMapper.selectAll");
		System.out.println(list.toString());
		//6.关闭资源
		sqlSession.close();
	}
```

### 3.Mybatis相关类的生命周期

### 3.1SqlSessionFactoryBuilder

```
这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。因此
SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。你可以重用
SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但是最好还是不要让其一直存在以保证所有的
XML 解析资源开放给更重要的事情
```

#### 3.2SqlSessionFactory

```
SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由对它进行清除或重建。使用
SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代
码“坏味道（bad smell）”。因此 SqlSessionFactory 的最佳作用域是应用作用域。有很多方法可以做到，最简单的
就是使用单例模式或者静态单例模式。
```

#### 3.3SqlSession

```
每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的
最佳的作用域是请求或方法作用域。绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量
也不行。也绝不能将 SqlSession 实例的引用放在任何类型的管理作用域中，比如 Servlet 架构中的
HttpSession。如果你现在正在使用一种 Web 框架，要考虑 SqlSession 放在一个和 HTTP 请求对象相似的作用域
中。换句话说，每次收到的 HTTP 请求，就可以打开一个 SqlSession，返回一个响应，就关闭它。这个关闭操作是很
重要的，你应该把这个关闭操作放到 finally 块中以确保每次都能执行关闭
```

### 4.Mybatis工具类

```java
public class MybatisUtil {

	static SqlSessionFactory sqlSessionFactory;

	static {
		
		String resource = "mybatis-config.xml";
		InputStream inputStream;
		try {
			inputStream = Resources.getResourceAsStream(resource);
			sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}
	
	/**
	 * 该方法调用必须在应用程序的成员方法中调用
	 * @return
	 */
	public static SqlSession findSqlSession(){
		
		return sqlSessionFactory.openSession();
	}
}
```

### 5.Mybatis传统方式进行CRUD操作

说明：主要是创建sqlSession对象与数据库进行交互（sqlSession底层封装了操作数据库的statement对象），类似于DBUtil中的QueryRunner对象

**mapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="dao.UserMapper">
	

	<select id="selectAll" resultType="model.User">
		select * from USER
	</select>
	
	<!-- OGNL表达式 
		1.简单类型参数,#{}中参数的命名可以自定义
		2.POJO参数，#{}中参数的命名必须是POJO对像中get方法的后缀（首字母小写）一致。
	-->
	<select id="selectById" resultType="model.User">
		select * from USER where id = #{id}
	</select>
	
	<select id="selectByItem" resultType="model.User" parameterType="model.User">
		select * from USER where username=#{username} and password=#{password}
	</select>
	
	<insert id="insert" parameterType="model.User">
		insert into USER(username,password) values(#{username},#{password})
	</insert>
	
	<delete id="delete" parameterType="model.User">
		delete from USER where id = #{id}
	</delete>
	
	<update id="update" parameterType="model.User">
		update USER set username=#{username},password=#{password} where id = #{id}
	</update>
</mapper>
```

**dao层代码**(核心)

```java
public class UserDao {

	public void insert(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		try {
			session.insert("dao.UserMapper.insert", user);

			// 提交事务
			session.commit();
		} catch (Exception e) {
			e.printStackTrace();
			session.rollback();
		} finally {
			session.close();
		}

	}

	public void delete(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		try {
			session.delete("dao.UserMapper.delete", user);

			// 提交事务
			session.commit();
		} catch (Exception e) {
			e.printStackTrace();
			session.rollback();
		} finally {
			session.close();
		}
	}

	public void update(User user) {
		SqlSession session = MybatisUtil.findSqlSession();
		try {
			session.update("dao.UserMapper.update", user);

			// 提交事务
			session.commit();
		} catch (Exception e) {
			e.printStackTrace();
			session.rollback();
		} finally {
			session.close();
		}
	}

	public User selectById(int id) {

		SqlSession session = MybatisUtil.findSqlSession();

		User user = null;

		try {
			user = session.selectOne("dao.UserMapper.selectById", id);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return user;
	}

	public List<User> selectAll() {
		
		SqlSession session = MybatisUtil.findSqlSession();

		List<User> users = null;

		try {
			users = session.selectList("dao.UserMapper.selectAll");
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return users;
	}

	public User selectByItem(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		try {
			user = session.selectOne("dao.UserMapper.selectByItem", user);
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			session.close();
		}
		return user;
	}

}

```

**service层代码**

```java
public class UserService {

	UserDao dao = new UserDao();

	public void save(User user) {

		dao.insert(user);

	}

	public void remove(User user) {

		dao.delete(user);
	}

	public void modify(User user) {
		dao.update(user);
	}

	public User findById(int id) {
		return dao.selectById(id);
	}

	public List<User> findAll() {
		return dao.selectAll();
	}

	public User findByItem(User user) {

		return dao.selectByItem(user);
	}
}
```

**单元测试类**

```java
public class CRUDTest {
	
	UserService ser;

	@Before
	public void init(){
		
		ser = new UserService();
	}
	
	@Test
	public void testSave(){
		
		User user = new User();
		user.setUsername("张三");
		user.setPassword("1234");
		ser.save(user);
	}
	
	@Test
	public void testModify(){
		
		User user = new User();
		user.setId(3);
		//user.setUsername("张三");
		user.setPassword("0000");
		ser.modify(user);
	}
	
	@Test
	public void testRemove(){
		
		User user = new User();
		user.setId(3);
		
		ser.remove(user);
	}
	
	@Test
	public void testFindById(){
		
		User user = ser.findById(1);
		
		System.out.println(user.getId()+","+user.getUsername());
	}
	
	@Test
	public void testFindByItem(){
		
		User user = new User();
		user.setUsername("admin");
		user.setPassword("admin");
		User findUser = ser.findByItem(user);
		System.out.println(findUser.getId()+","+findUser.getUsername()+","+findUser.getPassword());
	}
	
	
}
```

### 6.面向接口方式编程

*原理**

```
MyBatis将核心配置文件中的每一个节点抽象为一个 Mapper 接口，而这个接口中声明的方法和跟节点中的
<select|update|delete|insert>节点项对应，即<select|update|delete|insert> 节点的id值为Mapper接口中的
方法名称，parameterType值表示Mapper对应方法的入参类型，而resultMap 值则对应了Mapper接口表示的返
回值类型或者返回结果集的元素类型。
根据MyBatis的配置规范配置好后，通过SqlSession.getMapper(XXXMapper.class) 方法，MyBatis会根据相应的
接口声明的方法信息，通过动态代理机制生成一个Mapper实例，我们使用Mapper接口的某一个方法时，MyBatis会根据
这个方法的方法名和参数类型，确定Statement Id，底层还是通过
SqlSession.select("statementId",parameterObject);
或者
SqlSession.update("statementId",parameterObject);
等等来实现对数据库的操作，
MyBatis引用Mapper接口这种调用方式，纯粹是为了满足面向接口编程的需要。（其实还有一个原因是在于，面
向接口的编程，使得用户在接口上可以使用注解来配置SQL语句，这样就可以脱离XML配置文件，实现“0配置”）
```

**开发规范**

```
面向接口编程，必须遵守一定开发规范，如下：
1.接口和mapper必须同包放置
2.接口的类名必须和mapper的名称一致
3.接口方法名称必须和mapper中Sql的Id一致
4.mapper文件的namespace必须和接口的完全限定名一致
5.mapper接口的方法参数只能有一个，且类型要和mapper映射文件中statement的parameterType的值保持一致。
6.mapper接口的返回值类型要和mapper映射文件中statement的resultType值或resultMap中的type值保持一致。
7.接口中方法不允许重载。
8.如果传入多个参数，请把参数封装到POJO或Map集合中(也可以使用注解方式).
```

**mapper接口**

```java
public interface UserMapper {

	public void insert(User user);
	
	public void delete(User user);
	
	public void update(User user);
	
	public User selectById(User user);
	
	public List<User> selectAll();
	
    //注解方式支持多参传入
	public List<User> selectByPage(@Param(value="firstIndex")int firstIndex,@Param(value="maxResult")int maxResult);
	
    //????
	public List<User> selectByPage2(int firstIndex,int maxResult);
    
    //多参传值时，一般封装成map或POJO进行处理
    public List<User> selectByPage3(Map<String,Integer> map);
    
    public List<User> selectByLike(User user);
	
}
```

**mapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="crud2.mapper.UserMapper">
	

	<select id="selectAll" resultType="model.User">
		select * from USER
	</select>
	
	<!-- OGNL表达式 
		1.简单类型参数,#{}中参数的命名可以自定义
		2.POJO参数，#{}中参数的命名必须是POJO对像中get方法的后缀（首字母小写）一致。
	-->
	<select id="selectById" resultType="model.User" parameterType="model.User">
		select * from USER where id = #{id}
	</select>
	<!--多参传值时，#｛｝中参数名和接口中@Param()中的值一致-->
	<select id="selectByPage" resultType="model.User">
		select * from USER limit #{firstIndex},#{maxResult}
	</select> 
	
    <!--????-->
	<select id="selectByPage2" resultType="model.User">
		select * from USER limit #{0},#{1}
	</select> 
    
    <!--接收参数为map类型-->
    <select id="selectByPage3" resultType="model.User"  parameterType="java.util.Map">
		select * from USER limit #{firstIndex},#{maxResult}
	</select> 
   
    <!--不要使用EL${}取值，使用函数处理-->
    <select id="selectByLike" resultType="model.User"  parameterType="model.User">
		select * from USER where username like concat('%',#{username},'%');
	</select> 
	
	
	<insert id="insert" parameterType="model.User">
		insert into USER(username,password) values(#{username},#{password})
	</insert>
	
	<delete id="delete" parameterType="model.User">
		delete from USER where id = #{id}
	</delete>
	
	<update id="update" parameterType="model.User">
		update USER set username=#{username},password=#{password} where id = #{id}
	</update>
	
	
</mapper>
```

**service代码**

```java
public class UserService implements IUserService {

	@Override
	public void save(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
		try {
			mapper.insert(user);
			session.commit();
		} catch (Exception e) {
			session.rollback();
		} finally {
			session.close();
		}

	}

	@Override
	public void remove(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
		try {
			mapper.delete(user);
			session.commit();
		} catch (Exception e) {
			session.rollback();
		} finally {
			session.close();
		}

	}

	@Override
	public void modify(User user) {

		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
		try {
			mapper.update(user);
			session.commit();
		} catch (Exception e) {
			session.rollback();
		} finally {
			session.close();
		}

	}

	@Override
	public User findById(User user) {
		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);

		return mapper.selectById(user);
	}

	@Override
	public List<User> findAll() {
        
		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
        
        List<User> list = mapper.selectAll();
        
       	session.close();

		return list;
	}

	@Override
	public List<User> findByPage(int firstIndex,int maxResult) {

		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
        
       	List<User>  list = mapper.selectByPage(firstIndex,maxResult);
        
        session.close();
        
		return list;
	}
	
	@Override
	public List<User> findByPage2(int firstIndex,int maxResult) {

		SqlSession session = MybatisUtil.findSqlSession();
		// 代理对像
		UserMapper mapper = session.getMapper(UserMapper.class);
        
        session.close();
        
		return mapper.selectByPage2(firstIndex,maxResult);
	}

}

```

**测试（略）**

### 7.对mybatis的理解

```java
1.传统方式编程使用sqlSession的API对数据库进行操作，比如sqlSession.update("dao.UserMapper.update", user);
第一个参数是String类型的statementId,主要用于定位mapper.xml文件的sql语句的id，第二个参数是执行sql语句传入的参数。（类似于DBUtil中的queryRunner.update(sql,args...)）
2.面向对象编程方式，将mapper.xml中的sql语句的id与接口方法的方法名绑定（保持一致），我们再调用时就直接调用接口方法，通过接口方法也可定位statementId,从而找到需要执行的SQL语句；
```

### 8.Mybatis配置文件解析

#### 1.typeAliases标签用法

```xml
<!--给封装数据的模型类起别名，目的是为了减少mapper文件的配置-->

<!-- 别名配置 -->
	<typeAliases>
        <!--给单独的类起别名-->
		<typeAlias type="model.User" alias="User"/> 
		<!-- 默认把类名作为该类的别名（大小写没有影响） -->
		<package name="model"/>
	</typeAliases>
```

#### 2.properties标签的用法

```xml
<!--用来加载properties属性文件的-->

<properties resource="" url=""></properties>
	<!--resource：用来加载classess文件夹下的properties属性文件。
	url：用来加载指定位置的properties属性文件。-->

<!--也可以不加载外部的properties文件，也可以把连接信息直接写在properties标签中-->
<properties>
	<property name="jdbc.driver" value="com.mysql.jdbc.Driver" />
    <property name="jdbc.url" value="jdbc:mysql:///javaee"/>
    <property name="jdbc.username" value="root"/>
    <property name="jdbc.password" value="123"/>
</properties>
```

```
读取值的优先级：
1.在 properties元素体内指定的属性首先被读取。
2.然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，
并覆盖已读取的同名属性。
3.最后读取作为方法参数传递的属性，并覆盖已读取的同名属性（一般属性文件中的key要有前缀，如jbdc.url）。
即：
通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的是 properties
属性中指定的属性。
```

#### 3.Settings标签用法

```xml
<!--用来设置mybatis的全局功能配置-->
设置mybatis的日志实现厂商为Log4j
<!-- 全局功能配置 -->
	<settings>
		<!-- 指定日志厂商为LOG4J -->
		<setting name="logImpl" value="LOG4J"/>
	</settings>
```

#### 4.transactionManager标签用法

```
作用：设置mybatis的事务管理策略
type的默认值：
1.JDBC(默认使用)
	JDBC:这个配置是直接使用了JDBC的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域；
2.MANAGED:这个配置几乎没作什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期；

注意：如果你正在使用spring+mybatis，则没有必要配置事务管理器，因为spring模块会用自带的管理器来覆盖前面的配置。
```

#### 5.DataSource标签的用法

```
作用：设置mybatis中connection对象的获取策略。
有三种内建的数据源类型
1.UNPOOLED
这个数据源的实现只是每次被请求时打开和关闭连接。虽然有点慢，但对于在数据库连接可用性方面没有太高要求
的简单应用程序来说，是一个很好的选择。 不同的数据库在性能方面的表现也是不一样的，对于某些数据库来说，
使用连接池并不重要，这个配置就很适合这种情形
2.POOLED
这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证
时间。 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
还可以配置其他关于连接池中对像的管理策略.
3.JNDI
这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放
置一个 JNDI 上下文的引用。
```

#### 6.mapper标签的用法

```xml
作用：引入映射关系（引入mapper文件或接口类）

<!--使用相对于类路径的资源引用-->
<mappers>
	<mapper resource="mapper/UserMapper.xml"/>
</mappers>

<!--使用完全限定资源定位符（URL）-->
<mappers>
	<mapper url="d:/mappers/UserMapper.xml"/>
</mappers>

<!--使用映射器接口实现类的完全限定类名   注意：接口和mapper文件必须同包放置；接口和mapper文件命名一致-->
<mappers>
	<mapper class="mapper.UserMapper"/>
</mappers>

<!--将包内的映射器接口全部注册为映射器   注意：接口和mapper文件必须同包放置；接口和mapper文件命名一致-->
<mappers>
	<package name="mapper"/>
</mappers>
```

### 9.Mybatis映射文件的解析

#### 1.resultMap标签使用

```
两种用法：
1.当表中字段与对象set和get方法后缀不一致时，可以使用该标签，重新建立映射关系。
2.当进行多表关联查询时，可以使用该标签封装成java对象。
注意：如果表中字段和对象set个get方法后缀一致时，mybatis会自动进行映射封装数据。
```

##### 1.1对结果集重新定义映射关系

```xml
<!-- 自定义结果集映射 column：表中字段；properties：对象属性 -->
	<resultMap type="User" id="baseResultMap">
		<id column="t_id" property="id"/>
		<result column="t_username" property="username"/>
		<result column="t_password" property="password"/>
	</resultMap>
<!-- resultType与 resultMap只能使用其中之一-->
	<select id="selectById" parameterType="User" resultMap="baseResultMap">
		select * from USER where t_id = #{id}
	</select>
```

##### 1.2多表联查时，使用该标签进行复杂类型封装

**对象间关联关系表达**

```java
//关联多方
public class User{
    private int id;
    private String username;
    private String password;
    //关联多方（Orders）
    private List<Orders> list;
    //省略getter和setter方法
}

//关联一方
public class Orders{
    private int id;
 	private String oname;
    private int oprice;
    //关联一方
    private User user;
    //省略getter和setter方法
}
```

**Mapper文件中关联关系表达**

```xml
<!--UserMapper.xml配置-->
	<resultMap type="User" id="baseResultMap">
		<id column="id" property="id"/>
		<result column="username" property="username"/>
		<result column="password" property="password"/>
		<!-- 关联集合数据,使用collection标签，其中ofType用来说明集合中的数据类型 -->
		<collection property="list" ofType="Orders">
			<id column="oid" property="id"/>
			<result column="oname" property="oname"/>
			<result column="oprice" property="oprice"/>
		</collection>
	</resultMap>

	<!-- 连接查询：关联多方 -->
	<select id="selectByJoin" resultMap="baseResultMap2">
		SELECT u.*,
		o.id as oid,<!--给列起别名，目的是防止结果集中字段名重复，导致数据封装错误-->
		o.oname,
		o.oprice,
		o.uid
		FROM USER u 
        LEFT JOIN orders o
		ON u.id=o.uid
	</select>


   <!--OrdersMapper.xml配置-->
	<resultMap type="Orders" id="baseResultMap">
		<id property="id" column="oid"/>
		<result property="oname" column="oname"/>
		<result property="oprice" column="oprice"/>
		<!-- 关联一方:使用association标签，其中javaType是对Orders类中pojo包装类型的说明-->
		<association property="user" javaType="User">
			<id column="id" property="id"/>
			<result column="username" property="username"/>
			<result column="password" property="password"/>
		</association>
	</resultMap>

	<select id="selectById" resultMap="baseResultMap" >
		SELECT 
    	o.id AS oid,
    	o.oname,
   	 	o.oprice,
    	o.uid,
    	u.*
		FROM orders as o 
        LEFT JOIN USER as u
        ON u.id=o.uid 
        WHERE o.id = #{id}
	</select>


```

### 10.动态SQL

#### 10.1if标签

```xml
<select id="selectByItem" parameterType="User" resultType="User">
		select * from USER 
		where 1=1
        <!--mybatis如果是低版本时，如果是整形值，不要用‘’来判断;3.4.6版本没有问题-->
		<if test="id != null and id !=''">
			and id = #{id}
		</if>
		<if test="username !=null and username !=''">
			and username=#{username}
		</if>
		<if test="password !=null and password !=''">
			and password=#{password}
		</if>
		<if test="address !=null and address !=''">
			and address=#{address}
		</if>	 
	</select>
```

#### 10.2where标签

```
根据查询条件是否存在来决定是否生成where字符串，可以去除where后面紧跟的sql关键字，如or或and
```

```xml
<!-- where标签 -->
	<select id="selectByItem2" parameterType="User" resultType="User">
		select * from USER
		<where>
			<if test="id !='' and id != null">
				and id = #{id}
			</if>
			<if test="username !=null and username !=''">
				and username=#{username}
			</if>
			<if test="password !=null and password !=''">
				and password=#{password}
			</if>
			<if test="address !=null and address !=''">
				and address=#{address}
			</if>
		</where>	 
	</select>
```

#### 10.3choose when otherwise

```
和java中if...else if...else...作用类似，不管多少条件满足，只拼接其中一个条件；
```

```xml
<!-- choose when otherwise -->
	
	<select id="selectByItem3" parameterType="User" resultType="User">
		select * from USER
		<where>
			<choose>
				<when test="id !='' and id != null">
					and id = #{id}
				</when>
				<when test="username !=null and username !=''">
					and username=#{username}
				</when>
				<when test="password !=null and password !=''">
					and password=#{password}
				</when>
				<when test="address !=null and address !=''">
					and address=#{address}
				</when>
				<otherwise>
					and 1=1
				</otherwise>
			</choose>
		</where>	 
	</select>
```

#### 10.4set标签

```
set用法和上面where用法一致，可以生成set关键字，也可以去除sql关键字，如逗号（set标签中sql字符串最后的逗号）
```

```xml
<update id="update" parameterType="User">
		update USER
		<set>
			<if test="username !=null and username !=''">
				username = #{username},
			</if>
			<if test="password !=null and password !=''">
				password=#{password},
			</if>
			<if test="address !=null and address !=''">
				address=#{address},
			</if>
		</set>
		where id = #{id}
	</update>
```

#### 10.5 foreach标签

```
将传入sql语句中的数组类型或集合类型数据进行遍历操作。
```

```xml
<!-- 接口中参数为数组时，collection值必须为array -->
	<select id="selectByIds" resultType="User">
		select * from USER 
		<foreach collection="array" item="id" open="where id in(" separator="," close=")">
			#{id}
		</foreach>
	</select>

	<!-- 接口中参数为pojo，collection值必须为该数组类型属性名 -->
	<select id="selectByIds2" resultType="User" parameterType="User">
		select * from USER 
		<foreach collection="ids" item="id" open="where id in(" separator="," close=")">
			#{id}
		</foreach>
	</select>
```

#### 10.6trim标签

```
可以删除sql语句指定的字符串。
```

```xml
<!--
	prefix="前缀"：在后面sql字符串前面拼接指定的前缀。
	prefixOverrides="要去除的字符"：把紧跟着前缀后面的字符去掉。
	suffix="后缀"：在后面sql字符串后面拼接指定的后缀。
	suffixOverrides="要去除的字符"：把紧跟着后缀前面的字符去掉。
-->

<update id="update2" parameterType="User">
		update USER
			<trim prefix="set" prefixOverrides=",">
				<if test="username !=null and username !=''">
				,username = #{username}
				</if>
				<if test="password !=null and password !=''">
				,password=#{password}
				</if>
				<if test="address !=null and address !=''">
				,address=#{address}
				</if>
			</trim>
		where id = #{id}
	</update>
	
	<update id="update3" parameterType="User">
		update USER
			<trim prefix="set" suffixOverrides=",">
				<if test="username !=null and username !=''">
				username = #{username},
				</if>
				<if test="password !=null and password !=''">
				password=#{password},
				</if>
				<if test="address !=null and address !=''">
				address=#{address},
				</if>
			</trim>
		where id = #{id}
	</update>
```

### 11.性能优化

#### 11.1懒加载机制

```
进行多表关联查询时，如果关联中的数据暂时用不到，可以不去查询，当用到时再发送sql去数据库关联出来。  从而减少与数据库的交互行为；
注意：join查询不支持懒加载，多条select查询才支持懒加载；
```

#### 11.2实例步骤

```xml
1.在setting标签中设置lazy开关
	<settings>
		<!-- 开启lazy加载 -->
		<setting name="lazyLoadingEnabled " value="true"/>
	</settings>
2.使用select方式进行关联查询
```

**model类**

```java
//User类
public class User {
	private Integer id;
	private String username;
	private String password;
	private String address;
	private List<Orders> list;
｝
    
//Orders类
public class Orders {
	private int id;
	private String oname;
	private int oprice;
｝
```

**UserMapper接口**

```java
public interface UserMapper {	
	public User selectById(User user);
}
```

**UserMapper.xml**

```xml
<mapper namespace="lazy.mapper.UserMapper">
	<resultMap type="Users" id="baseResultMap">
		<id column="id" property="id"/>
		<result column="username" property="username"/>
		<result column="password" property="password"/>
		<!-- 关联集合数据,其中ofType用来说明集合中的数据类型 -->
		<collection property="list" ofType="Orders" select="lazy.mapper.OrdersMapper.selectByUid" column="id"></collection>
	</resultMap>
	
	<select id="selectById"  resultMap="baseResultMap">
		select * from USER where id = #{id}
	</select>
</mapper>
```

**OrdersMapper接口**

```java
public interface OrdersMapper {
	public List<Orders> selectByUid(int uid);
}
```

**OrdersMapper.xml**

```xml
<mapper namespace="lazy.mapper.OrdersMapper">
	<select id="selectByUid" resultType="Orders">
			select * from orders where uid = #{uid}
	</select>
</mapper>
```

**UserService类**

```java
public class UserService {
	public User findById(User user) {
		SqlSession session = MybatisUtil.findSqlSession();
		UserMapper mapper = session.getMapper(UserMapper.class);
		User findUser = mapper.selectById(user);
		session.close();
		return findUser;
	}
}
```

**Test类**

```java
public class TestLazy {
	UserService ser = new UserService();
	@Test
	public void testLazy(){
		User user = new User();
		user.setId(1);
		User findUser = ser.findById(user);
		//当只使用User表中数据时，只发送一条查询User表的sql语句。
		System.out.println(findUser.getUsername());
		//当主动使用关联表中数据时，mybaits才再次发送查询语句去查询关联表。如果不使用，则该条sql将不会发送给数据库。
		//System.out.println(findUser.getList());
	}
}
```

#### 11.3一级缓存

##### 3.1缓存的概念

```
当读取同一条SQL时，优先从内存中的缓存读取，如果能读到数据，则不需要交互数据库，如果读取不到，则会交互数据库，查询内容后放到缓存中以备下次查询时在缓存中能命中数据，提高程序响应速度。
```

##### 3.2mybatis的一级缓存

```
1.一级缓存是默认使用的；
2.一级缓存指的就是sqlSession范围内的缓存，在sqlSession中有一个数据区域，是map结构，这个区域就是一级缓存区域。一级缓存中的key是由sql语句、条件、statement等信息组成一个唯一值。一级缓存中的value，就是查询出的结果对象。
3.当数据对应的表发生增删改时，并执行commit操作时，默认会刷新一级缓存。
```

##### 3.3一级缓存测试

**UserMapper.xml**

```xml
<mapper namespace="cache1.mapper.UserMapper">
	<select id="selectById"  resultType="cache1.model.User">
		select * from USER where id = #{id}
	</select>
</mapper>
```

**UserMapper接口**

```java
public interface UserMapper {
	public User selectById(User user);
}
```

**UserService类**

```java
public class UserService {

	public User findById(User user) {

		SqlSession session = MybatisUtil.findSqlSession();

		UserMapper mapper = session.getMapper(UserMapper.class);

		//查询两次
		User findUser = mapper.selectById(user);
		System.out.println(findUser.getUsername());
		
        //如果不执行commit(),则发现执行两次查询时，只发送一条sql到数据库，则验证一级缓存存在。
		//当执行commit()操作时，mybatis上下文会默认你进行增删改数据了，则会清空一级缓存;
		session.commit(true);
		
		User findUser2 = mapper.selectById(user);
		
		System.out.println(findUser+"===="+findUser2.getUsername());

		session.close();

		return null;

	}

}
```

### 12.mybatis反向生成工具

#### 12.1步骤

```
1.Eclipse-->help菜单-->Eclipse Marketplace--->搜索mybatis-generator-->安装插件；
2.创建工程（maven project），用来放置反向生成的文件；
3.在工程中引入generatorConfig.xml，直接放在工程名下即可；
4.pom.xml中引入mybatis和mybatis-generator的jar包；
5.在generatorConfig.xml右键run:Run mybatis generator命令即可。
```

**pom.xml**

```xml
<dependencies>
  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.6</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.40</version>
		</dependency>
  </dependencies>
```

**generator.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
<!--数据库驱动-->
	<classPathEntry location="D:\apache-maven-3.5.4\reps\mysql\mysql-connector-java\5.1.40\mysql-connector-java-5.1.40.jar" />
	<context id="mysqlCxt" targetRuntime="MyBatis3">
<!--不生成注释-->
		<commentGenerator>
			<property name="suppressAllComments" value="true" />
			<property name="suppressDate" value="true" />
		</commentGenerator>
<!--数据库连接信息-->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
		connectionURL="jdbc:mysql://localhost:3306/javaee"
		userId="root"
		password="123">
		</jdbcConnection>
		<javaTypeResolver>
		<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		<!--model类-->
		<javaModelGenerator targetPackage="model" targetProject="mybatis_generator">
		<property name="enableSubPackages" value="true" />
		<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!--Mapper文件-->
		<sqlMapGenerator targetPackage="mapper" targetProject="mybatis_generator">
		<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		<!--mapper接口-->
		<javaClientGenerator type="XMLMAPPER" targetPackage="mapper" targetProject="mybatis_generator">
		<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		<!--依赖的表，%表示所有表-->
		<table tableName="User" >
			<columnOverride column="UNSIGNED_BIGINT_FIELD" javaType="java.lang.Object"
		jdbcType="LONG" />
		</table>
	</context>
</generatorConfiguration>
```

