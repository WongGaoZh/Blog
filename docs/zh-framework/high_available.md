## 高可用

### 容灾备份
	容灾备份 ：如何备份，不同的框架有不用的玩法 。 除了 Redis 中间件外，其他常见的 MySQL、Kafka 消息中间件、HBase 、ES 等 ，
    凡是涉及到数据存储的介质，都有备份机制，一旦主节点挂了，会启用备份节点，保证数据不会丢失。

### 多活策略
	多活策略 ：常见的多活方案有，同城双活、两地三中心、三地五中心、异地双活、异地多活

### 限流
	限流：
		限流支持多个维度：
		
		整个系统一定时间内（比如每分钟）处理多少请求
		单个接口一定时间内处理多少流量
		单个IP、城市、渠道、设备id、用户id等在一定时间内发送的请求数
		如果是开放平台，则为每个appkey设置独立的访问速率规则
		常见的限流算法：
		
		计数器限流
		滑动窗口限流
		漏桶限流
		令牌桶限流
		
		现成的轮子使用，其实现也是用了上述我们所说的限流算法。
		比如Google Guava 提供的限流工具类 RateLimiter，是基于令牌桶实现的，并且扩展了算法，支持预热功能。
		阿里开源的限流框架Sentinel 中的匀速排队限流策略，就采用了漏桶算法。
		Nginx 中的限流模块 limit_req_zone，采用了漏桶算法，还有 OpenResty 中的 resty.limit.req库等等
		
### 熔断
	熔断： 
		熔断，其实是对调用链路中某个资源出现不稳定状态时（如：调用超时或异常比例升高），对这个资源的调用进行限制，让请求快速失败，避免影响到其它的资源而导致级联错误。
		
### 降级
	降级 ： 
		降级是通过暂时关闭某些非核心服务或者组件从而保护核心系统的可用性。
		正如 “好钢用在刀刃上”，为了使有限资源发挥最大价值，我们会临时关闭一些非核心功能，减轻系统压力，并将有限资源留给核心业务。
		
		比如电商大促，业务在峰值时刻，系统抵挡不住全部的流量时，系统的负载、CPU 的使用率都超过了预警水位，可以对一些非核心的功能进行降级，降低系统压力，比如把商品评价、成交记录等功能临时关掉。弃车保帅，保证 创建订单、订单支付 等核心功能的正常使用。
### 告警		
	告警 ： 
		多渠道告警
		高可用不只是内部没有单点，可用性需要考虑到整个业务流程上的每一个环节。如果消息通知渠道同时也挂了怎么办呢？
		我们的服务可能会挂，微信也可能会挂，邮件服务也可能会挂，虽然它们同时挂掉的可能性极低。
		监控系统的末端——消息通知渠道，也应该不是单点。解决方案一般是在AlertManager配置多渠道冗余告警，来保障告警消息的可达性。
### 灰度
	灰度 ： 
		蓝绿发布 ： 有一台热备机器，我们做蓝绿发布，
		灰度发布 ： 在流量网关配置多少占比的值到灰度服务里面去，0%的流量，30%流量，70%流量，运行一个周期
		全链路灰度 ：
		物理环境隔离 ： 两个服务同时上线做灰度
		逻辑环境隔离 ： 节省大量的机器成本和运维人力，但是需要解决以下问题
			1. 组件或者服务能够通过流量特征进行动态路由
			2. 对服务下所有节点进行打标签
			3. 对流量进行灰度标识、版本标识
			4. 识别出不同版本的灰度流量
			
		解决办法 ： 
			1. 标签路由 ： 标签路由通过对服务下所有节点按照标签名和标签值不同进分组
			2. 节点打标 ：（标签路由下如何进行节点打标，使用Kubernetes Service 作为服务发现的业务系统，可以使用serivce下的pod资源的Labels数据， 那么另一种情况使用Nacos 作为服务发现的业务系统，希望为节点添加版本灰度标，那么为业务容器添加`spring.cloud.nacos.discovery.metadata.version=gray`，）
			3. 流量染色 ： 请求链路上各个组件如何识别出不同的灰度流量？答案就是流量染色，前端染色，微服务网关染色，
			4. 分布式链路追踪 ： 如何保证灰度标识能够在链路中直传递下去， 
			核思想就是通过个全局唯的 traceid 和每条的 spanid 来记录请求链路所经过的节点以及请求耗时，其中 traceid 是需要整个链路传递的。借助于分布式链路追踪思想，我们也可以传递些定义信息，如灰度标识。