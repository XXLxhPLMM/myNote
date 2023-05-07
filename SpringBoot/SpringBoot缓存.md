# SpringBoot缓存

缓存：

- 缓存是一种介于数据永久存储介质与数据应用之间的数据临时存储介质
- 使用缓存可以有效的减少低速数据读取过程的次数（例如磁盘IO），提高系统性能
- 缓存不仅可以用于提高永久性存储介质的数据读取效率，还可以提供临时的数据存储空间

## 1.手动设置缓存：

### （1）第一种：从数据库获取值

```java
@Service
public class BookServiceImpl implements BookService {

//    @Autowired
//    private BookMapper bookMapper;

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public List<Book> listAll() {
//        List<Book> list = bookMapper.selectList(null);
//        return list;
        List<Book> list =jdbcTemplate.query("select * from book",new BeanPropertyRowMapper<Book>(Book.class));
        return list;
    }
// 设置HashMap进行模拟缓存
    private HashMap<Integer, List<Book>> hashMap  =new HashMap<Integer, List<Book>>();

    @Override
    public List<Book> byId(Integer id) {
        // id为key 通过id键获取hashmap里面的值
        List<Book> books = hashMap.get(id);
        // 判断以id为键的值是否为空
        if (books==null){
            List<Book> listId = jdbcTemplate.query("select * from book where id=?", new BeanPropertyRowMapper<Book>(Book.class), id);
            // 为空把值存到hashmap中
            hashMap.put(id,listId);
            // 返回
            return listId;
        }
        // 不为空得到id的对应的值
        return hashMap.get(id);
    }
}
```

![image-20220424142004689](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241420732.png)



### （2）第二种：从外部进行获取

```java
@Service
public class MsgImpl implements Msg {

    private HashMap<String,String> hashMap=new HashMap<String, String>();

    @Override
    public String get(String qqMail) {
        String code = qqMail.substring(qqMail.length() - 6);
        hashMap.put(qqMail,code);
        return code;
    }

    @Override
    public boolean check(String qqMail, String coed) {
        String key = hashMap.get(qqMail);
        return coed.equals(key);
    }
}
```

![image-20220424182139747](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241821013.png)



## 2.SpringBoot提供的缓存技术

### 1、添加cache的坐标

```xml
<!--        cache 缓存-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
```

### 2、进行开启cache

```java
@SpringBootApplication
//开启缓功能
@EnableCaching
public class DbandredisApplication {

    public static void main(String[] args) {
        SpringApplication.run(DbandredisApplication.class, args);
        System.out.println("test...");
    }
}
```

> @EnableCaching是进行开启cache缓存的

### 3、进行使用缓存

```java
    @Override
    @Cacheable(value = "test",key = "#id")
    public List<Book> byId(Integer id) {
        List<Book> listId = jdbcTemplate.query("select * from book where id=?", new BeanPropertyRowMapper<Book>(Book.class), id);
        return listId;
    }
```

> 使用@Cacheable(value = "test",key = "#id")表示：
>
> 给缓存进行开辟一个空间test，空间中键的为id

## 案例（使用默认的cache）：

进行邮箱验证：

```java
@Service
public class MsgImpl implements Msg {
    @Autowired
    private CodUtils codUtils;

    @Override
    // 可以进行存储到缓存中，也可以进行取出来
//    @Cacheable(value = "codeQQ",key = "#qqMail")
    // 使用@CachePut可以做到把数据存储到缓存中，而不会取出来
    @CachePut(value = "codeQQ",key = "#qqMail")
    public String get(String qqMail) {
        String code = codUtils.generator(qqMail);
        return code;
    }

    /**
    *进行比对缓存中的qqMail 和获取到的code和输入进来的code进行比较
    */
    @Override
    public boolean check(String qqMail, String coed) {
        String codeMail = codUtils.get(qqMail);
        return coed.equals(codeMail);
    }
}
```

> 写一个工具类进行对验证码的随机性：
>
> ```java
> /**
>  * 处理验证码，进行随机输出
>  */
> @Component
> public class CodUtils {
> 
>     // 进行对6位验证码进行少这补0的处理
>     private String[] patch={"000000","00000","0000","000","00","0",""};
> 
>     /**
>      * 给出qq邮箱后给我们随机生成一个6位数的验证码
>      * @param qqMail
>      * @return
>      */
>     public String generator(String qqMail){
>         int hashCode = qqMail.hashCode();
>         int encryption=20220424;
>         long result=hashCode^encryption;
>         long nowTime=System.currentTimeMillis();
>         result=result^nowTime;
>         long code=result%1000000;
>         code=code<0?-code:code;
>         String codeStr=code+"";
>         int len=codeStr.length();
>         return patch[len]+codeStr;
>     }
> 
>     /**
>      * 获取存储在缓存中的验证码，共输入的的验证码和提供的验证码进行比较
>      * @param qqMail
>      * @return
>      */
>     @Cacheable(value = "codeQQ",key = "#qqMail")
>     public String get(String qqMail){
>         return null;
>     }
> }
> ```

写一个Controller层：

```java
@RestController
@RequestMapping("/code")
public class MsgController {

    @Autowired
    private Msg msg;

    @GetMapping("{qqMail}")
    public String code(@PathVariable("qqMail") String qqMail){
        String code = msg.get(qqMail);
        return code;
    }

    @PostMapping("{qqMail}/{code}")
    public boolean check(@PathVariable("qqMail")String qqMail,@PathVariable("code")String code){
        boolean check = msg.check(qqMail, code);
        return check;
    }
}
```

下图是生成验证码：

![image-20220424191912542](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241919663.png)

下图是进行比较验证码是否正确：

![image-20220424191958288](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241919363.png)



## 使用redis进行缓存

### 1、导入redis的坐标

```xml
<!--        加载NoSql redis操作的坐标-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

<!--        使用客户端jedis-->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>
```

### 2、进行配置redis缓存

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/sms?serverTimezone=UTC
    hikari:
     driver-class-name: com.mysql.cj.jdbc.Driver
     username: root
     password: 123456
  redis:
    host: localhost
    port: 6379
    client-type: jedis
  cache:
    type: redis
    redis:
      # 是否使用前缀
      #use-key-prefix: true
      # 缓存为空的值是否进行缓存
      #cache-null-values: false
      # 设置前缀
      #key-prefix: aa
      # 缓存最大的存活时间
      time-to-live: 10s
```

> 其中使用前缀和设置前缀是由冲突的，建议使用默认值
>
> ![image-20220424193432084](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204241934107.png)
>
> 并且缓存空值默认是false，这样我们只需要进行设置最大的缓存存活时间

### 3、案例

不需要进行修改前面使用cache进行缓存的操作，因为在配置中cache缓存使用的redis。（也就是使用默认的cache）案例不用修改

> 在使用redis进行缓存时，我们需要在yml中去进行配置即可：
>
> ```yml
> srping:
>  cache:
>    type: redis
>    redis:
> ```
>
> 进行使用redis缓存。这就可以进行完成使用redis进行缓存。

> 这样就能结合前的SpringBoot整合JavaMail 去实现一个QQ邮箱的一个验证，常用于登录页面进行验证。

