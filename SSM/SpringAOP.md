## SpringAOP

一、AOP思想

* aop意为面向切面编程，给业务增强相同功能，而不改变业务代码，在指定的切点（需要加入增强功能的业务类中的方法）处植入切面（增强功能）
* 底层原理以及实现：jdk动态代理和CGLib动态代理的切换 ；底层通过Spring提供的动态代理技术实现的。在运行期间，Spring通过动态代理技术动态的生成代理对象，代理对象方法执行时进行增强功能的介入，在去调用目标对象的方法，从而完成功能的增强，
* Spring-aop本身的切点很表达很复杂，所以映入了AspectJ框架使用起来更加容易
* AOP：面向切面的编程。是通过预编译方式和运行期动态代理实现程序统一维护的一种技术。



	二、作用以及优势
	
	作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强
	
	优势：减少重复代码，提高开发效率，并且便于维护



	三、使用步骤
	
	1、导入坐标

```xml
<!--       导入springAop使用的坐标-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
    </dependencies>
```

	2、创建一个需要增强的实体类，以及一个增强方法。
	
			》》创建一个多表操作的实体类（给了service）

```java
@Resource
UserDaoImpl userDao;

public void inserts(User user) throws SQLException {
    userDao.insert(user);
    SpringCha springCha = new SpringCha();
    springCha.setHabit("习惯");
    springCha.setHobby("爱好");
    int i=3/0;
    userDao.insert(springCha);
}
```

			》》给出一个事务管理器，来增强对业务控制

```java
@Component
public class TManager {

    @Resource(name = "druidDataSource")
    private DruidDataSource druidDataSource;

//    用于创建连接得ThreadLocal
    private ThreadLocal<Connection> local= new ThreadLocal<Connection>();

    /**
     * 基于Druid连接池获取连接
     * @return
     * @throws SQLException
     */
    public Connection getConnection() throws SQLException {
//        dao方法在获取连接的时候判断是否存在连接
        if (local.get()!=null){
            return local.get();
        }
//        创建新的连接
        Connection connection = druidDataSource.getConnection();
//        创建新的连接可能还被使用，所以把新的连接进行保存
        local.set(connection);
        return connection;
    }

    /**
     *关闭自动提交
     * @throws SQLException
     */
    public void closeCommit() throws SQLException {
        System.out.println("关闭自动提交...");
        getConnection().setAutoCommit(false);
    }
    /**
     *提交事务
     * @throws SQLException
     */
    public void commit() throws SQLException {
        System.out.println("手动提交...");
        getConnection().commit();
    }

    /**
     *回滚事务
     * @throws SQLException
     */
    public void rollBack() throws SQLException {
        System.out.println("异常抛出...");
        getConnection().rollback();
    }

}
```

	3、在spring核心配置文件中去实现对业务的功能增强

```xml
<!--    使用Aop进行事务处理-->
    <aop:config >
<!--        定义切面 需要增强事务的功能-->
        <aop:aspect ref="TManager">
<!--          抽取切点，定义切点 expression="execution(* 真实的业务类.*(..))"-->
            <aop:pointcut id="txPoint" expression="execution(* com.qinfeng.service.impl.UserServiceImpl1.*(..))"/>
<!--            指定开始时执行的方法-->
            <aop:before method="closeCommit" pointcut-ref="txPoint"/>
<!--            <aop:before method="closeCommit" pointcut="execution(* com.qinfeng.service.impl.UserServiceImpl1.*(..))"/>-->
<!--            指定正常返回值时执行的方法-->
            <aop:after-returning method="commit" pointcut-ref="txPoint"/>

<!--            指定抛出异常时执行的方法-->
            <aop:after-throwing method="rollBack" pointcut-ref="txPoint"/>
        </aop:aspect>
<!--        下面还可以在进行配置AOP-->
    </aop:config>
```

	4、进行测试



> 由于我们需要在配置文件中去处理，若是有很多业务需要去正确，那我们就需要在配置文件中写入很多，这样不仅繁琐，而且复杂。那我们就行需使用注解来完成业务功能的增强。

	三、注解
	
	1、在功能增强列中使用注解（创建一个计算耗时的功能增强类）

```java
@Component
// 定义切面
@Aspect
public class CountTime {
    private Long time;
//需要进行切点的实体类
    @Pointcut("execution(* com.qinfeng.service.impl.UserServiceImpl1.*(..))")
    public void cutPoint(){}
//    开始执行的方法
    @Before("cutPoint()")
    public void getStartTime(){
        time=System.currentTimeMillis();
    }
//    正常返回执行的方法
    @AfterReturning("cutPoint()")
//    抛出异常返回的方法
    @AfterThrowing("cutPoint()")
    public void printEnding(){
        Long ending=System.currentTimeMillis();
        System.out.println(ending-time+"ms");
    }
}
```

	2、在配置文件去读取我们aop注解，使注解生效

```xml
<!--    让aop注解生效-->
    <aop:aspectj-autoproxy/>
```

