## SpringBoot小功能以及小技巧

### 1、隐藏文件或文件夹

​	我们在**文件**的位置找到**编辑器**，在**编辑器**下面有个**文件类型**，在这个板块下有一下内容，

<img src="https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151639719.png" alt="image-20220415163907630" style="zoom:67%;" />

### 2、parent

#### 	（1）springBoot简介

​	它是pivotal团队提供的全新框架，其目的是为了简化spring应用的初始搭建以及开发过程

- spring的缺点

  - 依赖设置繁琐

  - 配置繁琐

    

- springBoot的优点

  - 起步依赖（简化依赖配置）
  - 自动配置（简化常用工程相关配置）
  - 辅助功能（内置服务器...）

  #### （2）parent的用处

  （1）开发springBoot程序要继承spring-boot-starter-parent

  （2）在继承中定义了若干个依赖管理，便于统一开发

  （3）继承parent模块可以避免多个依赖使用相同技术出现依赖版本冲突

  （4）继承parent的形式可以采用引入依赖的形式实现效果

### 3、starter

​	`spring-boot-starter-web`定义当前项目使用的所有依赖坐标，以到达**减少依赖配置的目的**

​	而上面的parent是所有的SB（SpringBoot）项目要继承的，它是定义了若干个坐标版本号（依赖管理，而不是依赖），**以达到减少依赖冲突的目的**

​	在实际开发中，要是发现坐标错误，在指定Version（要小心版本冲突）

### 4、引导类

​	SB的引导类是boot工程的执行入口，运行main方法就可以启动项目；

​	SB工程运行后初始化spring容器，扫描引导类所在包加载bean

启动方式：

```java
@SpringBootApplication
public class Demo1Application {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(Demo1Application.class, args);
    }

}
```

### 5、内嵌Tomcat

​	内嵌Tomcat服务器是SB辅助功能之一，它的工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理，而需要变更内嵌服务器思想是去除现有服务器，添加全新的服务器。

### 6、rest风格

<img src="https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151639934.png" alt="image-20220415163940870" style="zoom:50%;" />

还有一个@RestController是等同于@Controller和@ResponseBody的组合功能

<img src="https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204151640173.png" alt="image-20220415164003114" style="zoom:50%;" />

### 7、复制模块

​	1.在工作空间（文件夹）中复制对应的工程，并修改工程名称

​	2.删除与idea相关的配置，只保留src目录与pom.xml文件

​	3.修改pom.xml文件中的artifactId与新工程/模块名称相同

​	4.删除name标签（可选）

​	5.保留备份工程供后期使用