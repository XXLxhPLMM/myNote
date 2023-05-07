## SpringMVC拦截器

##### 拦截器的作用

SpringMVC中的拦截器类似于之前学习的javaWeb中servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。

> 拦截器将会按一定的顺序结成一条链，称拦截器链（Interceptor Chain），在访问被拦截的方法或字段时，拦截器链中的拦截器会按照之前定义的顺序被调用。拦截器也是AOP思想的具体实现。

##### 拦截器与过滤器的区别

![image-20220406125252461](C:\Users\qinsi\AppData\Roaming\Typora\typora-user-images\image-20220406125252461.png)

##### 自定义拦截器的步骤

1、创建拦截器类实现HandlerInterceptor接口

​	而在接口中最重要的一个实现方法是**`preHandle`方法**，它是在目标方法执行前，就执行。其中它需要有返回值，false或true 。false是禁止通行被拦截下来不允许通过。true是允许资源通过

2、配置拦截器

配置拦截器，就是我们需要把刚刚创建的拦截器类的配置到spring-mvc.xml中。

```xml
<!--  5、配置拦截器  -->
    <mvc:interceptors>
        <!--第一个拦截器-->
        <mvc:interceptor>
<!--            配置对那些进行拦截-->
            <mvc:mapping path="/**"/>
<!--            配置那些资源不用进行拦截-->
            <mvc:exclude-mapping path="/user/login"/>
            <bean class="com.qinfeng.interceptor.PrivilegeInterceptor"/>
        </mvc:interceptor>
        <!--第二个拦截器-->
        <!--<mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/user/login"/>
            <bean class="com.qinfeng.interceptor.PrivilegeInterceptor"/>
        </mvc:interceptor>-->
    </mvc:interceptors>
```

在配置文件可以配置多个拦截器，但是他们有一个执行顺序，`preHandle`方法是在上面的先执行，其它方法`postHandle`和`afterComletion`是下面的拦截器先执行，（先进后出）

3、测试拦截器的拦截效果

