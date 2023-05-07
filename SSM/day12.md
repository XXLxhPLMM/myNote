## spring-mvc

1. #### 创建springmvc项目

   步骤一：新建maven项目webapp

   ![1649317449944](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649317449944.png)

   步骤二：完善项目结构

   ![1649317474824](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649317474824.png)

   步骤三：导入jar包

   ```xml
   <dependencies>
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.11</version>
       <scope>test</scope>
     </dependency>
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.3.18</version>
     </dependency>
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-beans</artifactId>
       <version>5.3.18</version>
     </dependency>
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.0.8.RELEASE</version>
     </dependency>
     <dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>javax.servlet-api</artifactId>
       <scope>provided</scope>
       <version>3.1.0</version>
     </dependency>
   
   
   </dependencies>
   ```

   步骤四：创建一个handler类，实现Controller接口

   ```java
   package com.wolfcode.handler;
   
   import org.springframework.web.servlet.ModelAndView;
   import org.springframework.web.servlet.mvc.Controller;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class FirstHandler implements Controller {
       @Override
       public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
           System.out.println("接受到请求");
           return null;
       }
   }
   ```

   步骤五：添加spring核心配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="/test1.do" class="com.wolfcode.handler.FirstHandler"></bean>
   </beans>
   ```

   步骤五：配置web.xml文件

   ```xml
   <servlet>
       <servlet-name>dispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   <!--    初始化 配置spring核心配置文件路径
   -->
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:applicationContext.xml</param-value>
       </init-param>
   <!--    Tomcat启动时就完成初始化操作
           里面填0~正整数，数值越小越先加载
           如果是负数就不起作用
   -->
       <load-on-startup>0</load-on-startup>
     </servlet>
   <!--  servlet映射-->
     <servlet-mapping>
       <servlet-name>dispatcherServlet</servlet-name>
         <!--    以.do路径可以通过servlet访问，否则不放行-->
       <url-pattern>*.do</url-pattern>
     </servlet-mapping>
   ```

   步骤五：添加Tomcat驱动

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.tomcat.maven</groupId>
         <artifactId>tomcat7-maven-plugin</artifactId>
         <version>2.2</version>
         <configuration>
           <path>/</path>
           <port>8080</port>
           <uriEncoding>UTF-8</uriEncoding>
         </configuration>
       </plugin>
     </plugins>
   </build>
   ```

   步骤六：运行Tomcat，输入地址（spring配置文件bean的id值）

   ![1649318088812](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649318088812.png)

   步骤七：运行成功

![1649318136142](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649318136142.png)

2. #### web.xml文件中优化Tomcat启动

   ```xml
   <servlet>
   	<load-on-startup>0</load-on-startup>
   </servlet>
   ```

   `<load-on-startup>0~正整数</load-on-startup>`：数值越小越先加载

   未添加：

   ![1649319298614](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649319298614.png)

   添加：

   ![1649319357515](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649319357515.png)

3. #### springmvc实现页面跳转的方式

   1. 转发

      特点：

      1. url地址栏不发生改变
      2. 发送数据时只发送一次

      **原始转发：**

      ```java
      public class FirstHandler implements Controller {
          @Override
          public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
              System.out.println("接受到请求");
              req.setAttribute("key1","test1");
              req.getRequestDispatcher("jsp/test1.jsp").forward(req,resp);
              return null;
          }
      }
      ```

      **ModelAndView转发：**

      ```java
      public class FirstHandler implements Controller {
          @Override
          public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
              System.out.println("接受到请求");
              ModelAndView mav = new ModelAndView();
              mav.addObject("key1","test1");
              mav.setViewName("jsp/test1.jsp");
              return mav;
          }
      }
      ```

      > 注意：
      >
      > mav.setViewName("jsp/test1.jsp");省略了forward
      >
      > 完整版：mav.setViewName("forward:jsp/test1.jsp");

   2. 重定向

      特点：

      1. url地址栏发生改变
      2. 发送数据时会发送两次

      **原始版本重定向：**

      ```java
      public class FirstHandler implements Controller {
          @Override
          public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
              System.out.println("接受到请求");
              req.getSession().setAttribute("key1","test1");
              resp.sendRedirect("jsp/test1.jsp");
              return null;
          }
      }
      ```

      **ModelAndView重定向：**

      ```java
          @Override
          public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
              System.out.println("接受到请求")
              ModelAndView mav = new ModelAndView();
              mav.addObject("key1","tes1");
              mav.setViewName("redirect:jsp/test1.jsp");
              return mav;
          }
      ```

      ![1649347069362](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649347069362.png)

      > 页面获取到了参数，但是需要通过前端的方法显示到页面，所以ModelAndView重定向需要配合老版本一起使用

      优化后：

      ```java
      @Override
          public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
              System.out.println("接受到请求");
              req.getSession().setAttribute("key1","test1");
              ModelAndView mav = new ModelAndView();
              mav.setViewName("redirect:jsp/test1.jsp");
              return mav;
          }
      ```

![1649347187344](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649347187344.png)

**扩展：到WEB-INF里面添加一个jsp文件，然后去实现转发和重定向**

1. 转发：

   ```java
   @Override
   public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
       System.out.println("进入到了处理器");
       ModelAndView modelAndView = new ModelAndView();
       modelAndView.addObject("key2","test2");
       modelAndView.setViewName("WEB-INF/test2.jsp");
       return modelAndView;
   }
   ```

   运行结果，说明可以实现转发

   ![1649434323484](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649434323484.png)

2. 重定向

   ```java
   @Override
   public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
       System.out.println("进入到了处理器");
       ModelAndView modelAndView = new ModelAndView();
       modelAndView.addObject("key2","test2");
       modelAndView.setViewName("redirect:WEB-INF/test2.jsp");
       return modelAndView;
   }
   ```

   运行结果返回404

   ![1649434426995](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649434426995.png)

   >原因：WEB-INF目录下默认是安全的，也就是不可直接访问的



**配置视图解析器**

目的：为了简化路径过长的问题

步骤一：到配置文件中配置一个bean对象

```xml
<!--    配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="WEB-INF/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
```

步骤二：修改原来的路径（按照上面配置只需要写文件名即可）

```java
@Override
public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
    System.out.println("进入到了处理器");
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("key2","test2");
    modelAndView.setViewName("test2");
    return modelAndView;
}
```

运行执行：

![1649434780138](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649434780138.png)

视图解析器好使

> 注意：配置了视图解析器后不可加，forward或者redirect关键字，因为会默认将它们添加到路径里面去，造成404问题

```java
    @Override
    public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        System.out.println("进入到了处理器");
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("key2","test2");
        modelAndView.setViewName("forward:test2");
        return modelAndView;
    }
}
```

运行结果：

![1649434888994](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1649434888994.png)

