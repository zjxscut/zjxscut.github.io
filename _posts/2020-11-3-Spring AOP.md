---
layout: post
title:  Spring AOP
categories: Spring
description: Spring
keywords: Spring
---

### 什么是AOP？

Aspect Oriented Programming，面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。

### 1、AOP底层使用动态代理

#### 1.1、有接口，使用JDK动态代理

​	1）有接口，创建接口实现类的代理对象，调用newProxyInstance方法

定义一个接口：

```java
public interface TestInterface {
    int add(int a, int b);
    void update(int val);
}
```

实现该接口：

```java
public class TestClass implements TestInterface {
    @Override
    public int add(int a, int b) {
        return a+b;
    }

    @Override
    public void update(int val) {
        System.out.println("Update th val:" + val);
    }
}
```

创建一个代理类，实现InvocationHandler接口：

```java
public class ProxyClass implements InvocationHandler {
    TestClass testClass;

    public ProxyClass(TestClass testClass) {
        this.testClass = testClass;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("before");
        Object res = method.invoke(testClass, args);
        System.out.println("after");
        return res;
    }
}
```

在测试类中写如下代码：

```java
@Test
void proxyTest() {
    Class[] interfaces = {TestInterface.class};
    TestClass testClass = new TestClass();
    TestInterface t = (TestInterface) Proxy.newProxyInstance(TestClass.class.getClassLoader(), interfaces, new ProxyClass(testClass));
    int a = t.add(1,2);
    System.out.println("result : " + a);
}
```

结果如下：

```text
before
after
result : 3
```



#### 1.2、没接口，使用CGLIB代理

​	1）创建当前类子类的代理对象



### 2、AOP术语

#### 2.1、连接点

类里面可以被增强的方法称为连接点

#### 2.2、切入点

实际被真正增强的方法是切入点

#### 2.3、通知（增强）

1）实际增强的逻辑部分成为通知

2）通知可以有多种类型：

​	前置通知、后置通知、环绕通知、异常通知、最终通知

#### 2.4、切面 

切面是一个动作，把通知应用到切入点的过程称为切面



### 3、AOP操作

#### 3.1、AspectJ

不是Spring的组成部分，是一个独立AOP框架

#### 3.2、引入相关依赖

需要导入aspectj.weaver、sf.cglib、aopalliance、aspects四个依赖

#### 3.3、切入点表达式

1）作用是知道对哪个类里面的哪个方法进行增强

2）语法结构：

execution(\[权限修饰符\]\[返回类型\]\[类全路径\]\[方法名称\](参数列表))

如：对com.dao.BookDao里面的add进行增强：

execution(* com.dao.BookDao.add(..))

如：对com.dao.BookDao里面的所有方法进行增强：

execution(* com.dao.BookDao.*(..))

#### 3.4、基于AspectJ实现AOP操作

##### （1）基于xml配置文件实现

​	1）创建两个类

```java
public class AOPUser {
    public void add() {
        System.out.println("add ....");
    }
}
```

```java
public class AOPUserProxy {
    public void before() {
        System.out.println("before ...");
    }
}
```

​	2）spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--  开启注解扫描  -->
    <context:component-scan base-package="com.aopano"></context:component-scan>

    <!--  开启AspectJ生成代理对象  -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

​	3）添加注解

```java
import org.springframework.stereotype.Component;

@Component(value = "aopuser")
public class AOPUser {
    public void add() {
        System.out.println("add ....");
    }
}
```

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component(value = "aopuserproxy")
@Aspect
public class AOPUserProxy {
    @Before(value = "execution(* com.aopano.AOPUser.add(..))")
    public void before() {
        System.out.println("before ...");
    }
}
```

​	4）测试

```java
@Test
void AOPtest() {
    ApplicationContext context = new ClassPathXmlApplicationContext("aopbean1.xml");
    AOPUser aopUser = context.getBean("aopuser", AOPUser.class);
    aopUser.add();
}
```

​	5）使用切入点

spring-aspects：5.2.10

aspectjwweaver：1.9.6

```java
@Pointcut(value = "execution(* com.aopano.AOPUser.add(..))")
public void pointdemo() {}
```

使用切入点：

```java
@Before(value = "pointdemo()")
public void before() {
   System.out.println("before ...");
}
```



##### （2）基于注解方式进行实现