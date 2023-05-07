## SpringBoot测试专用属性

### 1.加载测试专用属性

1. 在我们使用测试用例时，有时会调用yml中的属性值，

```yml
test:
  prop: testing
```

我们在测试类中去使用@Value（"${test.prop}"）来调用yml中属性值，

```java
@SpringBootTest
class OperationApplicationTests {

    @Value("${test.prop}")
    private String msg;

    @Test
    void contextLoads() {
        System.out.println(msg);
    }

}
```

​	在运行测试用例后，会在控制台上打印给testing。

​	但是有是我们进行测试时，经常使用一些临时的属性，不会再yml中去进行编写，这样我们可以使用@SpringBootTest中的一个属性-----properties属性

```java
//properties属性可以为当前测试用例添加临时属性配置
@SpringBootTest(properties = {"test.prop=test"})
class OperationApplicationTests {

    @Value("${test.prop}")
    private String msg;

    @Test
    void contextLoads() {
        System.out.println(msg);
    }
}
```

这样在控制台上打印的出来的是test。

​	这样我们就能在测试用例中去使用一些自定义的属性。但是当我我们的yml也有这个属性时，在运行测试用例时，它会先加载yml文件，但是后面会在进行一次加载@SpringBootTest(properties = {"test.prop=test"})，去覆盖配置文件的属性，

2. 添加临时的命令行参数

   在命令行时，可以在项目配置中去添加，也可以在application类中的args中去添加而在测试用例中使用临时的命令行

   ```java
   //args属性可以为当前测试加临时的命令行参数
   @SpringBootTest(args = {"--test.prop=testArgs"})
   class OperationApplicationTests {
   
       @Value("${test.prop}")
       private String msg;
   
       @Test
       void contextLoads() {
           System.out.println(msg);
       }
   
   }
   ```

这样可以打印出临时命令行

> 加载测试临时属性应用与小范围测试环境



### 2.加载测试专用配置

​	在项目中我们会编写一些配置类，我们有时需要进行测试，可以在测试用下编写一个配置类：

```java
//指定当前类为一个spring配置类
@Configuration
public class MsgConfig {
    @Bean
    public String msg(){
        return "bean msg...";
    }
}
```

这样我们就在测试用例中编写一个简单的配置类，在是使用这个测试配置类：

```java
@SpringBootTest
//加载配置类
@Import({MsgConfig.class})
public class ConfigTest {
    @Autowired
    private String msg;

    @Test
    public void testConfig(){
        System.out.println(msg);
    }
}
```

进行测试后，控制台上可以进行打印出来 bean msg... 

其中在@Import({MsgConfig.class,...})可以加载多分配置类；这个测试用例不用加载也是可以实现的。

> 加载测试范围配置应用于小范围测试环境

### 3.web环境模拟测试

​	1.在测试用例进行web环境的测试：

```java
//DEFINED_PORT默认端口
//RANDOM_PORT 开启随机端口
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class WebTest {

    @Test
    void test(){

    }
}
```

其中在测试中有四个值：

![image-20220418145430149](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181454241.png)



> 其中建议使用随机端口进行打开，这样可以避免与其他的端口冲突



​	2.在测试用例进行虚拟mvc的调用：需要在测试类上加上一个注解@AutoConfigureMockMvc

```java
//DEFINED_PORT默认端口
//RANDOM_PORT 开启随机端口
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
//开启mvc的虚拟调用
@AutoConfigureMockMvc
public class WebTest {

    @Test
    void test(){
    }
//void testWeb(@Autowired MockMvc mvc)这样使用可以指定范围的使用
    @Test
    void testWeb(@Autowired MockMvc mvc) throws Exception {
//      从https://localhost/test
//      创建虚拟请求，当前访问/test
        MockHttpServletRequestBuilder builder= MockMvcRequestBuilders.get("/test");
//        执行对应的请求
        mvc.perform(builder);
    }
}
```



​	3.匹配响应执行状态

```java
    @Test
    void testStatus(@Autowired MockMvc mvc) throws Exception {
        MockHttpServletRequestBuilder builder= MockMvcRequestBuilders.get("/test");
        ResultActions action = mvc.perform(builder);
//设定预期值，与其页面进行比较，成功测试通过，失败测试失败
//        定义本次调用的预期值
        StatusResultMatchers status = MockMvcResultMatchers.status();
//        成功状态为200
        ResultMatcher ok = status.isOk();
//        添加预期值到本次调用过程中进行匹配
        action.andExpect(ok);
    }
```

若是成功的通过，就是正常进行输出；若是报错：

![image-20220418151639389](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181516420.png)

可以看到控制台上给出的提示。

​	4.匹配请求响应体

```java
    @Test
    void testBody(@Autowired MockMvc mvc) throws Exception {
        MockHttpServletRequestBuilder builder= MockMvcRequestBuilders.get("/test");
        ResultActions action = mvc.perform(builder);
//设定预期值，与其页面进行比较，成功测试通过，失败测试失败
//        定义本次调用的预期值
        ContentResultMatchers content = MockMvcResultMatchers.content();
//        进行匹配请求体中内容
        ResultMatcher resultMatcher = content.string("qwer");
//        添加预期值到本次调用过程中进行匹配
        action.andExpect(resultMatcher);
    }
```

![image-20220418152246424](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181522464.png)

这个表示匹配失败，原因是请求体中是显示数据咯，但是我们给的匹配内容是qwer。

​	5.匹配请求响应体（json）

```java
    @Test
    void testJson(@Autowired MockMvc mvc) throws Exception {
        MockHttpServletRequestBuilder builder= MockMvcRequestBuilders.get("/test");
        ResultActions action = mvc.perform(builder);
//设定预期值，与其页面进行比较，成功测试通过，失败测试失败
//        定义本次调用的预期值
        ContentResultMatchers content = MockMvcResultMatchers.content();
//        成功状态为200
        ResultMatcher resultMatcher = content.string("{\"id\":1}");
//        添加预期值到本次调用过程中进行匹配
        action.andExpect(resultMatcher);
    }
```

![image-20220418152731548](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181527603.png)

​	6.匹配请求响应头

```java
    @Test
    void testHeader(@Autowired MockMvc mvc) throws Exception {
        MockHttpServletRequestBuilder builder= MockMvcRequestBuilders.get("/test");
        ResultActions action = mvc.perform(builder);
//设定预期值，与其页面进行比较，成功测试通过，失败测试失败
//        定义本次调用的预期值
        HeaderResultMatchers header = MockMvcResultMatchers.header();
        ResultMatcher resultMatcher = header.string("Content=Type", "application/json");
//        添加预期值到本次调用过程中进行匹配
//        System.out.println(resultMatcher);
        action.andExpect(resultMatcher);
    }
```

![image-20220418153548136](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181535169.png)

​	7.在进行测试时，可以把上面的中的合并到一起

### 4.数据层测试回滚

​	在进行操作数据库数据的测试时，有又不想把测试时的数据填到数据库中去，可以在测试类上加上一个注解  @Transactional ，并在配合着@Rallback()（回滚）（）里面默认为true，表示回滚，这样就算测试成功也不会把数据填写到数据库中但是若是使用false ，成功也可以进行添加到数据库

### 5.测试用例数据设定

![image-20220418154830266](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204181548406.png)

