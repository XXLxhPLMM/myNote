## 项目上线运维

### 1.临时属性设置

​	带属性数启动SpringBoot ----使用命令去运行打包好的项目，再去设置它的端口，

```java
java -jar xxxx.jar --server.port=80
```

​	携带多个属性启动SpringBoot，属性之间要使用空格分隔；同时当我使用属性来启动项目，我们又会考虑到我们之前不是配置相对应的属性了吗？

原因：是由于属性加载是有一个优先级顺序的：
![image-20220416110343306](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161103504.png)

同时临时属性必须是当前项目工程支持的属性，否则设置无效。





而在使用临时属性前，我们可以在idea中去测试属性是否可用，在编辑属性里面的程序参数里进行添加

![image-20220416111151848](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161111889.png)

![image-20220416111138527](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161111607.png)

我们在这里改完后，也可以把添加的临时属性给打印出来，这里就需要用到我们的配置：

```java
@SpringBootApplication
public class SsmpApplication {

    public static void main(String[] args) {
        SpringApplication.run(SsmpApplication.class, args);
    }

}
```

中的args（添加临时属性），我们可以把它进行打印`System.out.println(Arrays.toString(args));`我们就会在控制台上看到我们临时属性的配置被打印出来，同时属性也会被应用。

![image-20220416111755749](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161117815.png)

同时在去掉外部参数的形参，是为了我们的安全性。



### 2.配置文件4级分类

![image-20220416122400696](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161224788.png)

![image-20220416122449831](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161224917.png)

### 3.多环境配置

```yml
# 多环境开发，
# 使用那个公共配置
spring:
  profiles:
    active: pro

---
# 设置环境 生产环境
spring:
  profiles: pro

server:
  port: 80
---
# 开发环境
spring:
  profiles: dev

server:
  port: 81
---
# 测试环境
#spring:
#  profiles: test
spring:
  config:
    activate:
      on-profile: test

server:
  port: 82

```

上面是一种多环境配置的格式，其中yml格式中设置多环境使用  --- 进行区分环境设置的边界；

而这种多环境开发需要设置若干常用环境，例如开发，生产，测试环境，并且每种环境的区别在于加载的配置属性不同，这样就可以启用某种环境时需要指定启动使用该环境。

**上面格式的问题**

1. 若是在连接数据库时，每种环境连接的的配置都写到一个配置文件里面多少有点不安全。

解决方法：多环境开发多配置文件格式：

- yml

![image-20220416144717783](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161447877.png)

其中主配置文件中设置公共配置（全局）；环境分类配置文件中常用于设置冲突属性（局部）；

这样使用独立配置文件定义环境属性，并且便于上线系统维护更新和保障系统安全性

- properties

![image-20220416145228850](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161452896.png)

### 4.多环境分组管理

![image-20220416145934967](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161459068.png)

![image-20220416150019910](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161500009.png)

其中多环境开发使用group属性设置配置文件分组，便于线上的维护管理。



### 5.多环境的开发控制

​	**考虑问题**：在maven和spring中都有profile，要是他们俩个起冲突了，怎么办，到底谁生效？

那就要先明白SpringBoot是依赖maven运行的还是maven依赖SpringBoot运行的？

​	**解**：SpringBoot是依赖maven运行的，

处理方法，maven与SpringBoot多环境兼容

- 第一步在maven中去设置多环境属性

```xml
<!--    设置多环境-->
    <profiles>
        <profile>
            <id>env_dev</id>
            <properties>
                <profile.actice>dev</profile.actice>
            </properties>
<!--            默认启动dev的运行环境-->
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>env_pro</id>
            <properties>
                <profile.actice>pro</profile.actice>
            </properties>
        </profile>
    </profiles>
```

> 中的设置多环境的配置中，有很多**`<profile.actice>dev</profile.actice>`**是为了给yml进行使用的。

- 在SpringBoot中的配置文件中去引用maven的属性

```yml
spring:
  profiles:
    active: @profile.actice@
    group:
      "dev": devDB,devMVC
      "pro": devDB,devMVC
```

- 在执行maven打包指令，并在生成打包文件的jar包中查看对应的信息

> 但是这里小问题？ 原因是idea的缓存问题，带来的去启动其他的运行环境而没有生效。

**解决**：基于SpringBoot读取Maven配置属性的前提下，如果在Idea下测试工程时，pom.xml每次更新需要手动compile一下方可生效。

