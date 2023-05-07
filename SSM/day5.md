# Spring-JDBC

JDBC连接数据库的几大方式：

1. #### 最原始方式

> 注意：
>
> 1. mysql版本如果是8.几的版本话，在加载数据库驱动的时候加**cj**，8.几版本以下的就不用加
> 2. 先创建的资源需要后释放

```java
@Test
    public void test2() throws Exception {
    	//1.加载数据库驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
    	//2.获取连接对象
        String url = "jdbc:mysql:///bms";
        String username = "root";
        String password = "123456";
        Connection connection = DriverManager.getConnection(url,username,password);
    	//3.创建语句对象（创建一个Statement对象，用与发送SQL语句到数据库）
        Statement statement = connection.createStatement();
    	//4.写Sql语句
        String sql = "select * from book";
    	//5.校验SQL语句
        ResultSet resultSet = statement.executeQuery(sql);
        System.out.println(resultSet);
    	//6.释放资源（先创建的后释放）
        resultSet.close();
        statement.close();
        connection.close();
    }
```

![1648286201433](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1648286201433.png)

> 显示为这种情况的就说明jdbc连接数据库成功

2. **使用spring框架集成jdbc**

第一步：创建maven项目

第二步：在pom.xml文件中添加依赖

```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.3.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.12</version>
        </dependency>
```

第三步：在main目录下的resource包里面添加配置文件

1. jdbc.properties（数据库连接对象信息）

```properties
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///数据库名
jdbc.username=数据库连接的用户名
jdbc.password=数据库连接的密码
```

2. applicationContext.xml（spring专门的配置文件）

![1648287075480](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1648287075480.png)

```xml
	<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       
       xmlns:context="http://www.springframework.org/schema/context"
       
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           
                           http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
                           
                           ">
    
    <!--首先得引入我们的数据连接对象的文件jdbc.properties-->
    <!--使用context标签，得先去beans头文件里面去添加context标签头-->
    <context:property-placeholder locaation="jdbc.properties(数据库连接对象的文件)"></context:property-placeholder>
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```

第四步：配置完成后去测试代码中测试是否连接成功

```java
@Test
    public void test3() throws SQLException {
        ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
        DriverManagerDataSource dataSource = ac.getBean("dataSource", DriverManagerDataSource.class);
        Connection connection = dataSource.getConnection();
        System.out.println(connection);
    }
```

![1648293396342](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1648293396342.png)

> 运行结果如图说明数据库连接成功

3. 目前最实用的方式，spring的数据库连接池

第一步：创建maven项目

第二步：在pom.xml文件中再添加一个依赖包，阿里巴巴的德鲁伊连接池

```xml
		<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.1</version>
        </dependency>
```

第三步：修改上面第三步的applicationContext.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       
       xmlns:context="http://www.springframework.org/schema/context"
       
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           
                           http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
                           
                           ">
    
    <!--首先得引入我们的数据连接对象的文件jdbc.properties-->
    <!--使用context标签，得先去beans头文件里面去添加context标签头-->
    <context:property-placeholder locaation="jdbc.properties(数据库连接对象的文件)"></context:property-placeholder>
    <bean id="DruidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driverClassName}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```

第四步：配置完成后去测试代码中测试是否连接成功

```java
@Test
    public void test1() throws SQLException {
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
        DruidDataSource druidDataSource = ac.getBean("DruidDataSource", DruidDataSource.class);
        DruidPooledConnection connection = druidDataSource.getConnection();
        System.out.println(connection);
    }
```

![1648294145612](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1648294145612.png)

> 说明连接成功