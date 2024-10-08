<!-- docs/_sidebar.md -->
* [**首页**](zh-cn/)
* [**一 设计模式**](zh-design/)
  * [01、创建型模式总结](zh-design/construction.md)
  * [02、行为型模式总结](zh-design/behavior.md)
  * [03、结构型设计模式](zh-design/structural.md)
* [**二 源码分析**](zh-sound/)   
  - **网络通信和IO**
    * [网络协议](zh-sound/网络协议.md)
    * [NIO,Socket,Netty,RPC都是什么](zh-sound/NIO,Socket,Netty,RPC都是什么.md)
    * [http请求原理](zh-sound/http.md)
    * [BIO和NIO还有AIO的区别](zh-sound/BIO和NIO还有AIO的区别.md)
    * [零拷贝](zh-sound/零拷贝.md)
    * [Netty原理](zh-sound/Netty原理.md)
  - **spring源码分析**
    * [Springboot的原理](zh-sound/springboot.md)
    * [springBean的生命周期](zh-sound/springBean的生命周期.md)
    * [spring三级循环依赖](zh-sound/spring三级循环依赖.md)
    * [springmvc的原理](zh-sound/springmvc的原理.md)
    * [类的加载和实例化区别和在jvm内存的过程](zh-sound/类的加载和实例化区别和在jvm内存的过程.md)
* [**三 工程化工具**](zh-devops/)      
  * [docker](zh-devops/docker.md)
  * [k8s](zh-devops/k8s.md)
  * [kubeadm 搭建集群](zh-devops/kubeadm.md)
  * [shell脚本样例](zh-devops/shell.md)
* [**四 微服务和分布式框架**](zh-spring/)   
  - **微服务框架**
  + [spring全家桶](zh-spring/spring全家桶.md)
  + [spring security 原理](zh-spring/security原理.md)
  + [01_服务网关](zh-spring/01_服务网关.md)
  + [02_服务发现](zh-spring/02_服务发现.md)
  + [03_服务通信](zh-spring/03_服务通信.md)
  + [04_链路追踪](zh-spring/04_链路追踪.md)
  + [05_配置中心](zh-spring/05_配置中心.md)

  - **分布式框架**
  * [50 分布式锁](zh-lock/分布式锁.md)
  * [redis的数据结构](zh-lock/100_redis的数据结构.md)
  * [RocketMQ的原理和实战](zh-lock/RocketMQ.md)
  * [RocketMQ的源码梳理](zh-lock/RocketMQ.md)
  * [Kafka原理和实践](zh-lock/Kafka原理和实践.md)


* [**五 多线程和高并发**](zh-lock/)
  - **多线程和高并发**
  * [10 多线程](zh-lock/多线程.md)
  * [11_synchronized底层原理](zh-lock/synchronized底层原理.md)
  * [12_volatile底层原理](zh-lock/volatile.md)
  * [13_CAS底层原理](zh-lock/CAS.md)
  * [14_AQS底层原理](zh-lock/AQS.md)
  - **多线程实践**
  * [50_多线程调用任务并聚合](zh-lock/CompletableFuture.md)

  
* **六 性能优化**
  + [JVM学习大纲](zh-optimize/10_JVM学习大纲.md)
  + [类加载子系统](zh-optimize/11_JVM_类加载子系统.md)
  + [JVM运行时数据区](zh-optimize/12_运行时数据区.md)
  + [JVM运行时数据区_堆](zh-optimize/12_运行时数据区_堆.md)
  + [JVM运行时数据区_方法区](zh-optimize/12_运行时数据区_方法区.md)
  + [执行引擎](zh-optimize/13_执行引擎.md)
  + [jvm垃圾回收算法](zh-optimize/14_jvm垃圾回收算法.md)
  + [jvm垃圾回收器](zh-optimize/15_jvm垃圾回收器.md)
  + [现场服务器问题排查](zh-optimize/11_现场服务器问题排查方法.md)
  + [Jprofiler使用](zh-optimize/Jprofiler使用.md)
  
* [**七 算法**](zh-algorithm/)   
  + [数据结构](/zh-algorithm/data.md)
  + [排序算法](/zh-algorithm/sort.md)
  

* [**八 数据库**](zh-database/)   
  + **原理篇**
    + [1_mysql原理_mysql底层数据结构](zh-database/1_mysql底层数据结构.md)
    + [2_mysql内部组件](zh-database/2_mysql内部组件.md)
    + [3_Explain工具使用](zh-database/3_explian使用.md)

  + **实战篇**
    + [101_mysql索引实战](zh-database/101_mysql索引实战.md)
    + [102_mysql索引实战2](zh-database/102_mysql索引实战2.md)
    + [103_mysql事务隔离级别](zh-database/103_mysql事务隔离级别.md)
    + [104_mysql_事务隔离级别MVCC机制](zh-database/104_mysql_事务隔离级别MVCC机制.md)
    + [105_mysql日志和刷盘脏页机制](zh-database/105_mysql日志和刷盘脏页机制.md)
    + [199_mysql慢查询](zh-database/199_mysql慢查询.md)
    + [200_docker安装mysql并挂载到宿主机上](zh-database/100_docker安装mysql并挂载到宿主机上.md)
    + [mysql线上问题排查](zh-database/201_mysql线上问题排查.md)
    + [mysql线上问题排查_sharding sphere问题](zh-database/202_mysql线上问题排查2.md)
* [**九 其他**](zh-other/)
  + [01_Json框架选型](zh-other/01_Json框架选型.md)
  + [01_java里面的classpath是什么以及怎么在程序中使用](zh-other/01_java里面的classpath是什么以及怎么在程序中使用.md)
  + [03_数字证书的验证](zh-other/03_数字证书的验证.md)
  + [04_开源协议的选择](zh-other/04_开源协议的选择.md)

  + **其他工具**
    + [mysql命令](zh-other/90_mysql.md)
    + [linux命令1](zh-other/91_linux命令1.md)
    + [linux命令2](zh-other/92_linux命令2.md)
    + [linux命令3](zh-other/93_linux命令3.md)
    + [*** Java易混淆知识点(自看)](zh-other/95_Java易混淆知识点(自看).md)
    + [*** Java开发数据部分笔记](zh-other/96_Java开发数据部分笔记.md)
    + [Git使用](zh-other/97_git.md)
    + [技术选型](zh-other/98_技术选型.md)                       

  - **工具类**
    + [常用网址资料](zh-other/101_常用网址资料.md)
    + [linux服务器安装各种软件](zh-other/102_linux服务器安装各种软件.md)
    + [idea插件使用](zh-other/103_idea插件使用.md)
    + [mac软件安装](zh-other/300_mac软件安装.md)
    
* [**十 架构相关**](zh-framework/)    
  * [高性能](zh-framework/high_performance.md)
  * [高可用](zh-framework/high_available.md)
  * [高扩展](zh-framework/high_scalability.md)
