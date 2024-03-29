# springboot的原理

## springboot启动流程(***)
1.	创建IOC容器
2.	加载源配置类，被@SpringbootApplication修饰的类
3.	加载并处理所有的配置类（依赖注入和自动装配就属于其中）
4.	实例化所有的单例bean
5.	启动web服务器（比如tomcat）

第三步是核心：
@import注解会调用到autoconfigurationnImportSelector中加载处理自动配置类，
那么如何实现这个呢
通过springfactories机制，是spring spi机制的延伸和扩展
通过classloader读取classpath中读取到所有jar包里面的配置文件/meta-inf/spring/Factories然后解析出来对应的vaules值

## springboot的注解

@SpringBootApplication是一个复合注解: 主要是@SpringBootConfiguration @EnableAutoConfiguration (EnableAutoConfiguration自动配置机制是SpringBoot的核心特色之一。可根据引入的jar包对可能需要的各种机制进进行默认配置。)

@ComponentScan(这是spring-context原来就存在的注释，需要在@Configuration标注的类上标注，用来指示扫描某些包及其子包上的组件。)

@EnableAutoConfiguration真正核心的动作就是通过Import机制加载EnableAutoConfigurationImportSelector.selectImports函数返回的配置类：

``` Java
public String[] selectImports(AnnotationMetadata annotationMetadata) {
		if (!isEnabled(annotationMetadata)) {
			return NO_IMPORTS;
		}
		try {
			AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
					.loadMetadata(this.beanClassLoader);
			AnnotationAttributes attributes = getAttributes(annotationMetadata);
			List<String> configurations = getCandidateConfigurations(annotationMetadata,
					attributes);
			configurations = removeDuplicates(configurations);
			configurations = sort(configurations, autoConfigurationMetadata);
			Set<String> exclusions = getExclusions(annotationMetadata, attributes);
			checkExcludedClasses(configurations, exclusions);
			configurations.removeAll(exclusions);
			configurations = filter(configurations, autoConfigurationMetadata);
			fireAutoConfigurationImportEvents(configurations, exclusions);
			return configurations.toArray(new String[configurations.size()]);
		}
		catch (IOException ex) {
			throw new IllegalStateException(ex);
		}
	}

```
