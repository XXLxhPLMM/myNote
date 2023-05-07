## SpringBoot的创建

它的创建有几种方式：

第一种是使用Idea进行新建项目

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407193026404.png" alt="image-20220407193026404" style="zoom: 50%;" />

​	在新建项目的面板中进行选择：Spring Initializr 点击下一步

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407193116837.png" alt="image-20220407193116837" style="zoom:50%;" />

​	其中在选在jdk的版本时需要注意下一步的版本选择，需要能够使用兼容，才能进行使用。

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407193550911.png" alt="image-20220407193550911" style="zoom:50%;" />*

​	直接就进行选择需要构建的项目，构建web项目

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407193835165.png" alt="image-20220407193835165" style="zoom:50%;" />

​	然后下一步直接点击完成就可以了。

​	最后页面的展示的项目结构如下：

<img src="C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407194010774.png" alt="image-20220407194010774" style="zoom:50%;" />

​	第二种方式创建项目：

​		我们可以直接去到Spring的官网中，在Projects中找到SpringBoot，点击进去后一直向下翻，可以找到一个Spring Initializer点击进去就好用Idea创建项目一样了，但是在网站上建好了会打成包帮我们下载下来，我们在自己的电脑上进行解压后，可以把项目用Idea打开。

​	第三种方法创建项目：

​	这个需要我们不用使用国外的网址进行创建springBoot项目，

![image-20220407194651028](C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220407194651028.png)

使用 http://start.aliyun.com 进行创建项目，其步骤也是一样的。

​	第四方法手动创建springBoot

​		这个方法就是我们需要创建一个maven工程项目（可以选择创建web，但是也是maven项目），创建好后，我们需要在pom.xml中去下加载springBoot的jar包，可以从之前的项目中去复制，也可以去官网复制。其中我们自需复制以下坐标

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.6</version>
</parent>

 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
<!--   下面是做测试用的，可以选择复制-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
</dependencies>
```

​	坐标加载完后，我们还需在java源码包下创建一个的Application类（类名自拟也可以），类中写上下面内容：

```java
@SpringBootApplication

public class Application {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(Application.class, args);
       
    }

}
```

​	做完这些，我们还需要一个Controller包，在里面写一个小功能就完成。

