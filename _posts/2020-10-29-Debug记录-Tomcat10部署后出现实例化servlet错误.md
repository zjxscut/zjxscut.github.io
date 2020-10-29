---
layout: post
title:  Debug记录-Tomcat10部署后出现实例化servlet错误
categories: WEB开发
description: Debug记录
keywords: Tomcat
---


## 问题描述：

​		创建一个简单的工程后，代码如下：

```java
@WebServlet(urlPatterns = "/my")
public class Test extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().println("i am fine");
    }
}

```

​		由于用的是Tomcat10，高版本的它只需要使用@WebServlet(urlPatterns = "/my")进行映射即可。然而当部署上去后，却出现了错误：HTTP Status 500-Internal Server Error，报错内容是无法实例化servlet。

​		起初笔者认为可能是哪里配置错误，导致这种不能正确识别映射或不能正常访问文件，弄了很久很久。。。



## 解决方法：

​		网友分析是Tomcat10中的Servlet-api依赖包与Maven导入的Servlet不兼容所致。

​		直接降级到9就好了。

