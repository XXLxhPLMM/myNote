# SpringMVC 注解方式实现JSON数据转换

步骤一：导入JSON转换的jar包

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.3</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.9.3</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.9.3</version>
</dependency>
```

步骤二：创建一个控制器类，并填入数据

```java
package com.wolfcode.handler;

import com.wolfcode.entity.Student;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.HashMap;

@Controller
public class SecondHandler {

    @ResponseBody
    @RequestMapping("/json.zt")
    public Object getJson(){
        HashMap<Object, Object> map = new HashMap<>();
        map.put("code","200");
        map.put("msg","请求成功");
        Student student = new Student();
        student.setsName("你");
        student.setAge(2);
        student.setSex('男');
        map.put("data",student);
        return map;
    }
}
```

> @Controller 需要添加扫描器  context
>
> @ResponseBody 需要添加扫描器生效  mvc  作用：让这个方法自动执行JSON转换
>
> @RequestMapping("/json.zt") ：接口访问路径

步骤三：在applicationContext.xml配置文件中配置扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"

       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd">

    <mvc:annotation-driven></mvc:annotation-driven>

    <context:component-scan base-package="com.wolfcode"></context:component-scan>
</beans>
```

**执行运行：**

![image-20220412133026532](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133026.png)

# SpringMVC之GET和POST请求

1. **get请求**

   - **请求的数据为简单数据类型**

     - **页面**

       ```jsp
       <form action="/getreq.zt" method="get">
           姓名：<input type="text" name="uname"><br>
           密码：<input type="password" name="upassword">
           <input type="submit" value="提交">
       </form>
       ```

     - **controller**

       ```java
       @ResponseBody
       @RequestMapping("/getreq.zt")
       public Map<String,Object> getreq (String uname,String upassword){
           System.out.println(uname+"---"+upassword);
           HashMap<String, Object> map = new HashMap<>();
           map.put("uname",uname);
           map.put("upassword",upassword);
           return map;
       }
       ```

     - **请求成功**

       ![image-20220412133105814](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133105.png)

       ![image-20220412133120543](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133120.png)

   - **接受的数据类型为复杂数据类型比如一个数组**

     - **页面**

       ```jsp
       <form action="/getreq2.zt" method="get">
           打游戏：<input type="checkbox" name="hobby" value="打游戏">
           耍游戏：<input type="checkbox" name="hobby" value="耍游戏">
           看游戏：<input type="checkbox" name="hobby" value="看游戏">
           <input type="submit" value="提交">
       </form>
       ```

     - **controller**

       ```java
       @ResponseBody
       @RequestMapping("/getreq2.zt")
       public Map<String,Object> getreq(String[] hobby){
           HashMap<String, Object> map = new HashMap<>();
           map.put("hobbies", Arrays.toString(hobby));
           System.out.println(Arrays.toString(hobby));
           return map;
       }
       ```

     - **请求成功**

       ![image-20220412133137550](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133137.png)

       ![image-20220412133150792](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133150.png)
       
       > **数据获取成功，只是出现乱码问题，后面回去解决**

   - **如果需要传入较多的参数时，我们可以将其封装成一个实体类，在传参时传入这个对象即可**

     - **页面**
   
       ```jsp
       <form action="/getreq3.zt" method="get">
           姓名：<input type="text" name="uname"><br>
           密码：<input type="password" name="upassword">
           电话：<input type="text" name="phone"><br>
           地址：<input type="text" name="address">
           <input type="submit" value="提交">
       </form>
       ```

     - 实体类
   
       ```xml
       <!--       简化bean代码的工具包-->
               <dependency>
                   <groupId>org.projectlombok</groupId>
                   <artifactId>lombok</artifactId>
                   <optional>true</optional>
                   <version>1.18.4</version>
               </dependency>
       ```
       
       使用lombok工具进行简化我们的实体类
       
       ```java
       package com.wolfcode.entity;
       
       //使用@Data可以帮助我们完成getset以及toString的方法
       @Data
       //可以帮我们完成构造无参构造的方法
       @NoArgsConstructor
       //完成有参构造方法的注入
       @AllArgsConstructor
       public class Register {
       
           private String uname;
           private String upassword;
           private String phone;
           private String address;
       }
       ```
       
     - **controller**
     
       ```java
       @ResponseBody
       @RequestMapping("/getreq3.zt")
       public Map<String,Object> getreq(Register register){
           HashMap<String, Object> map = new HashMap<>();
           map.put("data",register);
           System.out.println(register);
           return map;
       }
       ```
     
     - **请求成功**
     
       ![image-20220412133256769](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133256.png)
     
       ![image-20220412133308666](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133308.png)

> **注意：**
>
> 1. **页面数据参数的name属性需要和controller里面参数名一致**
>
> 2. **如果页面参数的name属性太长，后端人员不想用前端给的参数名，可以使用注解@RequestParam("前端页面name属性值") 参数类型 参数的方式来实现，但是一个注解只能对应一个参数，比如**
>
>    **页面：**
>
>    ```jsp
>    <form action="/getreq.zt" method="get">
>        姓名：<input type="text" name="utudedadsantname"><br>
>        密码：<input type="password" name="dadadaadupassword">
>        <input type="submit" value="提交">
>    </form>
>    ```
>
>    **controller：**
>
>    ```java
>    @ResponseBody
>    @RequestMapping("/getreq.zt")
>    public Map<String,Object> getreq (@RequestParam("utudedadsantname") String uname, @RequestParam("dadadaadupassword") String upassword){
>        System.out.println(uname+"---"+upassword);
>        HashMap<String, Object> map = new HashMap<>();
>        map.put("uname",uname);
>        map.put("upassword",upassword);
>        return map;
>    }
>    ```
>
>    **请求成功：**
>
>    ![image-20220412133434470](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133434.png)
>
>    ![image-20220412133446590](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133446.png)
>    
>    ![](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412133530.png)

2. **post请求**

**只需要将method修改为post就可以了，效果与get差不多，只不过请求头上没有了数据**

**页面：**

```jsp
<form action="/getreq3.zt" method="post">
    姓名：<input type="text" name="uname"><br>
    密码：<input type="password" name="upassword">
    电话：<input type="text" name="phone"><br>
    地址：<input type="text" name="address">
    <input type="submit" value="提交">
</form>
```

**controller：**

```java
@ResponseBody
@RequestMapping("/getreq3.zt")
public Map<String,Object> getreq(Register register){
    HashMap<String, Object> map = new HashMap<>();
    map.put("data",register);
    System.out.println(register);
    return map;
}
```

**请求成功：**

![image-20220412140219311](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140219.png)

![image-20220412140231743](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140231.png)

# SpringMVC之GET和POST请求解决乱码的方法

**如果在发送请求的时候使用到了中文，可能会出现乱码的问题（上面就有的情况是这样的）**

1. **解决get请求乱码问题**

   **找到pom.xml文件，找到Tomcat插件配置**

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
               
   <!--          解决get请求乱码-->
             <uriEncoding>UTF-8</uriEncoding>
               
           </configuration>
         </plugin>
       </plugins>
     </build>
   ```

   **运行结果**

   ![image-20220412140255877](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140255.png)

   ![image-20220412140428398](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140428.png)

2. **解决post请求乱码问题**

   **找到web.xml文件，配置解决post请求乱码问题**

   ```xml
   <filter>
     <filter-name>CharacterEncodingFilter</filter-name>
     <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
     <init-param>
       <param-name>encoding</param-name>
       <param-value>UTF-8</param-value>
     </init-param>
   </filter>
   <filter-mapping>
     <filter-name>CharacterEncodingFilter</filter-name>
     <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

   **运行结果**

   ![image-20220412140531067](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140531.png)

   ![](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140531.png)

3. **同时解决get和post请求乱码问题（原理）**

   **页面：**

   ```jsp
   <form action="/getreq3.zt" method="post">
       姓名：<input type="text" name="uname"><br>
       密码：<input type="password" name="upassword">
       电话：<input type="text" name="phone"><br>
       地址：<input type="text" name="address">
       <input type="submit" value="提交">
   </form>
   ```

   ```jsp
   <form action="/getreq3.zt" method="get">
       姓名：<input type="text" name="uname"><br>
       密码：<input type="password" name="upassword">
       电话：<input type="text" name="phone"><br>
       地址：<input type="text" name="address">
       <input type="submit" value="提交">
   </form>
   ```

   **controller：**

   ```java
   @ResponseBody
   @RequestMapping("/getreq3.zt")
   public Map<String,Object> getreq(Register register){
       System.out.println("乱码："+register);
       //1.获取到乱码前实体类里面的乱码数据
       String uname = register.getUname();
       String address = register.getAddress();
       //2.获取到后将其原先的ISO-8859-1的编码方式转化为utf-8的编码方式，再将转变好的utf-8存入到register对象中
       try {
           byte[] unameBytes = uname.getBytes("ISO-8859-1");
           uname= new String(unameBytes, "utf-8");
           register.setUname(uname);
           byte[] addressBytes = address.getBytes("ISO-8859-1");
           address = new String(addressBytes, "utf-8");
           register.setAddress(address);
       } catch (UnsupportedEncodingException e) {
           e.printStackTrace();
       }
       //3.最后返回
       HashMap<String, Object> map = new HashMap<>();
       map.put("data",register);
       return map;
   }
   ```

   **测试结果：**

   ![image-20220412140709212](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140709.png)

   ![image-20220412140716998](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140717.png)

   
   
   > 注意：在解决字符编码问题时，只能存在一个处理方法，如果一个项目中存在多个解决字符编码的问题的话，可能就不会起作用。
   >
   > **同时解决get和post请求乱码的问题的思路**
   >
   > 1. **调用对象的get方法获取到乱码的数据**
   > 2. **调用乱码数据的getBytes("ISO-8859-1")方法获取到它乱码时字符编码的字节数组**
   > 3. **然后调用new String()方法，第一个参数填入目标字节数组，第二个参数填入utf-8编码规则**
   > 4. **最后将编码正确的数据重新set到对象中就可以了**

# SpringMVC之文件上传

**页面：**

```jsp
<form action="/upload.zt" method="post" enctype="multipart/form-data">
    <input type="file" name="myFile">
    <input type="submit" value="提交">
</form>
```

> **文件上传必备属性`enctype="multipart/form-data"`**

**controller：**

```java
@ResponseBody
@RequestMapping("/upload.zt")
public Map<String,Object> upload(MultipartFile myFile){
    //定义一个返回值
    HashMap<String, Object> map = new HashMap<>();
    try {
        //1.获取文件输入流
        InputStream is = myFile.getInputStream();
        String url = "C:\\Users\\user\\Desktop\\upload\\";
        File file = new File(url+myFile.getOriginalFilename());
        //文件输出流
        OutputStream os = new FileOutputStream(file);
        byte[] bytes = new byte[1024];
        int len = 0;
        //文件边读边写
        while ((len=is.read(bytes)) != -1){
            os.write(bytes);
        }
        //文件关闭流
        os.close();
        is.close();
        //如果成功保存文件信息就返回一个成功的msg
        map.put("msg","保存成功");
    } catch (IOException e) {
        e.printStackTrace();
        //如果出现异常情况就返回一个失败的msg
        map.put("msg","保存失败");
    }
    return map;
}
```

> **文件上传思路：**
>
> 在服务端上传文件，是以文件流的形式进行的，所以得先获取文件的输入流（is），有了输入流就会有个输出流（os），输出流针对的参数那就是一个文件，所以我们得先new File文件。由于文件上传到客户端在磁盘中进行保存，而我们PC电脑既充当客户端，也充当服务端，所以我们可以在桌面创建一个文件夹，复制它的绝对路径url，然而我们上面new Flie文件的参数需要确定上传的文件的全路径，我们上传的所有形式的文件的全路径都基于我们桌面新建文件的url地址+上传文件的文件名+后缀，所以在new Flie对象中的参数为`url+myflie.getOriginalFilename`，后面的`myflie.getOriginalFilename()方法可以直接获取到上传文件的文件名+后缀`。最后我们对输出的文件进行边读边写的操作，因为不知道我们上传的文件需要进行几次读写操作所以使用while循环，调用`is.read()方法读取一个文件容器的字节大小`，每读一次就会返回一个长度值，只有长度不为`-1`就会一直读下去知道长度为`-1`，然后读一次就执行一次写文件的操作`os.write(定义的文件容器的字节大小)`。







**运行结果**

![image-20220412140751943](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140751.png)

![image-20220412140804597](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140804.png)

![image-20220412140816175](https://typora-text.oss-cn-chengdu.aliyuncs.com/20220412140816.png)

