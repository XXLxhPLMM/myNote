# Linux的基础操作

​	常用命名：

1.getconf LONG_BIT:可以查看我们安装的虚拟机的版本

2.ifconfig：可以查看虚拟机的ip地址以及其它网关信息

​	我们有时需要把本机的文件传到虚拟机里面，这里就需要我们使用Xshell7连接虚拟机和Xftp7传输文件。



> Linux目录结构

![image-20220426162116731](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261621881.png)

 **其中在Linux中的  / 表示根目录**。

---

##  cd  ls  ll mkdir rmkir

![image-20220426185046458](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261850504.png)

![image-20220426185028496](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261850623.png)

## cat  more less 

![image-20220426185433360](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261854429.png)

## rm cp mv

![image-20220426185759947](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261858034.png)

## 打包或解压 tar

![image-20220426190045886](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261900961.png)

进行打包：

![image-20220426191441930](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261914090.png)

![image-20220426191925718](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261919919.png)

![image-20220426192112461](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261921499.png)

进行解压：

![image-20220426192316488](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261923555.png)



## 查找文件find和grep以及查看下载软件rpm

![image-20220426190504461](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261905533.png)

![image-20220504132759442](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041327502.png)

这样可以看到我们已经下载好软件：要是我们想卸载软件可以使用 rpm -额--nodeps 软件名称：

![image-20220504133303465](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041333501.png)

## pwd  touch  clear

![image-20220426192937202](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261929258.png)

> ctrl+l  也可以进行清屏

## 重定向  >（进行覆盖）和 >>（进行追加）

![image-20220426193624584](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261936626.png)

![image-20220426193903916](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261939963.png)

## 关闭进行 kill

![image-20220426194021703](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261940740.png)

## 管道 |  结合着ps -ef

ps -ef可以进行查看我们的系统中全部的进程

![image-20220426194222721](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261942818.png)

## 资源重新加载 source

我们在进行修改一些配置后，它并不会立即起效，这样需要我们进行重新加载配置资源这样，修改后的配置会起效，这个命令常用在我们在进行安装软件时，并且还需要进行配置环境变量，经常用到：

例如：安装好JDK1.8；并在 /etc/profile 配置好环境变量后使用使用source /etc/profile

![image-20220504123645386](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202205041236428.png)

---



# Linux的权限

![image-20220426194703950](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261947996.png)

1           3         3        3   （对应着下图中的  -  ---  ---  ---）

![image-20220426194829948](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261948021.png)

234，他们都有读写执行的权限；

![image-20220426195351566](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261953640.png)

这样我们在通过修改文件的权限就可以使用数据来代替  u=rwx； g= rwx ；o=rwx；

## 权限修改 chmod 777 xxx  

![image-20220426195643402](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204261956465.png)

---

# Linux上常用网络操作

## 1、主机名配置 hostname

![image-20220426200421425](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262004461.png)

![image-20220426200806818](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262008940.png)

## 2、ip地址  ifconfig

![image-20220426201026187](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262010247.png)

## 3、域名映射

![image-20220426202805562](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262028613.png)

在进入/etc/hosts（域名解析文件）文件后：

![image-20220426202859917](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262028972.png)

通过ping 进行查看：

![image-20220426202937715](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262029867.png)

## 4、网络服务器管理

![image-20220426203040747](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202204262030805.png)

