# mysql的主从复制

​	问题：当我们在使用数据库时，在一个程序中要是面临很多的用户同时进行访问，那样大量的访问数据库，这就造成了大量的读写压力都有一台数据来承担，压力大，造成数据库服务器的磁盘损坏则数据会丢失，单点故障。

![image-20220507110412339](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071104491.png)



​	解决问题：使用多台数据来分担压力，同时数据库有多个可以实现数据备份，以及读写分离。

![image-20220507110619247](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071106396.png)



---

## Mysql主从复制

![image-20220507110733142](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071107310.png)

**配置-前置条件**

提前准备好两台服务器，分别安装mysql 并启动服务器。

配置-主库Master

![image-20220507123738077](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071237131.png)

> ```sql
> [mysqld]
> log-bin=mysql-bin #[必须]启动二进制日志
> server-id=100 #[必须]服务器的唯一ID
> ```
>
> 其中的server-id不是固定的，可以自己给个id

在修改完配置文件后，我们需要去刷新服务，是修改完后，可以正常启动

![image-20220507124005011](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071240052.png)

> ```sql
> systemctl restart mysqld
> ```

然后登录Mysql数据库，施行sql

![image-20220507124046380](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071240454.png)

> ```sql
> GRANT REPLICATION SLAVE ON *.* to 'xiaoming'@'%' identified by 'Root@123456'
> ```

第四步：

![image-20220507124718697](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071247759.png)

> ```slq
> show master status
> ```

---

## 从库的配置

![image-20220507124950185](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071249239.png)

![image-20220507125025431](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071250482.png)

![image-20220507125039738](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071250805.png)

> ```sql
> change master to master_host='192.168.138.100',master_user='xiaoming',master_password='Root@123456',master_log_file='mysql-bin.000001',master_log_pos=439;
> ```
>
> ```sql
> start slave;
> ```
>
> 注意：使用mysql-bin.000001是从主从配置里面的第四步进行查找的。master_log_pos=439也是查找到的。

![image-20220507125720017](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071257058.png)

在进行时出现上面的错误，按照提示的去进行关闭slave；

![image-20220507125846746](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071258794.png)



## 读写分离

![image-20220507130019325](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071300439.png)

### Sharding-JDBC来帮助我们完成读写分离

![image-20220507130126154](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071301276.png)

#### 相关坐标：

```xml
<!--        Sharding-JDBC来帮助我们完成读写分离-->
        <dependency>
            <groupId>org.apache.shardingsphere</groupId>
            <artifactId>sharding-jdbc-spring-boot-starter</artifactId>
            <version>4.0.0-RC1</version>
        </dependency>
```

**Sharding-JDBC入门案例**

![image-20220507130605371](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071306412.png)

#### 在配置文件yml中进行配置：

```yml
server:
  port: 8080
spring:
  shardingsphere:
    datasource:
      names:
        master,slave
      # 主数据源
      master:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.138.100:3306/rw?characterEncoding=utf-8
        username: root
        password: root
      # 从数据源
      slave:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://192.168.138.101:3306/rw?characterEncoding=utf-8
        username: root
        password: root
    masterslave:
      # 读写分离配置
      load-balance-algorithm-type: round_robin # 负载均衡，这个值的是从库（从库可能有多个）的负载均衡的策略，轮换着来从每个从库中
      # 最终的数据源名称
      name: dataSource
      # 主库数据源名称
      master-data-source-name: master
      # 从库数据源名称列表，多个逗号分隔
      slave-data-source-names: slave
    props:
      sql:
        show: true #开启SQL显示，默认false
  main:
    allow-bean-definition-overriding: true
mybatis-plus:
  configuration:
    #在映射实体或者属性时，将数据库中表名和字段名中的下划线去掉，按照驼峰命名法映射
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: ASSIGN_ID
```

> 要是没有进行配置
>
> ```yml
>  main:
>     allow-bean-definition-overriding: true
> ```
>
> 会进行报错，这个报错是，配置的数据数据无法识别读取数据库
>
> ![image-20220507131543617](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205071315698.png)



