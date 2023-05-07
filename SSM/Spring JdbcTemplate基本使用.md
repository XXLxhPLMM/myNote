## Spring JdbcTemplate基本使用

一、jdbcTemplate的概述

​	它是spring框架中提供的一个对象，是对原始繁杂的jdbc 对象的简单封装。

二、使用步骤：

​	1、导入坐标

```xml
<!--        jdbcTemplate坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.0.5.RELEASE</version>
</dependency>
<!--        业务功能管理-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.0.5.RELEASE</version>
</dependency>
```

​	2、创建数据库和实体类

​	3、创建JdbcTemplate对象

~~~	java
    @Test
    public void test1(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName("com.mysql.jdbc.Driver");
        druidDataSource.setUrl("jdbc:mysql:///mybatisdemo");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("123456");
        JdbcTemplate template = new JdbcTemplate();
        template.setDataSource(druidDataSource);
        int i = template.update("insert into spring_res(name,password) values (?,?)", "枫", "991002");
        System.out.println(i);
    }
~~~

​	我们进行创建它的对象时还是需要我们先一步进行操作连接数据池。在这创建好后，在去创建JdbcTemplate的对象，并把数据池传入连接

~~~ java
		JdbcTemplate template = new JdbcTemplate();
        template.setDataSource(druidDataSource);
~~~

​	4、操作数据库

更新操作（添加，删除，修改）使用：

```java
template.update("update spring_res set name=? where id=?","封","1");
```

查询操作（查询全部，条件查询，数量）：

```java
/**
 * 查询全部使用query方法 使用new BeanPropertyRowMapper<User>(User.class)，
 * 将帮我们自动封装成一个list集合
 */
@Test
public void test3(){
    List<User> query = template.query("select * from spring_res", new BeanPropertyRowMapper<User>(User.class));
    System.out.println(query);
}

/**
 * 按条件查询是依旧使用new BeanPropertyRowMapper<User>(User.class)
 * 但是我们按条件查询时，需要进行传值，需要使用
 * queryForObject 将会返回我们查询的一个对象实例
 */
@Test
public void test4(){
    User user = template.queryForObject("select * from spring_res where id=?", new BeanPropertyRowMapper<User>(User.class), 11);
    System.out.println(user);
}

/**
 * 查找条目，在使用queryForObject这个方法，可以在后面带上需要返回的类型，
 * 这样可以帮我们进行强转，
 * Long aLong = template.queryForObject("select count(*) from spring_res", Long.class);
 */
@Test
public void test5(){
    Long aLong = template.queryForObject("select count(*) from spring_res", Long.class);
    System.out.println(aLong);
}
```

> 在上面中我们可以发现在第三条中的测试用例用原始的方法来操作数据库很繁琐，而spring框架提供了一些封装方法，来简化操作。这在第四条中也可以清楚的看到。

Spring 简化JdbcTemplate：

​	1、在spring核心配置文件中配置JdbcTemplate

```xml
<!--    加载外部的properties文件-->
    <context:property-placeholder location="classpath:db.properties"/>

<!--    把数据库链接的部分进行分开，进一步解耦-->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource" >
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
<!--    进行扫描包下的注解-->
    <context:component-scan base-package="com.qinfeng"/>

<!--  jdbcTemplate  -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
```

> 而在我们使用外部文件的db.properties时，需要使用到context标签，这就需要我们在spring核心文件的跟标签中进行添加

添加context标签：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context ="http://www.springframework.org/schema/context"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
```

​	2、使用注解注入JdbcTemplate

```java
@Resource
private  JdbcTemplate template;
```

​	3、测试使用

```java
/**
 * 按条件查询是依旧使用new BeanPropertyRowMapper<User>(User.class)
 * 但是我们按条件查询时，需要进行传值，需要使用
 * queryForObject 将会返回我们查询的一个对象实例
 */
@Test
public void test4(){
    User user = template.queryForObject("select * from spring_res where id=?", new BeanPropertyRowMapper<User>(User.class), 11);
    System.out.println(user);
}
```

