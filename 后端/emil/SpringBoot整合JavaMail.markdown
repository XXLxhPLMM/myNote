## SpringBoot整合JavaMail

​	随着整合的内容逐渐变多，我们需要掌握整合第三技术，需要先导入坐标，再进行配置，使用。进行发送邮件的协议。

- SMTP：简单邮件传输协议，用于发送电子邮件的传输协议
- POP3：用于接收电子邮件的标准协议
- IMAP：互联网消息协议，是POP3的代替协议

### 1.导入JavaMail坐标

```xml
<!--        整合JavaMail，进行发送邮件-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

### 2.进行配置JavaMail

在导入jar包后，还需要进行配置JavaMail在yml中

```yml
# 多环境开发，
# 使用那个公共配置
spring:
#  profiles:
#    active: pro
#    下面的是哪些包或文件不进行热部署
#  devtools:
#    restart:
#      exclude: static/**,templates/**
# 关于发送邮箱的
  mail:
    host: smtp.qq.com #使用qq 也可以更换成其他的
    username: ********@qq.com #进行发送信息的邮件
    password: ********** #这个密码是需要在发送者邮箱中进行开启smtp协议后获取的秘钥
```

获取秘钥--来到邮箱的设置的账户下面：

![image-20220421104713000](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204211047601.png)

### 3.开启定时任务发送功能

#### 1.进行简单邮件发送

​	在service层进进行实现发送邮件的功能；

```java
@Service
public class SendMailImpl implements SendMail {
    /**
     * 实现进行发送邮件的业务
     */
//    进行发送邮件的
    @Autowired
    private JavaMailSender javaMailSender;
//    发送人
    @Value("${spring.mail.username}")
    private String from;
//    收件人
    private String to="********@qq.com";
//    标题
    private String subject="测试邮件";
//    正文
    private String context="测试邮件..正文内容(空)!";
//    进行发送简单邮件
    @Override
    public void sendMail() {
        // 使用获取简单的邮件信息
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

> 在进行发送邮件的过程中需要
>
> ```java
> //    发送人
>     @Value("${spring.mail.username}")
>     private String from;
> //    收件人
>     private String to="********@qq.com";
> //    标题
>     private String subject="测试邮件";
> //    正文
>     private String context="测试邮件..正文内容(空)!";
> ```

#### 2.进行多部邮件的发送

```java
/**
 * 发送复杂一点的邮件
 */
@Override
public void sendMails() {
    try {
        // 进行发送复杂信息
        MimeMessage mssage=javaMailSender.createMimeMessage();
        // 需要使用MimeMessageHelper helper=new MimeMessageHelper(mssage);
        // (mssage,true)可以进行发送附件，否则会报一个错误
        MimeMessageHelper helper=new MimeMessageHelper(mssage,true);
        // 进行报异常
        helper.setFrom(from);
        helper.setTo(to);
        helper.setSubject(subject);
        //helper.setText(context,true);
        // 可以把html的格式进行转换
        helper.setText(context,true);

        // 添加邮件的附件
        File f1=new File("F:\\idea_wenjian\\springBoot\\bootTest\\operation\\test.txt");
        File f2=new File("F:\\idea_wenjian\\springBoot\\bootTest\\operation\\cat.jpeg");
        // f1.getName()接收到文件名
        helper.addAttachment(f1.getName(),f1);
        // "机器猫.jpeg"带上jpeg可以再邮件里面进行查看，
        // 否则可能把图片发过去而无法预览
        helper.addAttachment("机器猫.jpeg",f2);
        javaMailSender.send(mssage);
    } catch (MessagingException e) {
        e.printStackTrace();
    }
}
```

无法进行发送附件的错误报错：

```cassandra
java.lang.IllegalStateException: Not in multipart mode - create an appropriate MimeMessageHelper via a constructor that takes a 'multipart' flag if you need to set alternative texts or add inline elements or attachments.
```

> 我们可以发现简单邮件和复杂点邮件发送区别不大，但是还要注意他们所需的参数不同导致邮件发送失败，
>
> MimeMessageHelper helper=new MimeMessageHelper(mssage,true);，这是里面的true就是表示了，我们在进行发送附件时，是否需要进行携带附件。
>
> 其中在简单邮件发送中，没有这个参数，实质上是有这个参数的，但是默认的参数值是false。

### 4.进行测试发送邮件

```java
@SpringBootTest
public class SendMailTest {

    @Autowired
    private SendMail sendMail;

    @Test
    public void sendMailTest(){
        // 测试简单邮件
        sendMail.sendMail();
        // 测试复杂一点的邮件发送
         sendMail.sendMails();
    }
}
```

> 进编写一个测试类
>
> 整合JavaMail可以结合着我们的验证码验证（缓存技术MongoDB|redis等等），这样就能实现使用邮箱获取验证码

> 多部邮件发送后的图片：
>
> ![image-20220421112443204](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204211124260.png)

