## SpringBoot的热部署

### 	1.热部署

​	**热部署**：在我们开发过程中，运行项目的同时有去修改项目里面的内容，这样使用热部署就不需要再次的在启动项目。

在使用热部署前我们需要导入相对应的工具的坐标：

```xml
<!--        使用热部署工具坐标-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
```

这样我们在启动项目后，需要去激活热部署，使用快捷键**ctrl+F9**或者是在：

![image-20220416201626738](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162016773.png)

点击构建项目。

​	关于热部署：

- 重启：自定义开发代码，包含类，页面，配置文件等，加载位置restart类加载器
- 重载：jar包，加载位置base类加载器

>注意：我们使用构建项目和使用<img src="https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162022332.png" alt="image-20220416202230303" style="zoom: 50%;" />他俩没有区别，但是他们两个是根本上是，启动的东西是不一样的。



### 2.开启自动部署

​	首先做到设置，在设置里面找到如图：

![image-20220416203056925](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162030007.png)

在整完后，我们还需要进行开启自动热部署，使用快捷键 ctrl+alt+shift+/：

![image-20220416203243655](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162032674.png)

进入注册里面选择：

![image-20220416203349353](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162033410.png)

这样当完成这些操作后，在修改完后我们需要把鼠标的焦点离开idea5秒后自动进行部署。



### 3.热部署范围配置

​	热部署不触发启动热部署的目录列表

![image-20220416203830847](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204162038882.png)

也可以使用自定义的方式来进行不去进行自动启动热部署：

```yml
spring:
  devtools:
    restart:
    # 不参与热部署的文件夹或文件
      exclude: static/**,templates/**
```

关闭热部署：

```yml
  devtools:
    restart:
    # 不参与热部署的文件夹或文件
      exclude: static/**,templates/**
      # 关闭热部署
      enabled: false
```

也看以在我们的OperationApplication中写上：

```java
@SpringBootApplication
public class OperationApplication {

    public static void main(String[] args) {
        // 关闭热部署
        System.setProperty("spring.devtools.restart.enabled","false");
        SpringApplication.run(OperationApplication.class, args);
    }
}
```

