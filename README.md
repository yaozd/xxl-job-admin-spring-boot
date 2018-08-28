# xxl-job-admin-spring-boot

#### 简介
---
xuxueli的xxl-job-admin默认为war包发布，需要放置到容器中，而spring boot “not war， just run jar！”的口号使我们早已习惯了java -jar 启动web服务的清爽简洁，因此我将xxl-job-admin改造到spring boot框架，公司目前正在使用。

#### 版本
---
1. spring-boot: 1.5.14.RELEASE
2. xxl-job-core: 1.9.1

#### 使用步骤：
---
1. 首先感谢xuxueli的开源奉献。
2. 进入xxl-job的源项目：[https://github.com/xuxueli/xxl-job](https://github.com/xuxueli/xxl-job) ,
   选择1.9.1的tag并下载：[https://github.com/xuxueli/xxl-job/tree/v1.9.1](https://github.com/xuxueli/xxl-job/tree/v1.9.1)
3. 运行 `mvn clean deploy` 安装到仓库中，我们需要xxl-job-core 子模块。
4. 下载本项目
5. 将resources/application-dev.yml中的配置改为自己的配置即可，对应内容可参考xxl-job官方文档: [http://www.xuxueli.com/xxl-job/#/](http://www.xuxueli.com/xxl-job/#/)
6. IDE中直接运行运行 `com.github.xxl.job.admin.Application` 即可.
7. 访问网站：[http://localhost:9000/](http://localhost:9000/)
8. 如有问题请联系: felton321@sina.com

#### 常见问题
---
```
2018-02-08 -by arvin
--------------------
常见问题：
1，
xxl-job-executor-sample-springboot下application.properties文件

### xxl-job admin address list, such as "http://address" or "http://address01,http://address02"
这里实际指的是：admin-web的URL地址：
xxl.job.admin.addresses=http://127.0.0.1:8080/xxl-job-admin //部署在tomcat下的地址
xxl.job.admin.addresses=http://127.0.0.1:7701/ //直接在ideal下开始调试时tomcat下的地址
如果URL地址不正确，可能会报如下错误：
16:48:22.863 logback [Thread-9] INFO  c.x.j.c.t.ExecutorRegistryThread - >>>>>>>>>>> xxl-job registry error, registryParam:RegistryParam{registGroup='EXECUTOR', registryKey='xxl-job-executor-sample', registryValue='192.168.1.203:9999'}
java.lang.RuntimeException: RpcResponse byte[] is null
	at com.xxl.job.core.rpc.netcom.NetComClientProxy$1.invoke(NetComClientProxy.java:65)
	at com.sun.proxy.$Proxy53.registry(Unknown Source)
	at com.xxl.job.core.thread.ExecutorRegistryThread$1.run(ExecutorRegistryThread.java:57)
	at java.lang.Thread.run(Thread.java:745)
解决方案：
xxl.job.admin.addresses=改为正确的URL
====================================
子程序可以使用永久不执行的cron的示例：
0 0 3 1 1 ? 2099
====================================
2，
JobHandler(value="shardingJobHandler")
JobHandler的名字是value的值=shardingJobHandler
3，
Cron表达式在线工具---无广告
http://www.pppet.net/
crontab表达式
https://tool.lu/crontab/
==
在线Cron表达式生成器--不推荐使用
http://cron.qqe2.com/
4，
运行过程中不时报错: java.lang.ClassCastException: java.lang.String cannot be cast to com.xxl.job.core.rpc.codec.RpcResponse #223
https://github.com/xuxueli/xxl-job/issues/223
最新发现：
如果将adminAddresses 设置成 ip+port地址的情况下，这种错误没有发生
在adminAddresses 设置成 域名地址 的情况下，这种错误才会发生，1个小时内不定时发生好几次。。。

5，
参考文章：
分布式任务调度平台XXL-JOB
http://www.xuxueli.com/xxl-job/#/
==
yaozd/xxl-job: A lightweight distributed task scheduling framework.（分布式任务调度平台XXL-JOB）
https://github.com/yaozd/xxl-job
==
分布式任务调度平台XXL-JOB - 许雪里 - 博客园--(许雪里是xxl-job的作者，特别推荐-byArvin)
https://www.cnblogs.com/xuxueli/p/5021979.html
==
运行过程中不时报错: java.lang.ClassCastException: java.lang.String cannot be cast to com.xxl.job.core.rpc.codec.RpcResponse · Issue #223 · xuxueli/xxl-job
https://github.com/xuxueli/xxl-job/issues/223
==
Releases · xuxueli/xxl-job-(稳定版
https://github.com/xuxueli/xxl-job/releases
==
--------------------

三、任务详解
配置属性详细说明：
- 执行器：任务的绑定的执行器，任务触发调度时将会自动发现注册成功的执行器, 实现任务自动发现功能; 另一方面也可以方便的进行任务分组。每个任务必须绑定一个执行器, 可在 "执行器管理" 进行设置;
- 描述：任务的描述信息，便于任务管理；
- 路由策略：当执行器集群部署时，提供丰富的路由策略，包括；
    FIRST（第一个）：固定选择第一个机器；
    LAST（最后一个）：固定选择最后一个机器；
    ROUND（轮询）：；
    RANDOM（随机）：随机选择在线的机器；
    CONSISTENT_HASH（一致性HASH）：每个任务按照Hash算法固定选择某一台机器，且所有任务均匀散列在不同机器上。
    LEAST_FREQUENTLY_USED（最不经常使用）：使用频率最低的机器优先被选举；
    LEAST_RECENTLY_USED（最近最久未使用）：最久为使用的机器优先被选举；
    FAILOVER（故障转移）：按照顺序依次进行心跳检测，第一个心跳检测成功的机器选定为目标执行器并发起调度；
    BUSYOVER（忙碌转移）：按照顺序依次进行空闲检测，第一个空闲检测成功的机器选定为目标执行器并发起调度；
    SHARDING_BROADCAST(分片广播)：广播触发对应集群中所有机器执行一次任务，同时传递分片参数；可根据分片参数开发分片任务；

- Cron：触发任务执行的Cron表达式；
- 运行模式：
    BEAN模式：任务以JobHandler方式维护在执行器端；需要结合 "JobHandler" 属性匹配执行器中任务；
    GLUE模式(Java)：任务以源码方式维护在调度中心；该模式的任务实际上是一段继承自IJobHandler的Java类代码并 "groovy" 源码方式维护，它在执行器项目中运行，可使用@Resource/@Autowire注入执行器里中的其他服务；
    GLUE模式(Shell)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "shell" 脚本；
    GLUE模式(Python)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "python" 脚本；
    GLUE模式(NodeJS)：任务以源码方式维护在调度中心；该模式的任务实际上是一段 "nodejs" 脚本；
- JobHandler：运行模式为 "BEAN模式" 时生效，对应执行器中新开发的JobHandler类“@JobHandler”注解自定义的value值；
- 子任务：每个任务都拥有一个唯一的任务ID(任务ID可以从任务列表获取)，当本任务执行结束并且执行成功时，将会触发子任务ID所对应的任务的一次主动调度。
- 阻塞处理策略：调度过于密集执行器来不及处理时的处理策略；
    单机串行（默认）：调度请求进入单机执行器后，调度请求进入FIFO队列并以串行方式运行；
    丢弃后续调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，本次请求将会被丢弃并标记为失败；
    覆盖之前调度：调度请求进入单机执行器后，发现执行器存在运行的调度任务，将会终止运行中的调度任务并清空队列，然后运行本地调度任务；
- 失败处理策略；调度失败时的处理策略；
    失败告警（默认）：调度失败和执行失败时，都将会触发失败报警，默认会发送报警邮件；
    失败重试：调度失败时，除了进行失败告警之外，将会自动重试一次；注意在执行失败时不会重试，而是根据回调返回值判断是否重试；
- 执行参数：任务执行所需的参数，多个参数时用逗号分隔，任务执行时将会把多个参数转换成数组传入；
- 报警邮件：任务调度失败时邮件通知的邮箱地址，支持配置多邮箱地址，配置多个邮箱地址时用逗号分隔；
- 负责人：任务的负责人；
3.1 BEAN模式
任务逻辑以JobHandler的形式存在于“执行器”所在项目中，开发流程如下：

步骤一：执行器项目中，开发JobHandler：

 - 1、继承"IJobHandler"：“com.xxl.job.core.handler.IJobHandler”；
 - 2、注册到Spring容器：添加“@Component”注解，被Spring容器扫描为Bean实例；
 - 3、注册到执行器工厂：添加“@JobHandler(value="自定义jobhandler名称")”注解，注解value值对应的是调度中心新建任务的JobHandler属性的值。
 - 4、执行日志：需要通过 "XxlJobLogger.log" 打印执行日志；
（可参考Sample示例执行器中的DemoJobHandler，见下图） 
```