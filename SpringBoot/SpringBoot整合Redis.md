# SpringBoot整合Redis

## 1.Redis的使用

### 1.1、redis的概念

- 概念：redis是一款高性能的Nosql系列的非关系型数据库

![image-20220423102024871](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231020049.png)

- redis的开启使用

![image-20220423102958508](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231029577.png)

### 1.2、命令操作

#### （1）数据结构

- redis 的数据结构：key，value 格式的数据，其中的key是字符串，value有五种不同的数据结构

  - value的数据结构

    - 字符串类型：String

    1. 存储：set key value

    2. 获取：get key

    3. 删除：del key

    ![image-20220423104136653](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231041701.png)

    > <a href="https://www.redis.com.cn/tutorial.html">点击学习更多--redis中文教程</a>  里面有更加全面的命令

    - 哈希类型：hash  --> map格式

    1. 存储：hset key field value

    2. 获取：hget key field hgetall key

    3. 删除：hdel key field

    ![image-20220423105344335](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231053374.png)

    

    - 列表类型：list ---> linkedlist格式

    1. 存储：
       lpush key value：将元素加入列表的左侧

       rpush key value：将元素加入列表的右侧

    2. 获取：lrange key start end ：范围获取

    3. 删除：
       lpop key：删除列表的最左边的元素，并将元素返回
       rpop key：删除列表的最右边的元素，并将元素返回

    ![image-20220423111334739](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231113795.png)

    - 集合类型：set   $不允许重复元素

    1. 存储：sadd key value

    2. 获取：smembers key：获取set元素中所有元素

    3. 删除：srem key value：删除set集合中的某个元素

    ![image-20220423112232468](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231122518.png)

    - 有序集合类型：sortedset

    1. 存储：zadd key score value

    2. 获取：zrange key 

    3. 删除：zrem key value

    ![image-20220423113905975](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231139027.png)

    

    ![image-20220423103214155](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231032215.png)

#### (2)通用命令

- keys *：查询所有的键
- type key：获取键对应的value的类型
- del key：删除指定的key value

#### （3）持久化

1. redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中
2. redis持久化机制：

- RDB：默认方式，不需要进行配置，默认就是使用这种机制

  * 在一定的间隔时间中，检查key的变化情况，然后持久化数据

  ![image-20220423132722864](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231327067.png)

- ADF：日志记录的方式，可以记录每一条命令的操作，可以每一次命令操作后，持久化数据。

![image-20220423132740984](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204231327058.png)



## 2.SpringBoot整合

第一步：加入坐标

可以在创建项目时，在sql里面选择Nosql在里面选在第一个就可以了，或者是加入下面的坐标：

```xml
<!--        加载NoSql redis操作的坐标-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

在加载完后可以进行redis的配置在yml中：

```yml
# 这是redis的最本的配置
spring:
  redis:
    host: localhost
    port: 6379
```

进行编写测试类进行测试：

```java
@SpringBootTest
public class testRedis {

    @Autowired
    private RedisTemplate redisTemplate;

    /**
     * 使用redis进行测试，测试数据类型字符串
     */
    @Test
    public void testString(){
        ValueOperations valueOperations = redisTemplate.opsForValue();
        valueOperations.set("username","qin");
        System.out.println("存储成后进行打印...");
        Object username = valueOperations.get("username");
        System.out.println(username);
    }

    /**
     * 测试数据类型，Hash
     */
    @Test
    public void testHash(){
        HashOperations hashOperations = redisTemplate.opsForHash();
        hashOperations.put("hash","test1","qin");
        hashOperations.put("hash","test2","feng");
        System.out.println("--------");
        // 打印出hash中的一个值
        Object o = hashOperations.get("hash","test1");
        // 打印出hash中全部值，并把它转换层map集合
        Map hash = hashOperations.entries("hash");
        System.out.println(o);
        System.out.println(hash);
    }
}
```

> 注意：```private RedisTemplate redisTemplate``` 这是提供操作redis接口对象的RedisTemplate接口来进行操作；
>
> 而在需要去操作个各种的数据类型时，需要使用接口去获取操作数据类型的操作接口：使用  ops*

 Redis实现客户端的转换（jedis）

```xml
<!--        使用客户端jedis-->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>
```

在配置中去设置相应的属性：

![image-20220424123804848](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241238982.png)

```yml
spring: 
  redis:
    database: 0                # 使用0号数据池
    host: localhost
    port: 6379
    password: 123456            # 密码（默认为空）
    timeout: 6000               # 连接超时时长（毫秒）
    jedis:
      pool:
        max-active: 1000        # 连接池最大连接数（使用负值表示没有限制）
        max-wait: 10000         # 连接池最大阻塞等待时间（使用负值表示没有限制）
        max-idle: 10            # 连接池中的最大空闲连接
        min-idle: 5             # 连接池中的最小空闲连接
        time-between-eviction-runs: 300000      # 逐出扫描的时间间隔
    lettuce:
      shutdown-timeout: 100ms
```

在使用redis中的实现客户端：

![image-20220424125031719](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241250806.png)

