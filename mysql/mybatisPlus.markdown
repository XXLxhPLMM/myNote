- [MybatisPlus and SpringBoot](#mybatisplus-and-springboot)
  - [配置](#配置)
    - [原生](#原生)
    - [Spring](#spring)
    - [SpringBoot](#springboot)
  - [指定Mapper.xml文件路径](#指定mapperxml文件路径)
    - [SrpingBoot](#srpingboot)
  - [别名](#别名)
    - [SpringBoot](#springboot-1)
  - [DB策略配置](#db策略配置)
    - [idType](#idtype)
    - [tablePrefix](#tableprefix)
  - [条件构造器](#条件构造器)
  - [指定运行端口](#指定运行端口)
  - [依赖](#依赖)
    
# MybatisPlus and SpringBoot
## 配置
### 原生
> 使用`myBatis-config.xml`文件进行配置
> 使用 mybatisplus的配置类进行配置
### Spring
```xml
<bean id="sqlSessionFactory" class=""></bean>
```
### SpringBoot
```properties
 mybatis-plus.config-location=classpath:mybatis-config.xml
```
## 指定Mapper.xml文件路径
### SrpingBoot
```properties
mybatis-plus.mapper-locations=classpath*:myabtis/*.xml
```
## 别名
### SpringBoot
```properties
mybatis-plus.type-aliases-package=包名
```
## DB策略配置
### idType
- 类型: com.baomidou.mybatisplus.annotation.IdType
- 默认值: ID_WORKER

全局默认主键类型.设置后,可以省略实体对象中的@TableId(type = IdType.AUTO)配置

SpringBoot:
```properties
mybatis-plus.global-config.db-config.id-type=auto
```
Spring
```xml

```
### tablePrefix
数据表前缀
## 条件构造器

## 指定运行端口
> 第一配置文件中添加server.port=9090

> 第二在命令行中指定启动端口，比如传入参数一server. port=9000     java -jar bootsample. jar -- server.port=9000

> 第三传入虚拟机系统属性java - Dserver.port=9000 -jar bootsample.jar
## 依赖