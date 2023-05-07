## Spring+SpringMVC+Mybatis的整合

#### Mybatis与web

​	1、在我们之前学习mybatis中，我们是先根据实体类后，创建一个mapper包（它想我们之前dao包下的内容）在包中建一个AccountMapper接口，然后还需要在resources包下创建一个路径和AccountMapper接口一样的包下写一个AccountMapper.xml，在xml中写对数据的操作。

AccountMapper.java的接口

```java
public interface AccountMapper {

    void save(Account account);

    List<Account> findAll();

}
```

AccountMapper.xml文件

```xml
<mapper namespace="com.itheima.mapper.AccountMapper">
    <insert id="save" parameterType="account">
        insert into account values(#{id},#{name},#{money})
    </insert>
    <select id="findAll" resultType="account">
        select * from account
    </select>
</mapper>
```

> 其中我们可以看到接口中定义的方法需要在xml中进行实现，但是我们要注意的是我们在接口中的方法名需要和xml中id名称要一致，这样才能使我们写的操作数据库在接口中的方法实现

2、而建好我们并不能成功的实现对数据库操作，我们还需要在resources资源包下创建一个使用mybatis的资源文件sqlMapConfig.xml中去进行配置

```xml
<!--加载properties文件-->
<properties resource="jdbc.properties"></properties>

<!--定义别名-->
<typeAliases>
    <!--<typeAlias type="com.itheima.domain.Account" alias="account"></typeAlias>-->
    <package name="com.itheima.domain"></package>
</typeAliases>

<!--环境-->
<environments default="developement">
    <environment id="developement">
        <transactionManager type="JDBC"></transactionManager>
        <dataSource type="POOLED">
            <property name="driver" value="${jdbc.driver}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="username" value="${jdbc.username}"></property>
            <property name="password" value="${jdbc.password}"></property>
        </dataSource>
    </environment>
</environments>

<!--加载映射-->
<mappers>
    <!--<mapper resource="com/itheima/mapper/AccountMapper.xml"></mapper>-->
    <package name="com.itheima.mapper"></package>
</mappers>
```

​	3、再service层使用进行业务逻辑操作就能可以了

```java
@Override
public void save(Account account) {
    try {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
        mapper.save(account);
        sqlSession.commit();
        sqlSession.close();

    } catch (IOException e) {
        e.printStackTrace();
    }
    
}
```



#### Mybatis和SpringMvc的结合

​	在我们进行使用SpringMvc中我们就已经潜移默化的把spring和springMvc进行结合了，而在我们需要使用Mybatis时，还需要把他们结合起来。

​	而在我们进行两者的整合时步骤和上面还是相似的，但是mybatis的资源文件sqlMapConfig.xml中在我们spring核心配置文件有重复的可以去进行整合，整合后只需要在里面定义一个别名

```xml
<!--定义别名-->
<typeAliases>
    <!--<typeAlias type="com.itheima.domain.Account" alias="account"></typeAlias>-->
    <package name="com.itheima.domain"></package>
</typeAliases>
```

​	而我们的spring-mvc.xml配置文件是不用变化的。

```xml
<!-- spring-mvc.xml中的配置 -->
<!--1.组件扫描  主要扫描controller-->
<context:component-scan base-package="com.itheima.controller"/>
<!--2.配置mvc注解驱动-->
<mvc:annotation-driven/>
<!--3.内部资源视图解析器-->
<bean id="resourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/pages/"/>
    <property name="suffix" value=".jsp"/>
</bean>
<!--4.开发静态资源访问权限-->
<mvc:default-servlet-handler/>
```

​	而在我们的spring核心配置文件中需要在去加载配置我们的mybatis资源

```xml
<!--组件扫描 扫描service和mapper-->
<context:component-scan base-package="com.itheima">
    <!--排除controller的扫描-->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

<!--加载propeties文件-->
<context:property-placeholder location="classpath:jdbc.properties"/>

<!--配置数据源信息-->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driver}"/>
    <property name="jdbcUrl" value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>

<!--配置sessionFactory-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--加载mybatis核心文件-->
    <property name="configLocation" value="classpath:sqlMapConfig-spring.xml"/>
</bean>

<!--扫描mapper所在的包 为mapper创建实现类-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.itheima.mapper"/>
</bean>


<!--声明式事务控制-->
<!--平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--配置事务增强-->
<tx:advice id="txAdvice">
    <tx:attributes>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<!--事务的aop织入-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:advisor>
</aop:config>
```

而在整合后，我们service层就无需进行繁琐的操作了下面的步骤将会被配置文件的配置取代

```java
InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
```

在配置文件配置：

```xml
<!--配置sessionFactory-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <!--加载mybatis核心文件-->
    <property name="configLocation" value="classpath:sqlMapConfig-spring.xml"/>
</bean>
```

service中对事务处理：

```java
sqlSession.commit();
sqlSession.close();
```

在配置文件进行配置：

```xml
<!--声明式事务控制-->
<!--平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--配置事务增强-->
<tx:advice id="txAdvice">
    <tx:attributes>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<!--事务的aop织入-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:advisor>
</aop:config>
```

而在结合后的service成

```java
@Autowired
private AccountMapper accountMapper;

@Override
public void save(Account account) {
    accountMapper.save(account);
}
```

> 通过最后整合完成，我发现主要是在配置文件进行配置，结合着注解，使我们开发逐步简化，并且耦合程度也在逐渐降低。

