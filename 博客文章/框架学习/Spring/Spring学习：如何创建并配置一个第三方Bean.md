## 一、知识储备
1. 使用 XML 配置
```xml
application.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="aService" class="com.test.learnjava.service.AService">
        <property name="bService" ref="bService" /> <!-- 注入另一个bean -->
		<property name="username" value="test" /> <!-- 注入int、String 等数据类型 -->
    </bean>

    <bean id="bService" class="com.test.learnjava.service.BService" />
</beans>
```

```java
// 创建上下文
ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");

// 获取Bean
AService aService = context.getBean(AService.class);

// 正常调用
aService.test();
```

2. 使用 Annotation（注解）配置
```java
@Component
public class BService {
    ...
}
```
@Component 注解就相当于定义了一个Bean

使用 @Autowired 就相当于把指定类型的Bean注入到指定的字段中。和XML配置相比，@Autowired大幅简化了注入，因为它不但可以写在set()方法上，还可以直接写在字段上，甚至可以写在构造方法中

@ComponentScan 标注的类表明是一个配置类，作用类似一个 .xml 配置文件。但要与 @ComponentScan 一起使用，目的是扫描涉及到的bean


## 二、创建第三方Bean

如果一个Bean不在我们自己的package管理之类，例如ZoneId，如何创建它？

答案是：我们自己在@Configuration类中编写一个Java方法创建并返回它，注意给方法标记一个@Bean注解：

```java

@Configuration
@ComponentScan
public class AppConfig {
    // 创建一个Bean, A是其他package中的
    @Bean
    A createA() {
        return A.of("Z");
    }
}

```

Spring对标记为@Bean的方法只调用一次，因此返回的Bean仍然是单例。