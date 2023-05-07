## SpringBoot第三bean属性绑定

### 1.属性绑定

1. 下面的区别：

```java
@ConfigurationProperties(prefix = "user")
```

```java
@EnableConfigurationProperties(User.class)
```

```java
@Component
```

首先我们在配置文件中定义一些属性：

```yml
user:
  myname: "张三"
  age: 15
```

> 注意：我们在使用@Value获取yml中的属性需要使用@Value("${user.myname}")

这样我们定义一个User实体类去分装配置文件的user属性里面的内容

```java
//使用@ConfigurationProperties获取配置文件中的属性，需要配合这@Component
//原因：@ConfigurationProperties相当于在spring中的配置中进行注入
// 我们属性的值，这才需要我们把User进行实例化bean对象
@Component
@Data
@ConfigurationProperties(prefix = "user")
public class User {
    private String myName;
    private Integer age;
}
```

​	在我们进使用@ConfigurationProperties(prefix = "user")需要先当前类进行分装成bean对象才能使用。

​	在项目中的application类：

```java
@SpringBootApplication
public class OperationApplication {

    public static void main(String[] args) {
//        关闭热部署
//        System.setProperty("spring.devtools.restart.enabled","false");
        ConfigurableApplicationContext run = SpringApplication.run(OperationApplication.class, args);
        User bean = run.getBean(User.class);
        System.out.println(bean);

    }
}
```

​	可以获取到User封装的bean对象，运行后显示的结果：

```
User{myName='张三', age=15}
```



​	使用**@EnableConfigurationProperties(User.class)**在application类上面也可以帮我们把User分装成bean对象，

> 注意：当我们使用了@EnableConfigurationProperties(User.class)  但是在User上还使用了@Component  就会报一个错误
>
> ```
> Caused by: org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type 'com.qinfeng.domain.User' available: expected single matching bean but found 2: user,user-com.qinfeng.domain.User
> ```
>
> 这是说我们把User进行了两次实例化bean的错误。
>
> 这样我们只用把@Component去掉就可以了

```java
@SpringBootApplication
@EnableConfigurationProperties(User.class)
public class OperationApplication {
    ......
}
```

这样也是可以把配置文件中的属性值输出出来：

```
User{myName='张三', age=15}
```

> 注意：在我们使用@EnableConfigurationProperties(User.class)这个时，需要把我们需要进行实例化bean的对象写到（）里面把它加入spring容器里，要不然我们User那边的@ConfigurationProperties(prefix = "user")会报错。
>
> 而@EnableConfigurationProperties()可以填写很多使用@EnableConfigurationProperties({User.class,....,..})

但是在pom.xml中需要添加一个坐标，来解除注解报警：

```xml
<!--        解除@ConfigurationProperties注解警告-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```



### 2.松散绑定

![image-20220417123345922](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204171233088.png)

> 注意：
>
> 1. 宽松绑定不支持使用@Value引用单个属性的方式
> 2. 在使用@ConfigurationProperties(prefix = "user")中的前缀命名规范仅能使用纯小写字母，数字，下划线作为合法的字符



### 3.常用计量单位

1. 时间：

   在yml中：

```yml
serverTime: 3
```

```java
@DurationUnit(ChronoUnit.HOURS)
private Duration serverTime;
```

在运行后显示serverTime=PT3H,，表示三小时。

其中的表示：

![image-20220417124923517](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204171249568.png)

2. 存储空间

   在yml中

   ```yml
   dataSize: 10
   ```

   ```java
   @DataSizeUnit(DataUnit.MEGABYTES)
   private DataSize dataSize;
   ```

   在运行后显示 dataSize=10485760B，

![image-20220417125142418](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204171251462.png)



### 4.数据校验

1. 开启bean的数据校验，先进行导入JSR303规范和Hibernate校验框架对应的坐标

```xml
<!--        导入JSR303规范 进行数据校验-->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
        </dependency>
<!--        使用hibernate框架提供的校验器进行实现类中数据规范-->
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
        </dependency>
```

2. 开启数据校验

```java
//开启数据校验
@Validated
```

```java
@Data
@ConfigurationProperties(prefix = "user")
//开启数据校验
@Validated
public class User {
    private String myName;
    @Max(value = 150,message = "要是修仙吗？这么大的年龄？")
    @Min(value = 0,message = "还没有出生呢。")
    private Integer age;
}
```

在配置文件中去添加 age: 1500

    Description:
    
    Binding to target org.springframework.boot.context.properties.bind.BindException: Failed to bind properties under 'user' to com.qinfeng.domain.User failed:
    
    Property: user.age
    Value: 1500
    Origin: class path resource [application.yml] - 45:8
    Reason: 要是修仙吗？这么大的年龄？
    
    Action:
    
    Update your application's configuration

若是在配置文件中 age: -1

```
    Property: user.age
    Value: -1
    Origin: class path resource [application.yml] - 45:8
    Reason: 还没有出生呢。
```



### 5.进制数字转换规则

![image-20220417131628416](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204171316463.png)

