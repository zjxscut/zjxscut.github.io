---
layout: post
title:  Spring JDBC
categories: Spring
description: Spring
keywords: Spring
---

### 1、Spring JDBC使用

1）导入相应的包

com.alibaba:druid:1.1.9、mysql-connector8.0.18、spring-jdbc、spring-orm5.2.9、spring-tx5.2.9

2）配置JdbcTemplate对象，注入DataSource

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
        <property name="url" value="jdbc:mysql://localhost:3306/user_db?characterEncoding=utf8&amp;useSSL=true&amp;createDatabaseIfNotExist=true&amp;serverTimezone=GMT&amp;nullNamePatternMatchesAll=true"></property>
        <property name="username" value="root"></property>
        <property name="password" value="Zhangjiaxin177"></property>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
</beans>
```

3）创建并注入类：

在dao层注入JdacTemplate：

```java
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;
}
```

在service层注入dao：

```java
@Service
public class BookService {
    @Autowired
    private BookDao bookDao;
}
```

4）根据数据库表创建实体，并在add操作中添加逻辑功能

在Application中添加注解@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})

```java
String sql = "insert into t_book values(?,?,?)";
Object[] args = {book.getUser_id(), book.getUsername(), book.getUstatus()};
int update = jdbcTemplate.update(sql, args);
System.out.println(update);	
```

5）写测试类

```java
@Test
void contextLoads() {
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    BookService bookService = context.getBean("bookService", BookService.class);
    bookService.addBook(new Book("13", "zjx", "normal"));
}
```

6）dao层

```java
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void add(Book book) {
        String sql = "insert into t_book values(?,?,?)";
        Object[] args = {book.getUser_id(), book.getUsername(), book.getUstatus()};
        int update = jdbcTemplate.update(sql, args);
        System.out.println(update);
    }

    @Override
    public void updateBook(Book book) {
        String sql = "update t_book set username=?,ustatus=? where user_id=?";
        Object[] args = { book.getUsername(), book.getUstatus(), book.getUser_id()};
        int update = jdbcTemplate.update(sql, args);
        System.out.println(update);
    }

    @Override
    public void deleteBook(String id) {
        String sql = "delete from t_book where user_id=?";
        int update = jdbcTemplate.update(sql, id);
        System.out.println(update);
    }

    @Override
    public int selectCount() {
        String sql = "select count(*) from t_book";
        Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
        return count;
    }

    @Override
    public Book findBookInfo(String id) {
        String sql = "select * from t_book where user_id=?";
        Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
        return book;
    }

    @Override
    public List<Book> findAllBook() {
        String sql = "select * from t_book";
        List<Book> books = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
        return books;
    }

    @Override
    public void batchAddBook(List<Object[]> batchArgs) {
        String sql = "insert into t_book values(?,?,?)";
        int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
        System.out.println(Arrays.toString(ints));
    }
}
```

