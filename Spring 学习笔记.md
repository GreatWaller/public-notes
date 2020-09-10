# Spring 学习笔记

Spring文档地址https://docs.spring.io/spring/docs/current/spring-framework-reference/index.html

### 一、IoC

#### 1 初识bean

**项目结构**

```
src:.

├─main
│  ├─java
│  │  └─com
│  │      └─cloudbeat
│  │          │  App.java
│  │          │
│  │          └─pojo
│  │                  MyBean.java
│  │
│  └─resources
│          beans.xml
│
└─test
    └─java
        └─com
            └─cloudbeat
                    AppTest.java

```

**引入依赖**

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.8.RELEASE</version>
</dependency>
```



**Bean**

```java
public class MyBean {
    private String name = "My Bean";

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public MyBean() {
    }
    
}
```

**baens.xml**

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="myBean" class="com.cloudbeat.pojo.MyBean"></bean>
</beans>
```

**Test**

```java
@Test
public void beanLoad() {
    XmlBeanFactory mybeanFactory = new XmlBeanFactory(new ClassPathResource("beans.xml"));
    MyBean bean = (MyBean)mybeanFactory.getBean("myBean");
    assertEquals("My Bean", bean.getName());
}
```



#### 2 解析Bean



#### 3 创建Bean



#### 4 循环依赖





### 二、AOP

#### 1 示例

引入 AspectJ’s `aspectjweaver.jar` library

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

AOP Schema

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <aop:aspectj-autoproxy/>
    <!-- bean definitions here -->
    <bean id="myBean" class="com.cloudbeat.pojo.MyBean">
        <constructor-arg>
            <value>Constructor Replacement</value>
        </constructor-arg>
    </bean>

    <bean class="com.cloudbeat.aop.AspectJTest"></bean>

</beans>
```

定义需要切入的方法testAop()

```java
public class MyBean {
    private String name = "My Bean";

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public MyBean() {
    }

    public MyBean(String name) {
        this.name = name;
    }
    
    public void testAop(){
        System.out.println("test AOP...");
    }
}
```

定义切面AspectJTest

```java
package com.cloudbeat.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

@Aspect
public class AspectJTest {
    @Pointcut("execution(* *.testAop(..))")
    public void test() {
    }

    @Before("test()")
    public void beforeTest() {
        System.out.println("before...");
    }

    @After("test()")
    public void afterTest() {
        System.out.println("after...");
    }

    @Around("test()")
    public Object aroundTest(ProceedingJoinPoint p) {
        System.out.println(" around begin");
        Object result = null;
        try {
            result = p.proceed();
        } catch (Throwable e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println("around finished");
        return result;
    }

}
```

> 注意：@Pointcut所标注的切面方法名称可自定义，实际进行aop的before或after等的参数需要与该==切入点==名称一致

测试

```java
@Test
public void testAoph() {
    ApplicationContext bf= new ClassPathXmlApplicationContext("beans.xml");
    MyBean bean=(MyBean) bf.getBean("myBean");
    bean.testAop();
}
```



#### 2 jdk与cglib代理

当需要使用cglib代理和@AspectJ自动代理支持，需要设置proxy-target-class属性

```xml
<aop:aspectj-autoproxy proxy-target-class="true"/>
```

- jdk：其代理的对象必须是某个接口的实现，它是通过运行期间创建一个接口的实现类来完成对目标对象的代理；
- cglib: 原理类似，只是它在运行期间生成的代理对象是针对目标类扩展的子类。cglib是高效的代码生成包，底层是依靠ASM操作字节码实现的，性能比JDK强；