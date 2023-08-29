## spring三级循环依赖

### 什么是循环依赖

springbean直接或者间接构成循环调用 ： 1-直接依赖，2-间接依赖，3，自我依赖

### spring设计了三级缓存来解决循环依赖问题

一级缓存存放完全初始化的bean，可以直接被使用
二级缓存存放原始bean对象，还没有被赋值，还没有被依赖注入
三级缓存存放的beanfactories，bean工厂的对象，用于生成原始的bean并放入二级缓存

核心是 ： **将bean的实例化和bean的依赖注入分离开来**
二级缓存用来解决循环依赖，第三级缓存用来解决aop代理的循环依赖。

代理类： bean有代理，那么注入的应该是代理对象，有两种办法可以解决这个问题， 
第一种是 不管有没有循环依赖，我都提前创建好代理对象，
第二种情况是 ： 当被其他类注入引用的时候，才创建对象。

spring选择第二种，实现方案是 在三级缓存里面包了一层objectFactory对象，在注入属性时，判断是否被代理，如果代理生成代理类，如果没有被代理，
生成原始的bean对象放入二级缓存里面。

为什么不选择第一种，因为spring的理念是将bean的实例化，和初始化分离开。**核心是延迟加载bean**

总结一下： 二级缓存的核心是 ： 将bean的实例化和bean的依赖注入分离开来，三级缓存的核心是延迟加载bean， 三级缓存存放的是bean的工厂对象，
二级缓存和一级缓存里面存放的是完整的bean对象，还有原始的bean对象


## springboot的启动流程或者说怎么发现tomcat
+ 1-创建IOC容器
+ 2-加载源配置类，也就是被@springbootApplication修饰的类
+ 3-加载和处理所有的配置类（依赖注入和自动装配就在这一步）
  @SpringbootApplication会引用@import注解会调用到autoconfigurationnImportSelector中加载处理配置类
  那么如何实现该功能： 
  通过springfactories机制，是java spi机制的延伸和扩展 ，Java内置的SPI通过java.util.ServiceLoader类解析classPath和jar包的META-INF/services/目录 下的以接口全限定名命名的文件，并加载该文件中指定的接口实现类，以此完成调用。
  通过classLoader读取classpath中读取到所有jar包的里面的配置文件，/meta-inf/spring/Factories然后解析出来对应的vaules值
+ tomcat为例子，它就存在于autoconfig一个jar包下吗的/meta-inf/spring/Factories，打开看一下就知道了
+ 4-实力化所有的单例bean
+ 5-启动web服务器