## SpringMVC

​	![image-20220406102131080](C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220406102131080.png)

>客户端发来请求，服务端接收请求，执行逻辑并进行视图的跳转。而通常情况下我们每使用一次servlet层，都会有相同的共有行为，后去使用里面的相应逻辑。使用SpringMVC可以把共有行为使用框架的方式进行取出，这样就只用编写每个servlet的特有行为

#### 开发步骤：

1、导入相应的坐标(这此导入的是spring-mvc的坐标后续还需其它坐标)

```xml
<!--        spring-mvc 坐标-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
```

2、配置SpringMVC核心控制器DispatcherServlet（前端控制器）在web.xml中

```xml
<!--  配置全局过滤器filter 去解决post请求时乱码问题-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

<!--配置前端控制器-->
  <servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!-- 进行读取spring-mvx.xml的核心配置文件-->
    <init-param>
      <param-name>contextConfigLocation </param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
     <!-- 我们访问的路径名称，这样我们访问到是http://localhost:8080 -->
    <url-pattern>/</url-pattern>
  </servlet-mapping>

<!--全局初始化参数-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
<!--    这里报红是在帮我们进行检测，而不是报错-->
  </context-param>

<!--使用spring-web-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
```

3、创建Controller类 具体的业务和视图转换

​	创建的Controller这包下创建的类就是如同我们之前创建的servlet层一样负责直接的业务流程控制，并能进行相应的视图转换（重定向）

4、使用注解配置Controller类中的业务方法的映射地址

```java
//使用注解把它注入到Spring-mvc.xml中
@Controller
public class UserController {
//    @RequestMapping请求映射
//    请求地址 http://localhost:8080/quick
//    若是在类外面使用@RequestMapping，那么这里的请求地址
//    中会有类外的地址名，同时return还需要"/success.jsp"或"success"
    @RequestMapping("/quick")
//    @responseBody注解的作用是将controller的方法返回的
//    对象通过适当的转换器转换为指定的格式之后，写入到response
//    对象的body区，通常用来返回JSON数据或者是XML
//    @ResponseBody
    public String save(){
        System.out.println("管理层...");
        return "success";
    }
 }
```

> 当我们进行页面跳转或者进行重定向时使用 @ResponseBody 这并不能帮我们直接跳转或重定向
>
> 认识把我们return的对象通过转换器变成指定的格式后写入response中，所以当我们需要进行页面跳转时，不需在方法体外面使用@ResponseBody这个注解

**@RequestMapping作用：用于建立请求url和处理请求方法之间的对应关系 **

![image-20220406102203339](C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220406102203339.png)

5、配置SpringMVC核心文件spring-mvc.xml

```xml
 <!--1、mvc注解驱动 也可以解决JSON数据与对象的转换，
    这样就能代替配置处理映射器-->
    <mvc:annotation-driven conversion-service="conversion"/>

    <!--2、配置视图解析器（内部资源视图解析器）-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--    读取路径资源的前缀    -->
        <property name="prefix" value="/"/>
        <!--    读取路径资源的后缀    -->
        <property name="suffix" value=".jsp"/>
        <!--        这样做的目的是为了方面我们在controller下中的页面转发或重定向-->
    </bean>

<!--    配置处理映射器-->
<!--  使用json的转换工具将对象转换成json格式字符串在返回
      ObjectMapper objectMapper = new ObjectMapper();
      String string = objectMapper.writeValueAsString(user);
      下的就为了处理json转换的配置映射器-->
<!--    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">-->
<!--        <property name="messageConverters">-->
<!--            <list>-->
<!--                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>-->
<!--            </list>-->
<!--        </property>-->
<!--    </bean>-->



    <!--4、组件扫描  扫描Controller页面管理器-->
    <context:component-scan base-package="com.qinfeng.controller"/>
<!--配置文本解析器 可以帮助我们在上传文本后，文本格式不会乱码-->
    <bean id="commonsMultipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
<!--        文本的编译类型UTF-8-->
        <property name="defaultEncoding" value="UTF-8"/>
<!--        上传文件总大小-->
        <property name="maxUploadSize" value="50000"/>
    </bean>

<!--   开放资源的访问 -->
    <!--3、静态资源权限开放-->
    <mvc:default-servlet-handler/>
<!--    <mvc:resources mapping="/js/**" location="/js/"/>-->
<!--    <mvc:default-servlet-handler/>-->

<!--    自定义转换器注入spring-mvc容器中，并在1中添加-->
    <bean id="conversion" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <list>
                <bean class="com.qinfeng.converter.DateConverter"></bean>
            </list>
        </property>
    </bean>
```

6、客服端发起请求

`http://localhost:8080/quick` 这样就会从Controller中寻找quick，并执行下面的方法

