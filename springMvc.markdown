- [Spring-Mvc 笔记](#spring-mvc-笔记)
- [跨域](#跨域)
- [SpringMvc**获取参数**](#springmvc获取参数)
  - [获取**基本参数**类型](#获取基本参数类型)
  - [获取**POJO**类型](#获取pojo类型)
  - [获取**数组**类型](#获取数组类型)
  - [获取**集合**类型](#获取集合类型)
  - [**@RequestParam**参数绑定](#requestparam参数绑定)
  - [**Restful**风格参数](#restful风格参数)
  - [获得**请求头**](#获得请求头)
  - [获得**Cookie**](#获得cookie)
- [SpringMvc**响应**](#springmvc响应)
  - [回显**数据**](#回显数据)
  - [@restController](#restcontroller)
- [上传**文件**](#上传文件)
  - [文件上传的**3要素**](#文件上传的3要素)
  - [依赖导入](#依赖导入)
  - [依赖注入`文件上传解析器`](#依赖注入文件上传解析器)
  - [`html`代码](#html代码)
  - [`java`代码](#java代码)
  - [**多文件**上传](#多文件上传)
    - [`html`](#html)
    - [`java`](#java)
- [**拦截器** `interceptor`](#拦截器-interceptor)
  - [配置拦截器](#配置拦截器)
  - [实现`HandlerInterceptor`接口](#实现handlerinterceptor接口)
- [`AOP`](#aop)
  - [依赖](#依赖)
  - [**`Xml`** 方式](#xml-方式)
    - [目标类 和 目标接口](#目标类-和-目标接口)
    - [切面类](#切面类)
    - [配置](#配置)
    - [切点表达式](#切点表达式)
    - [通知类型](#通知类型)
    - [切点表达式的 **`抽取`**](#切点表达式的-抽取)
  - [**`注解`** 方式](#注解-方式)
    - [目标类 和 目标接口](#目标类-和-目标接口-1)
    - [切面类](#切面类-1)
    - [通知](#通知)
    - [配置组件扫描](#配置组件扫描)
    - [AOP自动代理](#aop自动代理)
- [**SpringMvc.xml**中配置<br/>](#springmvcxml中配置)
  - [命名空间](#命名空间)
  - [拦截器](#拦截器)
  - [组件扫描](#组件扫描)
  - [注解驱动](#注解驱动)
  - [静态资源权限开放](#静态资源权限开放)
  - [文件上传依赖注入`文件上传解析器`](#文件上传依赖注入文件上传解析器)
- [**pom.xml**坐标](#pomxml坐标)
  - [数据库依赖](#数据库依赖)
  - [`SpringMvc`](#springmvc)
  - [测试](#测试)
  - [项目共用](#项目共用)
  - [`servlet` 本地CLASSPATH设置了可以不用](#servlet-本地classpath设置了可以不用)
  - [页面标签](#页面标签)
  - [`Json`数据解析 jackson](#json数据解析-jackson)
  - [文件上传`fileupload`](#文件上传fileupload)
- [**web.xml**配置](#webxml配置)
- [补充](#补充)
  - [文件下载](#文件下载)
# Spring-Mvc 笔记
# 跨域
> **@Controller** 类前面 或者业务方法前面 加上 **`@CrossOrigin`** 就能实现后端 *CORS*跨域  但是一般情况 跨域都是由前端解决的
# SpringMvc**获取参数**
## 获取**基本参数**类型
> **@Controller** 类中的 **业务方法** 参数必须名必须和URL中的参数名一致<br/> 
```java
    /* stringMvc 可以帮我们做基础类型的转换 比如String转Int   Int转 String */
    /* url = http://localhost:8080/er?name=1&id=12 */
    void save(String name,int id){

    }
```
## 获取**POJO**类型
> **@Controller** 类中的 **业务方法** 参数是一个对象 对象的属性和POJO的属性名一致  或者和参数名一致
```js
/* get请求 参数 和对象属性一致 */
var url = "localhost:8080/test/save?name=张三&age=18&sex=男"
```
```json
/* 发送过去的数据 为json数据 双方属性名一致 */
{
    name:"张三",
    age:18,
    sex:"男"
}
```
```java
/* 接收数据 */
class Obj{
    String name;
    String sex;
    int age;
    /* getter/setter... */
}
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save")
    void save(@RequestBody Obj name){
        
    }
}
```
## 获取**数组**类型
> **@Controller** 类中的业务方法数组名称与请求参数重复名称一致
```js
var url = "localhost:8080/test/save?name=张三&name=李四"
```
```java
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save")
    void save(String[] name){
        
    }
}
```
## 获取**集合**类型
> 首先得让获取集合参数需要将参数封装到POJO中<br/>
> **@Controller** 类中的业务方法数组名称与请求参数重复名称一致
```json
/* 发送过去的数据 为json数据 双方属性名一致 */
[{
    name:"张三",
    age:18,
    sex:"男"
},{
    name:"李四",
    age:19,
    sex:"男同"
}]
```
```java
/* 接收数据 */
class Obj{
    String name;
    String sex;
    int age;
    /* getter/setter... */
}
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save")
    void save(@RequestBody List<Obj> obj){
        
    }
}
```

## **@RequestParam**参数绑定
> **@Controller** 类中的业务方法参数 和请求中参数名字不一致时
```js
var url = "localhost:8080/test/save?name=张三"
```
> **@ResquestParam(参数)**<br/>
> value  请求中参数名<br/>
> required  参数是否必须<br/>
> defalutValue 当请求中没有指定参数时的默认值
```java
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save")
    void save(@requestParam("name") String userName){
        
    }
}
```
## **Restful**风格参数
> url /user/1?name=张三 中 1 就是参数 使用springMvc占位符绑定
```js
var url = "localhost:8080/test/save/1?name=张三"
```
> **@PathVarible(参数)**<br/>
> value  请求路径占位符名<br/>
> required  参数是否必须<br/>
```java
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save/{name}")
    void save(@PathVarible(value = "name" ,required = false) String name){
        
    }
}
```
## 获得**请求头**
> *@RequestHeader* 用来获取 请求头
```js
添加头 name
```
> **@RequestHeader(参数)**<br/>
> value  请求头名称<br/>
> required  是否必须携带这个请求头<br/>
```java
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save/{name}")
    void save(@RequestHeader(value = "name" ,required = false) String header){
        
    }
}
```
## 获得**Cookie**
> *@CookieValue* 用来获取 浏览器的Cookie

> **@CookieValue(参数)**<br/>
> value  Cookie名称<br/>
> required  是否必须携带这个Cookie<br/>
```java
@Controller
@RequestMapping(value = "/test")
public class Controller{
    @RequestMapping(value = "/save/{name}")
    void save(@RequestHeader(value = "name" ,required = false) String header){
        
    }
}
```
# SpringMvc**响应**
## 回显**数据**

```java
/* 控制层 用在类上面 把类交给spring容器 */
@Controller
/* value 请求路径 */
@RequestMapping(value = "/test" )
public class Controller{
    /* 方法上 加上 @ResponseBody 不在返回视图模型  可以用于返回json数据*/
    @RequestMapping(value = "/save")
    @ResponseBody
    String save(){
        return "hello world";
    }
}

```

> 如果需要返回Json数据 需要在xml中配置Json解析器 推荐使用 [SpringMvc注解驱动](#注解驱动)
## @restController
> 用于替代Controller 和 responseBody 是这两个注解的结合
# 上传**文件**
## 文件上传的**3要素**
- 表单的类型 `type = "flie"`
- 表单的提交必须使用`POST`方式
- 表单的`enctype`属性是多部分表单形式，`enctype="multipart/from-data"`

## 依赖导入
> 点击查看[依赖导入](#文件上传fileupload)
## 依赖注入`文件上传解析器`
> 点击查看[依赖注入 文件上传解析器](#文件上传依赖注入文件上传解析器)
## `html`代码
```html
<!-- 前端表单的input 标签的input 标签 name属性必须和后端 业务方法的参数名一致 -->
    <form action="http://localhost:8080/test/save3" method="POST" enctype="multipart/form-data">
        名称:<input type="text" name="name"/><br/>
        文件:<input type="file" name="upFile"/><br/>
        <button type="submit" value="提交"></button>
    </form>
```
## `java`代码
```java
@Controller
@RequestMapping(value = "/test" )
public class Controller {
    @RequestMapping("/save3")
    @ResponseBody
    public void save3(String name, MultipartFile upFile){
        upFile.transferTo(new File("路径"+"文件名"+".后缀"));
    }
}
```
## **多文件**上传
### `html`
```html
<!-- 前端表单的input 标签的input 标签 name属性必须和后端 业务方法的参数名一致 -->
    <form action="http://localhost:8080/test/save3" method="POST" enctype="multipart/form-data">
        名称:<input type="text" name="name"><br/>
        文件:<input type="file" name="upFile" multiple><br/>
        <button type="submit" value="提交"></button>
    </form>
```
### `java`
```java
@Controller
@RequestMapping(value = "/test" )
public class Controller {
    @RequestMapping("/save3")
    @ResponseBody
    public void save3(String name, MultipartFile[] upFile){
         for (MultipartFile file : upFile) {
            file.transferTo(new File("路径"+"文件名"+".后缀"));
        }
    }
}
```
# **拦截器** `interceptor`
> 拦截器类似于`过滤器` 用于对数据的`预处理`和`后处理`<br/>
> 拦截器可以用一定的顺序联成一条链 称为`拦截器链`
## 配置拦截器
> [拦截器配置](#拦截器)
## 实现`HandlerInterceptor`接口
```java
public class MyInterceptor implements HandlerInterceptor {
    /* 访问资源之前 */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }
    /* 资源返回之前 */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }
    /* 资源返回之后 */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```
# `AOP`
## 依赖
> [aspectjweaver](##SpringMvc)
## **`Xml`** 方式
### 目标类 和 目标接口
```java
class Target implements TargetInterface{

    @Override
    public void save() {
        System.out.println("save run...");
    }
}

interface TargetInterface{
    void save();
}
```
### 切面类
```java
class MaAspect{
    public void before(){
        System.out.println("前置");
    }
}
```
### 配置
```xml
<!-- 目标对象 -->
<bean id="target" class="全限定名"></bean>
<!-- 切面对象 -->
<bean id="aspect" class="全限定名"></bean>
<!-- 配置 -->
<aop:config>
<!-- 声明切面 -->
    <aop:aspect ref="aspect">
        <!-- 切面对象  切点 -->
        <aop:before method="before" pointcut="execution(返回类型 包名.类名.方法名)">
        </aop:before>
    </aop:aspect>
</aop:config>
```
### 切点表达式
> execution([修饰符]返回值类型包名.类名.方法名(参数))<br/>
> 访问修饰符 可以省略<br/>
> `返回类型` `包名` `类名` `方法名` 可以使用`*` 代替<br/>
> 如果包名后面是.. 表示当前包以及子类<br/>
> 参数列表可以使用.. 代替 表示所有参数类型
### 通知类型
> <aop:通知类型 method="通知方法" pointcut="切点表达式" ></aop:before> 

> 前置 目标方法之前执行
```xml
<aop:before>
```
> 后置 目标方法之后执行
```xml
<aop:after-returning>
```
> 环绕 目标方法前后都执行
```xml
<aop:around>
```
> 异常 抛出异常执行
```xml
<aop:throwing>
```
> 最终 不管怎样最后都执行
```xml
<aop:throwing>
```
### 切点表达式的 **`抽取`**
> <aop:poincut id="my" expression="切点表达式"><br/>

使用
> <aop:通知类型 method="通知方法" pointcut="id" ></aop:before>
## **`注解`** 方式
### 目标类 和 目标接口
> 目标交给Spring容器 @Component
```java
@Component
class Target implements TargetInterface{

    @Override
    public void save() {
        System.out.println("save run...");
    }
}

interface TargetInterface{
    void save();
}
```
### 切面类
> 交给Spring 容器 <br/>
> 加上@Aspect注解
```java
@Component
@Aspect
class MaAspect{
    
}
```
### 通知
> 方法前加上`@通知类型`注解 <br/>
> **`@通知类型("切点表达式")`**

前置
```java
@Before("切点")
public void func(){
        System.out.println("前置");
    }
```
后置
```java
@AfterReturning("切点")
    public void func(){
        System.out.println("后置");
    }
```
环绕
```java
@Around("切点")
    public void func(){
        System.out.println("环绕");
    }
```
异常
```java
@AfterThrowing(切点"")
    public void func(){
            System.out.println("异常");
        }
```
最终
```java
@After(切点"")
    public void func(){
            System.out.println("最后");
        }
```
### 配置组件扫描
> 扫描目标类 和 切面类
> <br/>[配置](#组件扫描)
### AOP自动代理
>配置到 Spring.xml 文件当中
```xml
<apo:aspectj-autoproxy/>
```
# **SpringMvc.xml**中配置<br/>
## 命名空间
```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    </beans>
```
## 拦截器
```xml
<!--拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
        <!-- /** 表示所有的请求路径都会被拦截-->
            <mvc:mapping path="/**"/>
            <bean class="包名.类名"/>
            <!-- 可以使用 <mvc:exclude-mapping path=""/> 设置排除路径 -->
        </mvc:interceptor>
    </mvc:interceptors>
```
## 组件扫描
```xml
    <!--    组件扫描  扫描controller-->
    <context:component-scan base-package="包名"></context:component-scan>
```
## 注解驱动
```xml
    <!--    注解驱动 用于返回数据为 Json-->
    <mvc:annotation-driven></mvc:annotation-driven>
```
## 静态资源权限开放
```xml
    <!--    静态资源权限开放-->
    <!-- <mvc:resources mapping="/路径/文件"mapping="/js/**" location="/js/"> -->
    <mvc:default-servlet-handler/>
```
## 文件上传依赖注入`文件上传解析器`
```xml
    <!-- 文件上传解析器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 上传文件的总的最大值-->
        <property name="maxUploadSize" value="50000"/>
        <!--上传文件单个最大值-->
        <property name="maxUploadSizePerFile" value="50000"/>
        <!--上传文件的编码类型-->
        <property name="defaultEncoding" value="UtF-8"/>
    </bean>
```
# **pom.xml**坐标
## 数据库依赖
```xml
<!-- 数据库 -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.24</version>
    </dependency>
```
## `SpringMvc`
```xml
    <!-- spring -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.8.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.13</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.0.8.RELEASE</version>
    </dependency>
```
## 测试
```xml
    <!-- 测试相关 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>5.0.8.RELEASE</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13</version>
      <scope>test</scope>
    </dependency>
```
## 项目共用
```xml
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.12</version>
      <scope>provided</scope>
    </dependency>
```
## `servlet` 本地CLASSPATH设置了可以不用
```xml
    <!-- servlet -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>
```
## 页面标签
```xml
    <!-- 页面标签-->
    <dependency>
      <groupId>org.apache.taglibs</groupId>
      <artifactId>taglibs-standard-spec</artifactId>
      <version>1.2.5</version>
    </dependency>
    <dependency>
      <groupId>org.apache.taglibs</groupId>
      <artifactId>taglibs-standard-impl</artifactId>
      <version>1.2.5</version>
    </dependency>
```
## `Json`数据解析 jackson
```xml
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.8</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.8</version>
    </dependency>
```
## 文件上传`fileupload`
```xml
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.6</version>
    </dependency>
    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.2</version>
    </dependency>
```
# **web.xml**配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4"
         xmlns="http://java.sun.com/xml/ns/j2ee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
         <!-- 监听器 -->
<!--    <listener>-->
<!--        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>-->
<!--    </listener>-->

    <!-- 过滤器 -->
    <filter>
        <filter-name>springFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 设置字符编码  从前端到后端的编码 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>springFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    <!-- 使用SpringMvc给我们提供的Servlet -->
    <servlet>
        <servlet-name>springMvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
        
```

# 补充
> `SpringMVC`支持`ant`风格的路径<br/>
> `?`: 表示任意的`单个字符`<br/>
> `*`: 表示任意的`0个或多个字符`<br/>
> `**`: 表示任意的`一层或多层目录`<br/>

> `@RequestMapping`的`headers`属性   
> `@RequestMapping`注解的`headers`属性通过请求的请求头信息匹配请求映射<br/>
> `@RequestMapping`注解的`headers`属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系<br/>
> `"header"`：要求请求映射所匹配的请求必须携带header请求头信息<br/>
> `"!header"`：要求请求映射所匹配的请求必须不能携带header请求头信息<br/>
> `"header=value"`：要求请求映射所匹配的请求必须携带header请求头信息且header=value<br/>
> `"header!=value"`：要求请求映射所匹配的请求必须携带`header`请求头信息且`header!=value`<br/>
> 若当前请求满足`@RequestMapping`注解的`value`和`method`属性，但是不满足headers属性，此时页面显示404错误，即资源未找到 <br/>
## 文件下载
```md
<!-- 返回加上 Content-Disposition 响应头 让浏览器下载东西的时候加上默认的名字 -->
Content-Disposition  attachment;filename=filename.xlsx

<!-- 当响应类型为application/octet-stream情况下使用上面的头信息的话，那么就不能直接显示内容，而是弹出一个"文件下载"的对话框，接下来就是用户决定“打开”还是“保存”了。 -->
```