Bean在spring下的实例化方式：

1. 方式一：构造器实例化方式
   * 要求：我们自定义的类型中存在无参的构造方法

2. 方式二：实例化工厂方式创建Bean

   1. 实现FactoryBean的接口方式

      * 自定义了一个类型Animal

      * 自定义工厂类实现了FactoryBean接口

      * 重写了getObject和getObjectType方法

      * 在beans.xml中配置

      * ```xml
        <bean id="animal" class="工厂的全限定名"></bean>
        ```

   2. 静态工厂方式实例化Bean

      - 自定义类的类型

      - 新建静态工厂（里面有个静态方法，返回我们想要的对象）

      - 将静态工厂以及方法配置到bean.xml中

      - ```xml
        <bean id="animal" class="工厂的全限定名" factory-method="静态方法"></bean>
        ```

   3. 实例工厂方式-配置的方式

      * 自定义类的类型

      * 自定义工厂，返回一个我们所需的对象

      * 先在beans.xml文件中配置一个工厂

      * ```xml
        <bean id="factory" class="工厂的全限定名"></bean>
        ```

      * 在beans.xml中配置

      * ```xml
        <bean id="animal" factory-bean="工厂的id名" factory-method="工厂里面的方法"></bean>
        ```

总结：Bean的生命周期（创建、初始化、运行、销毁）

