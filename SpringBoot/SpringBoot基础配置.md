## SpringBoot基础配置

###  1.配置文件的一些配置

#### 修改服务器的端口

​	在我们使用SpringBoot时，它自己内部有Tomcat的配置，当我们进行运行SB时在idea的控制台上，我们将会看到上面写的8080端口：

![image-20220415164601691](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151646759.png)

然后我们可以在网站上去输入`http://localhost:8080`我们可以看到

![image-20220415164617672](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151646720.png)

`application.properties`我们可以在配置文件中去编写`server.port=80`我们可以发现我们控制面板下的8080已经变成了80端口，这样我们就可以在浏览器中的输入`localhost`就可以把页面打开。

#### 修改banner 运行图标

```properties
#修改banner 图表
#关闭运行日志图标
spring.main.banner-mode=off
#换成照片
#spring.banner.image.location=cat.png
```

在我们的控制台上就不会看到我们spring的标记。

#### 运行日志

```properties
#日志 默认为info
logging.level.root=info

#logging.level.root=debug
```

> 注意上面的这些并不是我们项目原有，而是通过我们的SB的坐标加载后，可以使用里面的内容，才使我们的properties中可以去配置这些。下面的我们的坐标
>
> ```xml
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-web</artifactId>
> </dependency>
> ```



---



###  2.配置文件的使用选择

​	我们在创建SB项目时，我们的原始配置文件是application.properties,但是我们在日常编程中，我们经常使用的application.yml配置文件，其中还有yaml文件，但是我们的主流文件是使用我们的yml文件，但是在创建yml文件时，不能使用，!![image-20220415164657921](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151646951.png)，那我就需要把它变成我们的!![image-20220415164713848](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151647898.png)就可以了；

​	这就需要们在项目结构中去进行配置了。

![image-20220415164643356](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151646448.png)

其中，当properties和yaml以及yml同时存在时，那properties的优先级大于yml大于yaml，若是三个里面有相同的内容，那样优先级大的会覆盖优先级低的，若是里面的配置不同，那样他们是同时可以应用。

- yaml的数据化的格式：
  + 大小写敏感
  + 属性层级关系，同层级左侧对齐，只允许使用空格
  + #表示注释
  + 属性名与属性值之间需要有个空格分开

```yml
#推荐使用yml（主流）
#使用yml格式要求相对交严格
#核心规则：数据前要加空格与冒号隔开。
#server:
#  port: 80

#数组格式
liks:
  - game
  - music
  - sleep

liks1: [game,music,sleep]

user:
  ymlName: zhangsan
  age: 18

#对象数组
user1:
  - name: zhangsan
    age: 18
  - name: lisi
    age: 74

user2: [{name: zhangsan,age: 18},{name: lisi,age: 17}]

datasource1:
   drive: com.mysql.cj.jdbc.Driver
   url: jdbc:mysql://localhost:3306/mybatisdemo?serverTimezone=UTC
   userName: root
   password: 123456

#配置路径
#server:
#  port: 8080
#  servlet:
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
#数据库的连接
#spring:
#  datasource:
#    driver-class-name: com.mysql.jdbc.Driver
#    username: root
#    password: 123456
#    url: ${datasource1.url}


#设置Mp相关的配置
#下面的可以设置数据库的表名开头以tbl_开头
#mybatis-plus:
#  global-config:
#    db-config:
#      table-prefix: tbl_
##      表示数据库的id是自动递增
#      id-type: auto
```



---

###  3.读取yml中配置

- 使用我们@Value注解：@Value("${一级属性名.二级属性名...}")

下面是几种读取方式：

```java
//    读取yml中的数据
//    注意：我们在调用yml中的user数据name时
//    可能会把我们自己的用户名获取出来
    @Value("${user.ymlName}")
    private String name1;
//    读取数组中数据
    @Value("${liks[1]}")
    private String likes1;

//    使用自动装配封装到一个对象Environment中
    @Autowired
    private Environment env;
```

其中我们使用Environment对象封装整个yml，在使用的用法是：`System.out.println(env.getProperty("user.ymlName"));`这就能打印出来yml中user.ymlName的值获取出来。

- yml中的数据可以相互应用

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



- 使用实体类来获取yml中的数据

  - 前提：实体类中的属性名需要和yml的属性名一致
  - 这样才能把我们的yml中的数据封装到实体类
  - 需要把实体类上配置到bean上（否则无法进行属性注入）

  ```java
  //定义数据模型封装yml文件中对应的数据
  //定义为spring控制的bean
  @Component
  //指定加载的数据 prefix = "datasource1"需要小写
  @ConfigurationProperties(prefix = "datasource1")
  ```

```java
@Autowired
private MyDataSource myDataSource;
    @GetMapping("/book")
    public String getById(){       
        System.out.println(myDataSource.toString());
    }
```

在我们访问完/book后会在控制台上显示：

```MyDataSource(drive=com.mysql.cj.jdbc.Driver, url=jdbc:mysql://localhost:3306/mybatisdemo?serverTimezone=UTC, userName=root, password=123456)```

当我们在进行封装实体类时，使用lombok可以简化实体类的编写，（看个人习惯去使用lombok）

```xml
<!--       简化bean代码的工具包-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
            <version>1.18.4</version>
        </dependency>
```



编写实体类时：

```java
//使用@Data可以帮助我们完成getset以及toString的方法
@Data
//可以帮我们完成构造无参构造的方法
@NoArgsConstructor
//完成有参构造方法的注入
@AllArgsConstructor

//可以把需要操作的数据库中的表名注入到Book中
// 避免了实体类的名称和数据库的表名不匹配
@TableName(value = "book")
public class Book {
    private Long id;
    private String sn;
    private String name;
    private String author;
    private BigDecimal price;
    //    @TableField的使用可以规定定义的数据和数据库中的
    //    数据一致
    @TableField(value = "dirId")
    private Long dirId;

}
```

