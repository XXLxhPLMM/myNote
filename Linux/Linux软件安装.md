# Linux软件安装

![image-20220504121214172](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041212311.png)

## 安装JDK

![image-20220504121421927](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041214993.png)

> 注意我们在解压时安装jdk时，建议把它存放到   /usr/local 用于存放下载的软件便于查找。

![image-20220504122443678](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041224710.png)

这样在解压完成后，我们还是需要进行取配置环境变量，使用vim命令修改/etc/profile文件在文件里面进行加入JDk1.8的配置信息。

![image-20220504123022093](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041230118.png)

再进行保存并退出。

然后，在配置好环境变量，要是不去从新加载一下修改后的配置文件，这样就算配置好了也是不能用的。还需要我们去使用source /etc/profile重新加载一下配置文件

![image-20220504123920621](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041239649.png)

## 安装Tomcat

![image-20220504124235759](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041242813.png)

![image-20220504125124636](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041251685.png)

在我们把文件传输到虚拟机后，我们可以使用`tar -zxvf 解压文件 -C 解压后文件路径`，这样我们在解压后的文件路径下，我们找到Tomcat的bin目录，在里面有很多可以操作的命令。其中 使用`sh startup.sh`可以进行开启Tomcat服务。

在开启Tomcat服务后，如何确认是否开启呢：

![image-20220504125527562](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041255647.png)

上面的的几种方法都可以进行查看是否开启服务：

下面使用ps -ef|grep tomcat:(使用ps -ef 是查看去全部的进程，再使用grep tomcat 查找关于Tomcat的进程)

![image-20220504125903095](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041259139.png)

这样我们可以看到Tomcat以及启动成功，这样我们就可以在虚拟机上的浏览器上面是输入`192.xxx.xx.xxx:8080`或者是输入`localhost:8080`可以看到Tomcat的官网;

> 要是启动成功，但是我们通过本地的浏览器无法进行打开Tomcat的官网，这是由于我们的虚拟机默认把防火墙给开启的：
>
> ![image-20220504130646978](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041306076.png)



在我们进过程中，我们在使用Tomcat后，在工作完成后我们需要把他们进行关闭，

![image-20220504131549731](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041315805.png)

> 建议：我们在关闭Tom服务时，使用脚本进行关闭，在Tomcat的bin下使用 `sh shutdowm.sh`来关闭Tomcat，
>
> 使用**kill**一般是我们在通过正常的手段以及无法关闭某个服务，这样就需要我们进行`ps -ef|gerp 服务名称`查找到后在使用 `kill -9 ip`;

## 安装mysql

![image-20220504132441499](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041324581.png)

![image-20220504132836338](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041328367.png)

通过使用`rpm -pa|grep mariadb`可以发现系统中以及安装了 mariadb，这样我们在安装mysql时，可以能回出现错误。

这样我们就需要进行卸载软件`rpm -e --nodeps 软件名称`：

![](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041334309.png)

![image-20220504133752153](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041337185.png)

这样我们把`rpm -e --nodeps mariadb-libs-5.5.64-1.el7.x86_64`删除成功。下面就能按照mysql

按照步骤：

![image-20220504133929636](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041339717.png)

在解压完成可以看到六个rpm文件，这样我们还需要去按照rpm软件包并且之间还有相互依赖的关系，这就需要我们按照顺序去按照rpm：

![image-20220504134814447](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041348541.png)

其中在安装到最后一个时，我们要是安装最后一个失败而报了一个错误：

![image-20220504135258147](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041352188.png)

在安装好，我们可以使用mysql命令查看是否安装好：

![image-20220504135519213](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041355258.png)

这个表明我们还需要进行进行启动mysql；

![image-20220504135605886](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041356977.png)

在使用tpm安装好后，在启动好的服务后我们在mysql的日志文件里面可以进行查阅数据库的临时密码（日志文件是固定的。）：

![image-20220504140143676](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041401741.png)

在进入mysql后可以修改mysql的访问密码这与我们使用：

![image-20220504135955181](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041359243.png)

在使用临时密码进入后我们可以去使用`set password=password('设置密码'); `来修改数据库密码；

在修改完成后我们需要重新的登录mysql，这样才能进行开启访问权限`grant all on *.* to 'root'@'%' identified by '密码'; `在开启后我们需要再去使用 `flush privileges`

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION
```

![image-20220504142439550](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041424602.png)

并且在开启访问权限后，我们可以在本地上使用Navicat进行连接；在连接时，要是没有连接成功，而以上操作也没有错，这就需要我们去想，是不是防火墙的原因，导致我们3306端口在从外部连接时失败没这就需要用使用开启防火墙的3306端口：

```java
firewall-cmd --zone=public --add-port=3306/tcp --permanent  //这里使用进行开启3306端口的，使用防火墙不去处理这个端口
firewall-cmd --reload // 使修改完的后，立即生效

firewall-cmd --zone=public --list-ports // 查看开放的端口

```

> 这些在上面的Tomcat那边的有比较详细的内容。

## 安装lrzsz

![](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041434840.png)

> 这个软件可安可不安，要是安装了可以使用  rz 命令弹出一个文件传输框，可以供我们进行文件上传和下载。 



---

# Linux上项目部署

## 手工部署

​	将项目进行手工部署，先需要一个把它打成jar包，这样我们可以把它进上传到虚拟机上去。

同时建议我们上传的项目给他放到：

![image-20220504152117097](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041521135.png)

这个目录下面，便于我们的后期的查找。

​	在我们把项目的jar上传的相应的目录下我们可以在改目录下使用 `java -jar 项目名称.jar` 来**启动项目**

同时我们还需要进行检测我们的对应的**端口是否对外进行开放**：

![image-20220504152432643](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041524701.png)

但是我们在进行启动项目时我们的屏幕就被它全部占领，同时当我们关闭它时，我们的项目就无法运行了。

![image-20220504152717348](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041527432.png)

在上图中的命令，总的来说就一个命令语句：使用`nohup java -jar 项目名称.jar &> 日志文件名.log &` 这些语句的含义是，在后台进行运行，同时在改目录下去生成一个日志文件用来存放每次执行时打印的日志给保存到改文件中。

同时当我们需要进行关闭改项目时我们就需要使用到	`ps -ef|grep java`: 

![image-20220504153550561](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041535619.png)

在找到后，我们在使用 `kill -9 ip` 就可以关闭该项目；

## 通过Shell脚本进行自动部署项目

![image-20220504153851885](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041538965.png)

在使用shell进行安装时，我们还需要先安装Git：

![image-20220504154133919](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041541991.png)

![image-20220504154536783](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041545822.png)

在安装好Git后，我们还需要进行一个安装一个maven：

![image-20220504154659117](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041546192.png)

这样我们就需要在/usr/local 目录下，创建一个repo文件夹。在去设置我们maven的本地仓库，中去进行设置，，把本地maven仓库放到` <localRepository>/usr/local</localRepository>`中。

在这一切整理好后，我们可以使用git的命令从远程仓库去拉取代码供我们使用。

​	这就是需要我们在/usr/local 目录下创建一个新的目录sh 用来存放bootStart.sh 脚本的。

```sh
#!/bin/sh
echo =================================
echo  自动化部署脚本启动
echo =================================

echo 停止原来运行中的工程
APP_NAME=helloworld

tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
if [ ${tpid} ]; then
    echo 'Stop Process...'
    kill -15 $tpid
fi
sleep 2
tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
if [ ${tpid} ]; then
    echo 'Kill Process!'
    kill -9 $tpid
else
    echo 'Stop Success!'
fi

echo 准备从Git仓库拉取最新代码
cd /usr/local/helloworld

echo 开始从Git仓库拉取最新代码
git pull
echo 代码拉取完成

echo 开始打包
output=`mvn clean package -Dmaven.test.skip=true`

cd target

echo 启动项目
nohup java -jar helloworld-1.0-SNAPSHOT.jar &> helloworld.log &
echo 项目启动完成

```



![image-20220504160851570](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041608633.png)

同时我们在使用shell脚本时，我们还需要进行权限设置；

![image-20220504160959892](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041610002.png)

同时与有我们的现在处于的ip为共享的，有时在没有连网下我们的虚拟机就不会给我们提供一个ip地址，这样我们就需要设置一些静态的ip供我们使用。

![image-20220504162220175](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041622289.png)

> 同时修改文件的不是固定死的，但是文件位置是的。这样我们就需要到 `/etc/sysconfig/network-scripts`下去找一下

![image-20220504163449321](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041634430.png)

> 注意：要是这我们的网段一样，那样会导致我们无法进行连接。

![image-20220504163626525](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041636580.png)

