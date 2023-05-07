## SSM详细项目部署

​	利用SSM做一个简单的页面，页面上可以展示出数据库中的全部数据。

​	在知道需要做一个查询全部数据的的页面后，首先我们需要使用IDEA创建一个项目，File>new>project;在进行入创建项目的页面下，进行选择需要部署的项目，这里我们选择创建一个maven项目，选择如下：

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220408223715133.png" alt="image-20220408223715133" style="zoom: 67%;" />

在进入下一步next后，进入一个页面是进行对项目进行命名的，

![image-20220408224029734](C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220408224029734.png)

> 在这个页面不同的IDEA版本和设置不一样，但是也是和这个差不多的，若是不一样，可以去百度如何规范填写。

然后下一步，到

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220408224541859.png" alt="image-20220408224541859" style="zoom:67%;" />

然后进行下一步来到，我们项目需要存放的位置选择好点击完成。

​	下面是我们项目的结构：

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220408225159666.png" alt="image-20220408225159666" style="zoom: 50%;" />

> 在创建好后我们需要自己手动的去添加各种包，其中在main包下的java包和resources包有啥创建时，并不是上面的样子而是一种灰色的这就需要我们进行改动，我们可以选择做java包后点击右键在里面找到 **Mark Directory as**（标记目录为）下选择 **Test Sources Root**（源码 根）就可以了，java、resources这包皆可以实现



---



### 项目步骤

### 	1、创建数据访问层：

​			1.1、首先我们在数据库中创建一个表（以book为例）后，我们需要在domain包下进创建一个封装book表的实体类Book：

```java
public class Book {
    private Long id;
    private String sn;
    private String name;
    private String author;
    private BigDecimal price;

    private Long dirId;

    @Override
    public String toString() {
        return "Book{" +
                "id=" + id +
                ", sn='" + sn + '\'' +
                ", name='" + name + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                ", dirId=" + dirId +
                '}';
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getSn() {
        return sn;
    }

    public void setSn(String sn) {
        this.sn = sn;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public BigDecimal getPrice() {
        return price;
    }

    public void setPrice(BigDecimal price) {
        this.price = price;
    }

    public Long getDirId() {
        return dirId;
    }

    public void setDirId(Long dirId) {
        this.dirId = dirId;
    }
}
```

> 实体类中变量需要和数据库中创建的表的数据类型，名字一致，这样才能把属性封装上去

​		1.2、创建数据访问层（javaweb中是DAO层），使用mybatis创建一个mapper来代替DAO，在mapper下创建一个接口

```java
public interface BookMapper {
    /**
     * 使用注解方法来完成相应的数据库操作
     * @return
     */
//    @Select("select * from book")
    List<Book> list();
}
```

> 其中我们创建好接口后，需要去实现接口，在BookMapper.xml中去编写方法，也可以使用mybatis注解来实现，建议想简单的操作使用注解，复杂的可以在xml中进行编写。

​		1.3、创建mapper文件（xml），这个我们需要在 resources资源根下创建出一个和BookMapper接口一样的目录结构，这样才能识别到接口（详细原因可以查资料，这里暂不做解释）并且名字也要一样，只不过是xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.qinfeng.mapper.BookMapper">

    <select id="list" resultType="com.qinfeng.domain.Book">
        select * from book
    </select>
</mapper>
```

> 其中的id为list这个是和我们接口中的方法名是一样，要是不一样，那我们就无法进行使用这个sql语句

​		1.4、我们还需要创建在resources下建一个mybatis配置文件

```xml
<!-- 配置文件的根元素 -->
<configuration>
    <!--    类的别名-->
    <typeAliases>
        <package name="com.qinfeng.domain"/>
    </typeAliases>
  
</configuration>
```

> 这里按照我们之前学习mybatis时，这里还有很多配置没有写，如数据库的环境配置，加载properties文件，以及加载我们的mapper映射文件，因为我们在使用spring是在它配置文件也有其内容重复，进而简化。

### 	2、创建业务逻辑层

​		2.1、在service包下创建一个接口，并创建一个包（impl）用来存放接口实现的，先创建接口：

```java
public interface Book {

    List<com.qinfeng.domain.Book> list();
}
```

​		2.2、在impl包下创建一个类来继承Book接口并实现接口中的方法

```java
@Service("bookImpl")
public class bookImpl implements Book {

    @Autowired
    private BookMapper bookMapper;

    @Override
    public List<com.qinfeng.domain.Book> list() {
        List<com.qinfeng.domain.Book> list = bookMapper.list();
        return list;
    }
}
```

​		2.3、这里的实现类使用spring的注解，那我们还需要在resources下建一个bean文件（applicationContext.xml）：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
<!--    1.使用注解进行扫描-->
    <context:component-scan base-package="com.qinfeng">
<!--        1.1扫描的同时我们会排除这个Controller-->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
<!--2.导入我们db配置文件-->
    <context:property-placeholder location="classpath:db.properties"/>

<!--    3.配置数据池-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
     </bean>
<!-- 4.这是把我们mybatis配置文件集成在spring配置文件中-->
    <!--4.1配置sessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--4.1.1加载mybatis核心文件-->
        <property name="configLocation" value="classpath:mybatisSpring.xml"/>
    </bean>

    <!--4.2扫描mapper所在的包 为mapper创建实现类-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.qinfeng.mapper"/>
    </bean>

    <!--4.3声明式事务控制-->
    <!--平台事务管理器 把我们的数据源配置到mybatis中-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--4.4配置事务增强-->
    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <!--5事务的aop织入-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.qinfeng.service.impl.*.*(..))"></aop:advisor>
    </aop:config>
    
</beans>
```

> 上面代码有注释，要是不明白需要在去学习一下ssm的整合以及spring事务处理

### 	3、表示层

​		由于使用SpringMVC，之前的servlet就不用了，被Controller替代

​		3.1、在domain包的同级下创建Controller包，在包下建一个Controller的视图业务逻辑的管理

```java
/**
 * 无法直接使用@Controller的原因：
 * 由于我们写的这个Controller类的名字就是Controller
 * 所以当我们使用@Controller时，系统能无法识别是那个类会进行报错
 * 但是我们还是需要@Controller就需要带上他org.springframework.stereotype.Controller
 */
@org.springframework.stereotype.Controller
//这里加上注解，添加路径，那么下面的方法在访问时，路径都需要有/zc
//@RequestMapping("/zc")
public class Controller {

    @Autowired
    private Book bookImpl;

    @RequestMapping(value = "/book")
    public ModelAndView list(){
        List<com.qinfeng.domain.Book> list = bookImpl.list();
        System.out.println(list);
        ModelAndView modelAndView = new ModelAndView();
//存储数据
        modelAndView.addObject("list",list);
//        转发页面
        modelAndView.setViewName("findAll");
        return modelAndView;
    }

}
```

​		3.2、在Controller中我们使用了springMVC的注解，那我们需要在resources下建一个读取spring-mvc.xml的文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">
<!--    1.注册驱动-->
    <mvc:annotation-driven/>

<!--    2.扫描Controller包-->
    <context:component-scan base-package="com.qinfeng.contorller"/>

<!--    3.设置我们页面资源读取-->
    <!--内部资源视图解析器-->
    <bean id="resourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--4.开发静态资源访问权限-->
    <mvc:default-servlet-handler/>

</beans>
```

> 这里为什么有新建一个springmvc的配置文件？难道之前创建的bean文件不能读取到这些注释吗？  答案是可以读取到的，但是我们为了使前端与后端配置进行分离才进行这样的操作。

​		3.3、当我们进行页面跳转，需要进行编写相应的页面，我们在webapp下创建；

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@page isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<table border="1" cellspacing="0" width="80%" align="center" style="margin: auto">
    <tr style="color: lightskyblue">
        <th>序号</th>
        <th>图书编号</th>
        <th>书名</th>
        <th>作者</th>
        <th>价格</th>
        <th>分类</th>
        <th>操作</th>
    </tr>
    <c:forEach items="${list}" var="i" varStatus="s">
        <tr id="hang${s.count}" style="color: gray">
            <td>${s.count}</td>
            <td>${i.sn}</td>
            <td>${i.name}</td>
            <td>${i.author}</td>
            <td>${i.price}</td>
            <td>${i.dirId}</td>
            <td><a href="#">修改</a>&nbsp;
                <a href="#">删除</a></td>
        </tr>
    </c:forEach>
</table>
</body>
</html>
```

> 我们访问时一般都是到index.jsp页面，可以在这个页面上加上
>
>  `<jsp:forward page="${pageContext.request.contextPath}/book"></jsp:forward>`
>
> 可以自动跳转去访问Controller下的/book 路径下的资源

​		3.4、做完以上步骤，恭喜你，你距离成功只差一步了。因为我们还需要去配置我们web.xml使用的我们的spring配置可以被前端识别。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">

  <!--spring 监听器-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!--springmvc的前端控制器-->
  <servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!--乱码过滤器-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>
```

好的在一切正常的情况下我们可以看到页面的展示的数据

---

### 补充项目所需坐标：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.qinfeng</groupId>
  <artifactId>ssm</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>


  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <!--spring相关-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.7</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.5.RELEASE</version>
    </dependency>

    <!--servlet和jsp 这两留能回和自己我Tomcat中的配置重复，导致无法识别用哪个而出错-->
<!--    <dependency>-->
<!--      <groupId>javax.servlet</groupId>-->
<!--      <artifactId>servlet-api</artifactId>-->
<!--      <version>2.5</version>-->
<!--    </dependency>-->
<!--    <dependency>-->
<!--      <groupId>javax.servlet.jsp</groupId>-->
<!--      <artifactId>jsp-api</artifactId>-->
<!--      <version>2.0</version>-->
<!--    </dependency>-->

    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

    <!--mybatis相关-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.5</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
    </dependency>
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.10</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
    </dependency>
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>

  </dependencies>
  <!--        Tomcat-->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <!--                <artifactId>tomcat</artifactId>-->
        <version>2.2</version>
        <configuration>
          <path>/</path>
          <port>8080</port>
          <uriEncoding>UTF-8</uriEncoding>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```



### 打印日志的log4j.properties:

```properties
#
# Hibernate, Relational Persistence for Idiomatic Java
#
# License: GNU Lesser General Public License (LGPL), version 2.1 or later.
# See the lgpl.txt file in the root directory or <http://www.gnu.org/licenses/lgpl-2.1.html>.
#

### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file hibernate.log ###
#log4j.appender.file=org.apache.log4j.FileAppender
#log4j.appender.file.File=hibernate.log
#log4j.appender.file.layout=org.apache.log4j.PatternLayout
#log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=all, stdout

```



### 数据库信息db.properties:

```properties
# jdbc.driver=com.mysql.cj.jdbc.Driver,数据库是八以上的使用这个
jdbc.driver=com.mysql.jdbc.Driver
# jdbc.url=jdbc:mysql:///数据库名称
jdbc.url=jdbc:mysql://localhost:3306/数据库名称
jdbc.username=root
jdbc.password=自己的数据库密码
```



> 注意：在编写时，一定要注意单词，单词出错真的很难受呀~ 

