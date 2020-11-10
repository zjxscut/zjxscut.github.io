---
layout: post
title:  Spring IOC
categories: Spring
description: Spring
keywords: Spring
---

### 什么是IOC？

​		IOC（Inversion of Control）即控制反转，将对象所需要的实例，从外部实例传入，以此降低耦合。Spring使用IOC可以管理对象创建与对象之间的调用过程。（一般通过工厂模式生成需要的实例）

​		Spring中提供IOC容器实现的两种方式：（解析xml）

​			1）BeanFactory：基本实现，Spring中内部的使用接口

​					加载配置文件时不会创建对象，等真正创建对象实例的时候（getBean()）才创建

​			2）ApplicationContext：提供更强大功能

​					加载配置文件就创建对象

### 1、基于xml方式

#### 1.1、创建对象

```xml
<bean id="" class=""></bean>
```

id：唯一表示

class：类全路径

创建对象的时候默认使用无参构造器

#### 1.2、基于xml方式注入属性

1）DI：依赖注入，注入属性

​	两种方式：set注入和有参构造进行注入

​	set注入：

```xml
<bean id="user" class="com.beanTest.User">
    <property name="id" value="123"></property>
    <property name="name" value="zjx"></property>
</bean>
```

有参构造注入：

```xml
<bean id="user" class="com.beanTest.User">
	<constructor-arg name="id" value="23"></constructor-arg>
	<constructor-arg name="name" value="ajj"></constructor-arg>
</bean>
```

```java
BeanFactory context = new ClassPathXmlApplicationContext("bean3.xml");
User u = context.getBean("user", User.class);
```



#### 1.3、p名称空间注入

​	在beans标签中加入：

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

注入：

```xml
<bean id="user" class="com.beanTest.User" p:name="aaa" p:id="123"></bean>
```

#### 1.4、Bean管理-注入属性

注入空值：

```xml
<bean id="user" class="com.beanTest.User">
    <property name="id"><null/></property>
</bean>
```

value中带特殊符号：

使用转义：

```xml
<!--<property name="name" value="<zjx>"></property>-->
<property name="name" value="&lt;&gt;<zjx>"></property>
```

使用CDATA：

```xml
<property name="name">
    <value><![CDATA[<zjx>]]></value>
</property>
```

注入属性-外部Bean

```xml
<bean id="useuser" class="com.beanTest.UseUser">
	<property name="u" ref="user" ></property><!--通过set方法注入-->
</bean>
<bean id="user" class="com.beanTest.User" p:id="1" p:name="zjx"></bean>
```

注入属性-内部Bean：

```xml
<bean id="useuser" class="com.beanTest.UseUser">
	<property name="u">
		<bean id="user" class="com.beanTest.User">
			<property name="name" value="zjx"></property>
			<property name="id" value="1234"></property>
		</bean>
	</property>
</bean>
```

注入属性-级联赋值：

```xml
<bean id="useuser" class="com.beanTest.UseUser">
	<property name="u" ref="user"></property>
</bean>
<bean id="user" class="com.beanTest.User">
	<property name="id" value="12"></property>
	<property name="name" value="lll"></property>
</bean>
```

#### 1.5、Bean管理-注入集合属性

1）注入数组类型属性

```xml
<bean id="stu" class="com.beanTest.Stu">
    <property name="strs">
        <array>
            <value>123</value>
            <value>456</value>
        </array>
    </property>
</bean>
```

2）注入List集合类型属性

```xml
<bean id="stu" class="com.beanTest.Stu">
    <property name="lists">
        <list>
            <value>zjx</value>
            <value>345</value>
        </list>
    </property>
</bean>
```

3）注入Map集合类型属性

```xml
<bean id="stu" class="com.beanTest.Stu">
    <property name="maps">
        <map>
            <entry key="1" value="2"></entry>
            <entry key="2" value="3"></entry>
        </map>
    </property>
</bean>
```

4）注入Set集合类型属性

```xml
<bean id="stu" class="com.beanTest.Stu">
    <property name="sets">
        <set>
            <value>set1</value>
            <value>set2</value>
        </set>
    </property>
</bean>
```

5）注入对象集合类型属性

```xml
<bean id="stu" class="com.beanTest.Stu">
    <property name="courses">
        <list>
            <ref bean="c1"></ref>
            <ref bean="c2"></ref>
        </list>
    </property>
</bean>
<bean id="c1" class="com.beanTest.Course">
    <property name="cname" value="Chinese"></property>
</bean>
<bean id="c2" class="com.beanTest.Course">
    <property name="cname" value="English"></property>
</bean>
```

6）使用util标签完成list注入

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       					   http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util 		
                           http://www.springframework.org/schema/util/spring-util.xsd">
<util:list id="li">
    <value>123</value>
    <value>456</value>
    <value>789</value>
</util:list>
<bean id="stu" class="com.beanTest.Stu">
    <property name="lists" ref="li"></property>
</bean>
```

#### 1.6、Bean管理-工厂bean

​	普通Bean：定义的类型和返回类型一样

​	工厂Bean：返回类型不一定等于定义类型

```java
public class MyBean implements FactoryBean<Course> {	// 实现FactoryBean接口
    @Override	// 重写三个方法
    public Course getObject() throws Exception {
        Course course = new Course("English");
        return course;
    }
    @Override
    public Class<?> getObjectType() {
        return null;
    }
    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

​	Spring中建立bean对象默认单实例对象，即context.getBean返回的是同一个对象。可以在xml中配置时通过scope进行改变。

​	scope默认值是singleton，其他值如下：

​		prototype：实现多例的创建

​		request：表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP request内有效

​		session：session作用域表示该针对每一次HTTP请求都会产生一个新的bean，同时该bean仅在当前HTTP session内有效

```xml
<bean id="stu" class="com.beanTest.Stu" scope="prototype"></bean>
```

#### 1.7、Bean管理-生命周期

​		从对象创建到销毁的过程

1）通过构造器创建bean实例（无参构造）

2）为bean的属性设置值和对其他bean引用（set方法）

3）调用bean的初始化方法（配置）

4）bean正常使用（获取到bean）

5）容器关闭的时候，调用销毁方法

```java
@Test
void life() {
    BeanFactory context = new ClassPathXmlApplicationContext("bean4.xml");
    Orders u = context.getBean("orders", Orders.class);
    System.out.println("4.use bean");
    ((ClassPathXmlApplicationContext)context).close();
}
```

```java
public class Orders {
    private String name;
    
    public Orders() {
        System.out.println("1.contructor");
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        System.out.println("2.set");
        this.name = name;
    }
    public void initMethod() {
        System.out.println("3.initMethod");
    }
    public void destoryMethod() {
        System.out.println("5.destoryMethod");
    }
}
```

```xml
<bean id="orders" class="com.beanTest.Orders" init-method="initMethod" destroy-method="destoryMethod">
    <property name="name" value="zjx"></property>
</bean>
```

使用后置处理器后，有七步：

1）通过构造器创建bean实例（无参构造）

2）为bean的属性设置值和对其他bean引用（set方法）

3）把bean实例传递bean后置处理器的方法

4）调用bean的初始化方法（配置）

5）把bean实例传递bean后置处理器的方法

6）bean正常使用（获取到bean）

7）容器关闭的时候，调用销毁方法

```java
public class MyBeanPost implements BeanPostProcessor {// 实现BeanPostProcessor
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("before init");
        return null;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("after init");
        return null;
    }
}
```

```xml
<!--在配置文件中添加后，会在每个bean中添加后置处理器-->
<bean id="myBeanPost" class="com.beanTest.MyBeanPost"></bean>
```

#### 1.8、Bean管理-自动注入

xml配置文件中\<bean\>标签下拥有的属性autowire可以实现自动注入，两个属性值：

​	byName：需要bean的id名字和bean中名字一致才可以实现

​	byType：需要只有一个同类型实例，否则它无法自动找到对应的bean进行注入

```xml
<bean id="emp" class="com.beanTest.Emp" autowire="byType"></bean>
<bean id="course" class="com.beanTest.Course">
    <property name="cname" value="Chinese"></property>
</bean>
```

#### 1.9、Bean管理-外部属性文件

数据库配置可以使用阿里出品的druid包

### 2、基于注解方式

#### 2.1、提供的注解

@Component、@Service、@Controller、@Repository功能一样，都可以创建bean实例

#### 2.2、实现注解

1）引入AOP依赖

2）开启组件扫描

3）写bean类，并注解

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.dao"></context:component-scan>
</beans>
```

在bean上注解

```java
@Component(value = "userService") // <bead id="userService" class="...">
```

#### 2.3、基于注解方式实现属性注入

@Autowired：根据属性类型自动注入（针对对象）

@Qualifier：根据属性名称自动注入（针对对象）

@Resource：可根据类型或名称注入（针对对象）

@Value：注入普通类型属性 

#### 2.4、完全注解开发

1）创建配置类代替配置文件

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"com.dao"})
public class Config {
}
```

2）编写测试类

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan(basePackages = {"com.dao"})
public class Config {
}
```

