# 

## 什么是Spring框架,Spring框架都有哪些主要的模块
- 服务网关 (zuul)
- 服务发现 (eurkka)
- 服务通信 (ribbin+fegin+hystrix)
- 链路追踪 (sleuth+zipkin+kafka)
- 配置中心 (config)


### 服务网关
服务网关是微服务架构中一个不可或缺的部分。通过服务网关统一向外系统提供REST API的过程中，除了具备服务路由、均衡负载功能之外，它还具备了权限控制等功能。Spring Cloud Netflix中的Zuul就担任了这样的一个角色，为微服务架构提供了前门保护的作用，同时将权限控制这些较重的非业务逻辑内容迁移到服务路由层面，使得服务集群主体能够具备更高的可复用性和可测试性。
Netflix Zuul   对比 spring cloud gateway


### 服务发现
Netflix Eureka   ---- eureka停止开发了,  替代方案:consul


### 客服端负载均衡
Netflix Ribbon 

### 断路器
Netflix Hystrix 


### 分布式配置
——Spring Cloud Config  + spring cloud bus  等于 apollo配置中心

### 分布式链路追踪 -
sleuth+zipkin+kafka(RabbitMQ)

### 