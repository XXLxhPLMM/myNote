# 使用JMeter来进行测试高并发

​	在使用JMeter时我们在进行入JMeter页面时首页是我们进行

![image-20220613205113845](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132051964.png)

上图里面的我们只需在name栏下进行输入进行test的名称，可写可不写

![image-20220613205237356](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132052390.png)

![image-20220613210802332](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132108373.png)

在点击右键进行添加，add->Threads(Users)->Thread Group 下进行添加线程组：

![image-20220613205511517](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132055581.png)

![image-20220613210843683](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132108722.png)

 参数说明：

1、线程数：模拟请求的次数，开启多少个线程进行请求；

2、Ramp-Up时间（秒）：所有线程执行的时间，比如：配置了 2 秒，线程数配置了 100，要在 2 秒内执行 100 次请求；

3、循环次数：要轮询执行几次；

4、调度器

（1）持续时间（秒）：就是本次执行的线程持续多长时间；

（2）启动延迟（秒）：本次执行的线程启动要延迟多长时间执行；

在添加 add->Sampler->HTTP Request完成后，我们进行添加我们需要requst请求：

![image-20220613205645291](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132056330.png)

![image-20220613211028856](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132110897.png)

这样我们就能进行入下图的页面，进行添加我们需要的信息：

![image-20220613210103532](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132101577.png)

![image-20220613211104438](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132111472.png)

在完成上图步骤后，我们还需要进行添加请求头里面的信息，为了保障我们在进行test时避免出现错误（Content type 'text/plain;charset=UTF-8' not supported"）：

![image-20220613210156395](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132101433.png)

![image-20220613211203903](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132112960.png)

在进行这个HTTP信息头管理器，中的下面添加： Content-Type：application/json;charset=UTF-8两个信息。

![image-20220613210357942](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132103985.png)

在一起处理好，我们还需要进行配置监听器，可以添加 汇总报告、聚合报告、察看结果树![image-20220613210615384](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132106426.png)

![image-20220613211259870](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132112952.png)

![image-20220613211620100](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132116156.png)

![image-20220613211635935](https://qinfeng-typora-img.oss-cn-chengdu.aliyuncs.com/img/202206132116984.png)

几个重要的列表参数解释说明：

样本：表示测试中一共发出去了多少个请求

平均值：平均响应时间，默认是单个request响应时间
最小值：最短响应时间
最大值：最大响应时间
异常 %：请求失败率
吞吐量：吞吐量——默认情况下表示每秒完成的请求数（Request per Second）
接受 KB/sec：每秒钟服务器端接收请求量
发送 KB/sec：每秒钟客户端发送请求数量
