## SpringBoot整合第三方技术

### 1.整合Junit

​	整合Junit，就类似我们学习Spring的SpringJunit类似，让我们进行注解测试。

步骤：

- 导入对应的starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

- 在测试类中使用@SpringBootTest进行修饰
- 在类中写方法方法上使用@Test注解

> 注意：当我们在使用test时，需要常见一个包要和我们java源码中的Demo1Applicationd的路径一致，要不然会把错

```java
//加入引导类，可以处理由于位置原因
// 导致无法在测试类中引用的其他的
// 接口配置。
@SpringBootTest(classes = Demo1Application.class)
class Demo1ApplicationTests {

    @Autowired
    private BookMapper bookMapper;

    @Test
    void contextLoads() {
        List<Book> list = bookMapper.selectList(null);
        for (Book book:list){
            System.out.println(book);
        }
        System.out.println("text。。。");
    }
}
```

但是当使用`@SpringBootTest(classes = Demo1Application.class)`可以解决上面的问题。

### 2.整合mybatis

​	我们自需要在创建项目时在选择技术时，找到SQL ，在右侧我们要选在MySQL Driver（SQL驱动）和Mybatis Farmework,这样我们就能使用mybatis的技术。

​	接着我们需要配置驱动信息（yml中去配置）：

```yml
datasource1:
   drive: com.mysql.cj.jdbc.Driver
   url: jdbc:mysql://localhost:3306/mybatisdemo?serverTimezone=UTC
   userName: root
   password: 123456
#数据库的连接
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: 123456
    url: ${datasource1.url}
```

下面的操作就是mapper的接口和之前mybatis的操作一致。

> 在我们当我们把我们驱动版本降低，会出现一个错误（时区设置的问题），导致我们无法连接到数据库。解决方法：在数据库里去设置时区；也可以直接在连接时在后面添加上serverTimezone=UTC可以帮我们解决这个问题 
>
> 驱动类过时：提醒更换为com.mysql.cj.jdbc.Driver

### 3.整合MP

​	在SpringBoot中有两种方法可以使用MP，第一种方法，我们在开始创建项目在我们选择了SpringBoot后项目后；我们可以不用它给我的地址

![image-20220415164947864](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151649913.png)

而去使用阿里云提供的地址去进行创建http://start.aliyun.com。

在进行选择技术时我们可以在sql中可以选择MP，而在它自己给我们的sql技术选择中没有MP。

​	这样创建完成后在pom.xml中会

```xml
<!--        mybatis-plus在springBoot的使用 需要数据库驱动才能进行使用-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
```

​	第二种方法去创建整合MP：我们自需在按照原始的创建项目一样，在选择技术时，我们选择sql驱动的技术就可以了，但是我们需要在pom文件中去手动添加坐标（MP坐标）：

```xml
<!--        mybatis-plus在springBoot的使用 需要数据库驱动才能进行使用-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
```

上面的两种方式可以把SB和MP整合起来一起使用

### 4.整合Druid

第一步：我们需要到入相应的starter

```xml
<!--        druid连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.6</version>
        </dependency>
```

第二步：配置连接池的相应数据：

```yml
datasource1:
   drive: com.mysql.cj.jdbc.Driver
   url: jdbc:mysql://localhost:3306/mybatisdemo?serverTimezone=UTC
   userName: root
   password: 123456

#配置路径
#server:
#  port: 8080
#  servlet:
#context-path的配置可以使用我们的
#路径上添加上locathost:8080/test/...
#    context-path: /test

#当我们需要整合第三方技术
#我们一般需要几步：导入对应的starter
#配置对应的设置或采取默认配置
#连接池
spring:
  datasource:
    druid:
      driver-class-name: ${datasource1.drive}
      url: ${datasource1.url}
      username: ${datasource1.userName}
      password: ${datasource1.password}
```

通过这两步就已经把我们的Druid整合到一起了。

