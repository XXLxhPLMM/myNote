# SpringBoot数据层解决方案

## 1.sql

- 现在数据库解决方案技术选型

  ```
  Druid+Mybatis-Plus+MySQL
  ```

  - 数据源：使用的DruidDataSource

  ```yml
  #这种是用的比较多的
  #spring:
  #  datasource:
  #    druid:
  #      driver-class-name: com.mysql.cj.jdbc.Driver
  #      url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
  #      username: root
  #      password: 123456
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
      username: root
      password: 123456
      type: com.alibaba.druid.pool.DruidDataSource
  ```

  这是druid数据源配置的两种方式。这样多可以使用Druid数据源在测试中显示：

  ![](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231437114.png)

  假如说我们要是在配置数据源时没有表明Druid：

  ```yml
  spring:
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
      username: root
      password: 123456
  ```
  
  这种形式去操作，在进行测试中我们能看到：
  
  ![image-20220423143658638](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231436669.png)
  
  > 没有表明使用Druid，但是却是使用了Druid，这因为我们在pom.xml中有Druid的坐标
  >
  > ```xml
  > <!--        druid连接池-->
  > <dependency>
  >     <groupId>com.alibaba</groupId>
  >     <artifactId>druid-spring-boot-starter</artifactId>
  >     <version>1.2.6</version>
  > </dependency>
  > ```
  >
  > 这样配置会自动匹配到Druid数据源
  
  当我们把pom中的Druid的坐标给去掉，也能正常运行，在进行测试查看：
  
  ![image-20220423144318495](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231443526.png)
  
  可以看到一个HikariPool数据源：这个Hikari是SpringBoot内置的数据源，并且还可以进一步的对其进行配置：
  
  ```yml
  spring:
    datasource:
      url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
      hikari:
       driver-class-name: com.mysql.cj.jdbc.Driver
       username: root
       password: 123456
       ... #可以进行配置其他东西
  ```
  
  > 注意：其中url要是放到hikari下面会进行一个报错：
  >
  > ```ABAP
  > Description:
  > 
  > Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.
  > 
  > Reason: Failed to determine a suitable driver class
  > ```
  
  SpringBoot提供了3种内嵌的数据源对象供开发者选择使用
  
  1. HikariCP：默认内置数据源对象
  2. Tomcat提供DataSource：HikariCP不可用的情况下，且在web环境中，将使用Tomcat服务器配置的数据源对象
  3. Commons DBCP：HikariCP不可用，Tomcat数据源也不可用，将使用dbcp数据源
  
  ---
  
  
  
  - 持久化技术：Mybatis-Plus | Mybatis
  
  ​    数据层解决方案也可以使用JdbcTemplate来代替Mybatis-Puls|Mybatis。
  
  ![image-20220423150152371](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231501500.png)
  
  使用SpringBoot的Jdbc可以来解决持久化技术的方案：
  
  使用步骤：
  
  1. 坐标
  
  ```xml
  <!--       加载数据库驱动-->
  		<dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <scope>runtime</scope>
          </dependency>
  <!--        操作数据库的模板对象-->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-jdbc</artifactId>
          </dependency>
  ```
  
  > 注意使用JdbcTemplate时还是需要数据库的驱动的，但是不需要把mybatis坐标带
  
  2. yml配置需要进行连接
  
  ```yml
  spring:
    datasource:
      url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
      hikari:
       driver-class-name: com.mysql.cj.jdbc.Driver
       username: root
       password: 123456
  ```
  
  > 这里使用HikariCP内置的数据源进行操作
  
  3. 进行测试
  
  ```java
  @SpringBootTest
  public class DBTest {
  
      @Autowired
      private BookService bookService;
  
      /**
       * 进行更换数据源测试
       */
      @Test
      public void test1(){
          List<Book> list = bookService.listAll();
  //        System.out.println(list);
      }
  
      /**
       * 更换持久化方案
       * @param jdbcTemplate 使用JdbcTemplate来进行持久化
       */
      @Test
      public void test2(@Autowired JdbcTemplate jdbcTemplate){
          List<Book> query = jdbcTemplate.query("select * from book", new BeanPropertyRowMapper<Book>(Book.class));
          System.out.println(query);
      }
  }
  ```
  
  并且在yml中JdbcTemplate还可以进行一些配置：
  
  ![image-20220423152922367](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231529401.png)
  
  ---
  
  
  
  - 数据库：MySQL

SpringBoot还给我们内置了一些数据库：

​	H2；

​	HSQL；

​	Derby

但是这些内置数据库也有各种的优缺点，若是想要了解可以去他们的官网学习一下。

技术选型：

![image-20220423153950239](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231539292.png)

以及其他的技术选型。

## 2.Nosql

### 1.redis

 redis是一款key-value存储结构的内存级NoSQL数据库

- 支持多种数据存储格式
- 支持持久化
- 支持集群

> 可以查看<a href="https://www.cnblogs.com/qinsir817/p/16182634.html">SpringBoot整合Redis</a> 

### 2.MongoDB

> 可以查看<a href="https://www.runoob.com/mongodb/mongodb-tutorial.html">MongoDB教程</a> 来源于菜鸟教程

### 3.ES(Elasticsearch)

> 可以查看<a href="https://www.bootwiki.com/elasticsearch/elasticsearch-tutorial.html">ES教程</a> 来源于菜鸟教程

