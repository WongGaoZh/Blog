## spring  bean的生命周期
spring bean的生命周期
```
1.spring 容器完成扫描-----class-----beanDefinition---bdmap
2.开始遍历这个map
3.parse bd 解析beanDefinition
4.validate 校验
5.推断构造方法
6.利用构造方法---反射来执行----实例化对象
7.判断这个对象是否需要合并
8.判断这个对象是否需要提前暴露一个工厂?如果需要提前暴露,会缓存一个工厂在map中
9.自动注入---填充属性
10. 回调部分的aware方法 (BeanNameAware的setBeanName方法  还有BeanFactoryAware的setBeanFactory方法 )
11.执行beanPostprocesser的before方法
12.执行部分的生命周期初始化回调方法  ( initializingBeande afterPropertiesSet方法, 还要 init-method方法 )
13.BeanPostProcessor after----aop----事件发布
14.put 单例池
```

spring aop原理: 
+ AOP（面向切面）比如你写了个方法用来做一些事情，但这个事情要求登录用户才能做，你就可以在这个方法执行前验证一下，执行后记录下操作日志
+ 这个类就是切面（Aspect），这个被环绕的方法就是切点（Pointcut），你所做的执行前执行后的这些方法统一叫做增强处理（Advice）。
+ 面试的话，就得装B  所以必须得深化点，你得告诉他，aop实现原理其实是java动态代理，但是jdk的动态代理必须实现接口，所以spring的aop是用cglib这个库实现的，cglib使用了asm
这个直接操纵字节码的框架，所以可以做到不实现接口的情况下完成动态代理。最好拿张纸手写两个例子给他，然后他就没什么好问的了
+ CgLib动态代理--首先要实现MethodInterceptor接口（interceptor是拦截器的意思）