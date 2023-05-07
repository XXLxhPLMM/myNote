## SpringBoot项目打包

### 	1.windows版



在完成项目后需要进打包上传服务器。

#### 1.打包步骤：

- 在idea的右侧找到maven 打开项目
- 在lifecycle中先进清除我们自己生成的target包，
- 在点击：![image-20220413150557787](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204131506902.png)

- 在使用package进行打包
- 在我们打包的位置进行打开cmd
- 在控制台上进行运行项目（执行启动指定） java -jar xxxxx.jar(在输入jar的前面的字段使用tab键可以自动补全)

> jar支持命令行启动需要依赖的maven插件支持，

> ```xml
> <build>
>  <plugins>
>      <plugin>
>          <groupId>org.springframework.boot</groupId>
>          <artifactId>spring-boot-maven-plugin</artifactId>
>      </plugin>
>      <!-- 我们要注意maven的版本，有时版本不同引起打包时出错 -->
>      <plugin>
>          <artifactId>maven-resources-plugin</artifactId>
>          <version>3.1.0</version>
>      </plugin>
>  </plugins>
> </build>
> ```

假如我在maven打版本下进行打包会报错：

下面是我的maven的版本号：

```  cmd
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: E:\MAVEN_life\apache-maven-3.6.3\bin\..
Java version: 1.8.0_161, vendor: Oracle Corporation, runtime: E:\JDK_life\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"```  
```

进行的报错信息：

```cmd
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-resources-plugin:3.2.0:resources (default-resources) on project demo1: Input length = 1 -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```



```cmd
[WARNING] The POM for com.alibaba:druid:jar:1.2.6 is invalid, transitive dependencies (if any) will not be available, enable debug logging for more details
The POM for com.alibaba:druid:jar:1.2.6 is invalid, transitive dependencies (if any) will not be available, enable debug logging for more details

```



```cmd
[INFO] 
[INFO] --- maven-resources-plugin:3.2.0:resources (default-resources) @ demo1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] Copying 4 resources

```

我们需要添加下面的一下配置：

```xml
<plugin>
    <artifactId>maven-resources-plugin</artifactId>
    <version>3.1.0</version>
</plugin>
```

#### 2.处理在windows端口被占用

​	当我们打包完成后，找到打包的位置，需要在cmd控制台上运行打包后的项目：`java -jar xxxx.jar`；其中的xxxx.jar就是我们打包后的项目，

![image-20220414141228220](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204141412380.png)

上面我们可以看到我们的端口8080被占用，这就需要我们去把8080端口关闭后在运行项目；

##### 关闭端口：

- 查询端口 `netstat -ano `可以查看全部的端口
- 查询指定端口 ` netstat -ano |findstr "8080(这里的8080是我们需要查找的端口号)`

![image-20220414141837491](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204141418527.png)

- 根据进程PID查询进程名称：`tasklist |findstr “7744（上图标记处）"`

![image-20220414142036437](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204141420479.png)

可以看到我们电脑中在使用这个端口的程序，

-  根据PID结束任务进程：`taskkill /F /PID "进程PID号"`

![image-20220414142303324](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204141423412.png)

回车后，将会提示进行已经结束

- 也可以根据进行名称结束任务进程 `taskkill -f -t -im "进程名称"`

![image-20220414142647129](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204141426163.png)

> 注意要是使用 `taskkill -f -t -im "进程名称"`这个可能把关于进程名称的全部都关闭了，
>
> 这样有时间会误关我们需要使用你的端口
>
> 主要使用`taskkill /F /PID "进程PID号"`因为进行PID是唯一的标识。
