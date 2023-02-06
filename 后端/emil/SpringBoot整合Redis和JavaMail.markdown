# SpringBoot整合Redis和JavaMail

​	在之前已经学习过使用redis进行缓存和使用JavaMail进行发送邮箱，那么我们就能做到一个小需求：进行注册登录时，可以使用邮箱发送验证码进行登录。（由于页面问题现在使用postman进行验证。）

## 案例：发送验证码到邮箱

### 1、创建一个新项目（SpringBoot项目）

在进行选在技术时，可以先进行选择一下技术：

![image-20220426143127207](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261431356.png)

在进入项目后我们还是需要进行加入其他技术的坐标，如mybatis-pus，以及Druid连接池，和使用发送邮件的技术坐标等等。

```xml
<dependencies>
    <!--        加载NoSql redis操作的坐标，通过技术选择可以只能帮我们加载-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>

    <!--        使用客户端jedis-->
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!--        druid连接池-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>

    <!--        mybatis-plus在springBoot的使用 需要数据库驱动才能进行使用-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.1</version>
    </dependency>

    <!--        整合JavaMail，进行发送邮件-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-mail</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
```

### 2、进行创建数据库以及进行配置yml所需的配置

#### 	1、进行创建数据库中字段

![image-20220426143545508](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261435548.png)

#### 	2、进行配置yml中的所需的配置

```yml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/***?serverTimezone=UTC # serverTimezone=UTC 进行统一时区
      username: ***
      password: *****
  redis:
    host: localhost
    port: 6379
    client-type: jedis # redis的使用jedis进行操作，这样可以避免后续的修改而造成的问题
  cache: # 缓存技术的选型 使用redis
    type: redis
    redis:
      # 缓存最大的存活时间
      time-to-live: 15m
  # 关于发送邮箱的
  mail:
    host: smtp.qq.com # 发送者的类型为qq
    username: *****@qq.com
    password: *****  # 这是某个QQ邮箱进行开启SMTP设置后显示的秘钥

mybatis-plus:
  global-config:
    db-config:
      id-type: auto
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

### 3、数据库的实体类以及表现层

#### 	1、进行封装数据库的实体类

```java
/**
 * 进行封装实体类
 * @TableName("yonghu")
 * 作用是为了对应着数据库中的yonghu表
 */
@TableName("yonghu")
public class Yong {
    private Integer id;
    private String yonghu;
    private String yonghukey;
    private String yonghumail;

    @Override
    public String toString() {
        return "Yong{" +
                "id=" + id +
                ", yonghu='" + yonghu + '\'' +
                ", yonghukey='" + yonghukey + '\'' +
                ", yonghumail='" + yonghumail + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getYonghu() {
        return yonghu;
    }

    public void setYonghu(String yonghu) {
        this.yonghu = yonghu;
    }

    public String getYonghukey() {
        return yonghukey;
    }

    public void setYonghukey(String yonghukey) {
        this.yonghukey = yonghukey;
    }

    public String getYonghumail() {
        return yonghumail;
    }

    public void setYonghumail(String yonghumail) {
        this.yonghumail = yonghumail;
    }
}
```

#### 	2、数据访问层mapper（使用mybatis-plus）

```java
/**
 * 使用mybatis-plus进行操作，
 * 通过扩展BaseMapper获取基础的操作数据的方法
 */
@Mapper
public interface YongHuMapper extends BaseMapper<Yong> {
}
```

### 4、业务逻辑层（service）

#### 1、进行操作数据库的业务

1. 进行数据业务操作的接口

```java
public interface YongHuService {

    /**
     * 进行添加数据
     * 同时还需要进行判断数据库中是否以及存在
     * @param yong
     * @return
     */
    int saveYongHu(Yong yong);

    /**
     * 进行使用用户名和密码进行登录进行验证
     * @param yong
     * @return
     */
    boolean select(Yong yong);

    /**
     * 使用邮箱登录，进行获取发送给邮箱验证码
     * @param yonghumail
     * @param code
     * @return
     */
    boolean selectMail(String yonghumail,boolean code);
}
```

2. 进行实现数据业务操作的接口

```java
@Service
public class YongHuServiceImpl implements YongHuService {

    // 使用YongHuMapper
    @Autowired
    private YongHuMapper yongHuMapper;

    @Override
    public int saveYongHu(Yong yong) {
        // 进行判断数据库中是否存在yong.getYonghumail()这个邮箱
        // 如果存在提示进行使用该邮箱登录否则，正常注册。
        QueryWrapper<Yong> wrapper=new QueryWrapper<Yong>();
        wrapper.eq("yonghumail",yong.getYonghumail());
        Yong one = yongHuMapper.selectOne(wrapper);
        // 后续还需考虑是否存在改用户名以及用户密码
        // 或是同一注册密码去生成不能的用户名；
        if (one!=null){
            return 0;
        }
        return yongHuMapper.insert(yong);
    }
// 进行用户名密码登录验证
    @Override
    public boolean select(Yong yong) {
        QueryWrapper<Yong> wrapper=new QueryWrapper<Yong>();
        wrapper.eq("yonghu",yong.getYonghu());
        wrapper.eq("yonghukey",yong.getYonghukey());
        return yongHuMapper.selectOne(wrapper)!=null?true:false;
    }
// 进行发送邮箱后获取验证码进登录
    @Override
    public boolean selectMail(String yonghumail,boolean code) {
        QueryWrapper<Yong> wrapper=new QueryWrapper<Yong>();
        wrapper.eq("yonghumail",yonghumail);
        return yongHuMapper.selectOne(wrapper)!=null && code ? true : false;
    }
}
```

#### 2、进行获取验证码

1. 进行获取验证码的接口

```java
public interface Msg {

    String getCode(String yonghumail);

    boolean checkCode(String yonghumail,String code);

}
```

2. 进行实现获取验证码的接口

```java
@Service
public class MsgImpl implements Msg {

    // 一个自动生成6位数的验证码的工具类
    @Autowired
    private CodUtils codUtils;
    
    // 在缓存中开辟出一个空间，用来放生成的验证码
    @Override
    @Cacheable(value = "codeQQ",key = "#yonghumail")
    public String getCode(String yonghumail) {
        String code = codUtils.generator(yonghumail);
        return code;
    }

    // 进行验证输入的密码和缓存中的验证码是否一致
    @Override
    public boolean checkCode(String yonghumail, String code) {
        String codeMail = codUtils.get(yonghumail);
        return code.equals(codeMail);
    }
}
```

3. 生成验证码的工具类

```java
@Component
public class CodUtils {

    // 进行对6位验证码进行少这补0的处理
    private String[] patch={"000000","00000","0000","000","00","0",""};

    /**
     * 给出qq邮箱后给我们随机生成一个6位数的验证码
     * @param yonghumail
     * @return
     */
    public String generator(String yonghumail){
        int hashCode = yonghumail.hashCode();
        int encryption=20220424;// 这个值可以随意的给,但是一般为了下面的异或结果足够大
        long result=hashCode^encryption;
        long nowTime=System.currentTimeMillis();
        result=result^nowTime;
        long code=result%1000000; // 对得到的结果进行取余（6位)
        code=code<0?-code:code;
        String codeStr=code+"";
        int len=codeStr.length();
        return patch[len]+codeStr;
    }

    /**
     * 获取存储在缓存中的验证码，供输入的的验证码和提供的验证码进行比较
     * @param yonghumail
     * @return
     */
    @Cacheable(value = "codeQQ",key = "#yonghumail")
    public String get(String yonghumail){
        return null;
    }
}
```

int hashCode = yonghumail.hashCode();中的hashCode（）表述的意思

![image-20220426150243259](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261502305.png)

4. 进行开启缓存

```java
@SpringBootApplication
//开启缓功能
@EnableCaching
public class TestsendqqmailApplication {

    public static void main(String[] args) {
        SpringApplication.run(TestsendqqmailApplication.class, args);
    }

}
```

#### 3、进行发送邮箱

1. 进行发送邮箱接口

```java
public interface SendMail {
    /**
     * 发送邮件
     */
    void sendMail(String yonghumail,String code);

}
```

2. 实现发送邮箱的接口

```java
@Service
public class SendMailImpl implements SendMail {
    /**
     * 实现进行发送邮件的业务
     */
//    进行发送邮件的所需要的接口JavaMailSender 
    @Autowired
    private JavaMailSender javaMailSender;

//    发送人
    @Value("${spring.mail.username}")
    private String from;
//    收件人
//    标题
    private String subject="登录验证";
//    正文
//    进行发送简单邮件
    @Override
    public void sendMail(String yonghumail,String code) {
        // 收件人
        String to=yonghumail;
        // 正文
        String context=yonghumail+":进行登录需要验证时，需要的验证码："+code+"/n15分钟后失效";
        SimpleMailMessage mssage=new SimpleMailMessage();
        //  mssage.setFrom(from+"枫");这样邮件发送过去后，邮件发送者 枫
        mssage.setFrom(from);
        mssage.setTo(to);
        mssage.setSubject(subject);
        mssage.setText(context);
        // 在使用javaMailSender后进行发送
        // 一个简单的邮件信息
        javaMailSender.send(mssage);
    }
}
```



5、表现层（Controller）

​	使用restful风格进行展示页面

```java
@RestController
@RequestMapping("/yonghu")
public class YongHuController {

    @Autowired
    private YongHuService yongHuService;

    /**
     * 从页面获取数据进行添加操作
     * @param yonghu
     * @param yonghukey
     * @param yonghumail
     * @return
     */
    @PostMapping("{a}/{b}/{c}")
    public String saveYong(@PathVariable("a") String yonghu,@PathVariable("b") String yonghukey,@PathVariable("c") String yonghumail){
        Yong yong=new Yong();
        yong.setYonghu(yonghu);
        yong.setYonghukey(yonghukey);
        yong.setYonghumail(yonghumail);
        int i = yongHuService.saveYongHu(yong);
        System.out.println();
        System.out.println(i);
        System.out.println();
        return i==1?"添加成功^_^":yonghumail+":已经被注册，可以使用该邮箱登录！";
    }
	/**
     * 使用用户名密码进行登录
     * @param yonghu
     * @param yonghukey
     * @return
     */
    @PostMapping("{a1}/{b1}")
    public String dengYong(@PathVariable("a1")String yonghu,@PathVariable("b1")String yonghukey){
        Yong yong=new Yong();
        yong.setYonghu(yonghu);
        yong.setYonghukey(yonghukey);

        boolean deng = yongHuService.select(yong);
        return deng?"登录成功":"用户或密码错误";
    }

    /**
     * 使用邮箱进行登录，先进行发送验证码到邮箱
     * @param yonghumail
     * @return
     */
    @Autowired
    private Msg msg;
    @Autowired
    private SendMail sendMail;
    @PostMapping("{yonghumail}")
    public String yanZheng(@PathVariable("yonghumail")String yonghumail){
        String code = msg.getCode(yonghumail);
        sendMail.sendMail(yonghumail,code);
        return code!=null?"发送成功":"数据异常发送验证码失败";
    }

    /**
     * 在查看邮件后进行输入验证码进行邮箱登录
     * @param yonghumail
     * @param code
     * @return
     */
    @GetMapping("{yonghumail}/{code}")
    public String MailDeng(@PathVariable("yonghumail")String yonghumail,@PathVariable("code")String code){
        boolean b = msg.checkCode(yonghumail, code);
        boolean mail = yongHuService.selectMail(yonghumail, b);
        return mail?"登录成功":"数据异常登录失败";
    }

}
```

1. 注册用户

进行注册一个新的用户，需要输入用户名密码以及邮箱（后续进行注册时可以尝试不用输入用户名而是自己生成，需要修改用户名可以在后续的的设置中去修改用户信息，先使用邮箱登录后在去修改，后面项目可以这样做）

![image-20220426152409220](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261524278.png)

进行注册新的用户：

![image-20220426152551668](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261525714.png)

2. 使用用户名登录

正确情况下：

![image-20220426152900315](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261529362.png)

错误情况下：

![image-20220426152946628](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261529669.png)

3. 邮箱登录

1、需要先进行发送邮箱获取验证（这页面中就是输入邮箱号后下面的进行输入验证码时的下面后右面有个发送验证）：

![image-20220426153313746](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261533835.png)

可以看到通过邮箱发送过来的验证码为499914；

2、进行使用邮箱和验证码登录：

![image-20220426153530119](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261535185.png)

在15分钟后改验证码会失效。

