## SpringBoot日志基础操作

### 1.添加日志记录操作

```java
@RestController
@RequestMapping("/test")
public class testController {
//   1.声明记录日志的对象
    private static final Logger log= LoggerFactory.getLogger(testController.class);

    @RequestMapping("/op1")
    public void test(){
        
//    2.根据你的日志级别记录日志 
        log.debug("debug...");
        log.info("info...");
        log.warn("warn...");
        log.error("error...");
        //    ----------------------
        System.out.println("...");
    }
}
```

- 日志级别
  - TRACE：运行堆栈信息，使用率低
  - DEBUG：调试代码使用
  - INFO：记录运维过程中的数据
  - WARN：记录运维过程报警提示数据
  - ERROR: 记录运维过程堆栈信息
  - FATAL: 灾难信息，合并记入ERROR中

> 用于记录开发调试与运维过程的信息 
>
> 通常使用4种即可：debug，info，warn，error

### 2.设置日志输出的级别

```yml
# 开启debug模式，输入调试信息，
#  常用于检查系统运行状况
debug: true
# 设置日记级别，root表示根节点，
#  即表示整体应用日志级别
logging:
  level:
    root: debug
```

### 3.设置日志组

![image-20220416153714184](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161537299.png)

> 可以通过日志组或代码包的形式进行日志显示级别的控制



---

## 快速创建日志对象

### 1.优化日志对象创建代码

- 使用**lombok**提供的注解@Slf4j简化开发，减少日志对象的声明操作

  - 导入坐标

  ```xml
  <!--       简化bean代码的工具包-->
  <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
      <version>1.18.4</version>
  </dependency>
  ```

  - 使用注解

  ```java
  @Slf4j
  @RestController
  @RequestMapping("/test")
  public class testController {
  //声明记录日志的对象
  //    private static final Logger log= LoggerFactory.getLogger(testController.class);
  
      @GetMapping
      public String test(){
  
  //      根据你的日志级别记录日志
          log.debug("debug...");
          log.info("info...");
          log.warn("warn...");
          log.error("error...");
          //    ----------------------
          System.out.println("年后...");
          return "显示数据咯";
      }
  }
  ```

> 这里有个错误是，我这里使用完注解，但是问题是，我的log无法给加载出来，而是报错。

### 2.日志输出格式控制

​	![image-20220416170301489](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161703545.png)

上图是我们的日志格式：

> **时间+级别+PID ···[所属线程]+所属类/接口名：日志信息**

- PID : 进程id，用于表明当前操作所处的进程，当前服务同时记录日志时，该值可用于协助我们进行调试
- 所属类/接口名：当前显示的信息为SpringBoot重写后的信息，名称过长时，简化包名为首字母，甚至直接删除

在配置文件进行设置：

```yml
#日志模板
logging:
  pattern:
    console: "%d %clr(%5p) --- [%16t] %clr(%-40.40c){cyan}: %m %n"
```

> %d：日期， %m：消息， %n：换行；
>
> `console: "%d %clr(%5p) --- [%16t] %clr(%-40.40c){cyan}: %m %n"` 
>
> 代表时间+（颜色）级别···[所属线程]+(颜色)所属类/接口名：日志信息



## 文件记录日志

```yml
#日志模板
logging:
  pattern:
    console: "%d %clr(%5p) --- [%16t] %clr(%-40.40c){cyan}: %m %n"
#  设置日志文件
  file:
    name: server.log
#  日志文件的详细配置
  logback:
    rollingpolicy:
#      日志文件大小
      max-file-size: 5KB
#      生成日志文件的名的格式
      file-name-pattern: server.%d{yyyy-MM-dd}.%i.log
```

![image-20220416173259100](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204161732158.png)

