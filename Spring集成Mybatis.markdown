- [Spring集成](#spring集成)
- [**`db.properties`** 配置数据库](#dbproperties-配置数据库)
- [**`Spring.xml`** 配置](#springxml-配置)
  - [添加命名空间](#添加命名空间)
  - [导入数据库预设](#导入数据库预设)
  - [组件扫描](#组件扫描)
  - [数据源注入](#数据源注入)
  - [配置`SqlSession`工厂`Bean`](#配置sqlsession工厂bean)
  - [批量扫描`映射文件`](#批量扫描映射文件)
- [**`java`**](#java)
  - [测试对象](#测试对象)
  - [`mapper`接口](#mapper接口)
  - [`映射`文件](#映射文件)
  - [测试](#测试)
- [**pom.xml**](#pomxml)
  - [`MyBatis` 依赖](#mybatis-依赖)
  - [`Spring`](#spring)
  - [测试](#测试-1)
  - [项目共用](#项目共用)
- [SpringBoot](#springboot)
  - [条件装配](#条件装配)
  - [资源导入](#资源导入)
  - [配置绑定](#配置绑定)

# Spring集成
# **`db.properties`** 配置数据库
```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=UTF8&autoReconnect=true&useSSL=false
u_name=账号
password=密码
```
# **`Spring.xml`** 配置
## 添加命名空间
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd"></beans>
```
## 导入数据库预设
> `location`属性值前面加上classpath
```xml
<context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
```
## 组件扫描
```xml
<context:component-scan base-package="包名"></context:component-scan>
```
## 数据源注入
> 使用Druid连接池 [依赖](#mybatis-依赖)
```xml
<bean id="druid" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${driver}"></property>
    <property name="password" value="${password}"></property>
    <property name="username" value="${u_name}"></property>
    <property name="url" value="${url}"></property>
</bean>
```
## 配置`SqlSession`工厂`Bean`
```xml
    <bean id="sqlS" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!-- 选择数据源 -->
        <property name="dataSource" ref="druid"></property>
    <!-- 可选  设置项目别名 -->
        <property name="typeAliasesPackage" value="包名"></property>
    </bean>
```
## 批量扫描`映射文件`
```xml
    <bean id="configurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="包名"></property>
    </bean>
```
# **`java`** 
## 测试对象
```java
public class TestObj {
    long id;
    String name;
    String password;

    public TestObj() {
    }

    public TestObj(long id, String name, String password) {
        this.id = id;
        this.name = name;
        this.password = password;
    }

    /* getter and setter */

    @Override
    public String toString() {
        return "TestObj{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```
## `mapper`接口
> 必须在接口前面加上`@Repository`
```java
@Repository
public interface TestMapper {
    List<TestObj> save();
}
```
## `映射`文件
> TestMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="mapper接口的全限定名">
    <insert id="insert" parameterType="testObj">
        insert into spring_class(name, password) VALUE(#{name},#{password})
    </insert>
</mapper>
```
## 测试
> 这里同样别忘了加上`classpath`
```java
@ContextConfiguration("classpath:Spring.xml")
@RunWith(SpringJUnit4ClassRunner.class)
public class AtTest {
    /* 自动装载 */
    @Autowired
    private TestMapper testMapper;
    @Test 
    public void test1() throws SQLException {
        testMapper.save();
    }
}
```
# **pom.xml**
## `MyBatis` 依赖
```xml
    <!-- mybatis-->
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
    <!-- 这个不是 这是日志 -->
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>1.7.25</version>
    </dependency>
```
## `Spring`
```xml
    <!-- spring -->
    
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.13</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.8.RELEASE</version>
    </dependency>
```
## 测试
```xml
    <!-- 测试相关 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.8.RELEASE</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13</version>
      <scope>test</scope>
    </dependency>
```
## 项目共用
```xml
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.12</version>
      <scope>provided</scope>
    </dependency>
```

# SpringBoot
## 条件装配
```java
@ConditionalOnBean(类名.class)
@ConditionalOnBean(name="组件id")
/* 容器中如果
有
这个 组件 那么被此注解标记的组件才会被 加载到容器*/
@ConditionalOnMissingBean(类名.class)
@ConditionalOnMissingBean(name="组件id")
/* 容器中如果 
没有
这个 组件 那么被此注解标记的组件才会被 加载到容器*/
```
## 资源导入
`@ImportResource("目录")`
## 配置绑定
`@ConfigurationProperties`所在的类是Spring组件 或被开启了配置绑定功能

`@EnableConfigurationProperties(类名.class)`开启配置绑定功能
