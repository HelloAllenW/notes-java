## 1. Java Web应用的相关概念

1、动态web开发技术统称为Java Web。

2、Web服务器指的是运行及发布web应用的容器，例如Tomcat

3、Servlet是一种动态网页开发技术，除此之外，还有JSP、ASP、PHP



## 2. Tomcat

1、Tomcat的使用

（1）下载解压apache-tomcat-xx

（2）配置一个环境变量：java_home（指向JDK安装的根目录）或jre_home （不需要）

（3）双击apache-tomcat-xx/bin目录下的startup.bat，启动服务器

（4）在浏览器中输入localhost:8080来检验Tomcat安装启动是否成功

（5）执行shutdown.bat来关闭tomcat服务

（6）可以通过修改server.xml文件中的配置信息来修改Tomcat服务端口号

2、Tomcat安装目录介绍

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161020102.png)

3、Tomcat下创建和部署项目

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161020570.png)

4、Tomcat管理程序

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161020442.png)

## 3. Servlet

Servlet是Server Applet的简称，是服务端的程序，可交互式的处理客户端发送到服务端的请求，并完成操作响应。

Servlet作用是：(1)接收客户端请求，完成操作；(2)动态生成网页；(3)将包含操作结果的动态网页响应给客户端。

1、Servlet就是一个Java类，运行在像tomcat之类的Servlet容器中。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161021993.png)

2、Servlet容器为JavaWeb应用提供运行时环境，它负责管理Servlet和JSP的生命周期，以及管理他们的共享数据。Servlet容器也称为JavaWeb应用容器，或者Servlet/JSP容器。目前流行的Servlet容器软件包括Tomcat、Resin、J2EE服务器中也提供了内置的Servlet容器。



## 4. Servlet开发步骤

1、原生开发步骤

（1）搭建开发环境

将tomcat中的lib路径下的servlet-api.jar绝对路径配置到系统环境变量的classpath中（？？？没搞懂这步是在干嘛）

（2）编写Servlet

- 实现javax.servlet.Servlet

- 重写5个主要方法

- 在核心的service()方法中编写输出语句，打印访问结果

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161021199.png)

  （3）部署Servlet

  将编写的Servlet文件编译后生成的class文件，放在Tomcat中的webapps下的myweb的classes中（详见二）

  （4）配置Servlet（Tomcat中）

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161022476.png)

  （5）运行测试

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161022357.png)

  2、IDEA创建web项目

  创建项目窗口，选择JavaEE7，并勾选web application（下左图）。然后项目结构见下右图

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161023024.png)

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161023484.png)

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161023210.png)

  （1）然后手工导入servlet.api.jar文件（来自Tomcat安装包）；

  （2）接下来创建MyServlet，实现Servlet接口，覆盖5个方法；

  （3）最后配置web.xml；

  （4）之后的部署和访问同上。

  有个问题就是每次修改代码后都要重新编译出class文件，然后手动将class文件复制到tomcat安装的相关目录下。这样做太麻烦了，IDEA可以帮助我们自动部署。IDEA可以内部集成Tomcat，然后将Tomcat绑定到我们的当前项目中，这样每次编译后就会自动部署。（具体如何配置Tomcat并绑定到当前项目中，自己去百度或在下面找视频链接）

  - 如何打war包？

  为什么要打war包？刚刚我们是使用IDEA内置的Tomcat完成一个开发过程中的热部署。开发完成后，就需要打成war包方便部署。war包可以直接放入Tomcat的webapps目录中，启动Tomcat后自动解压，即可访问。





## 5. 深入Servlet

  1、GenericServlet 帮我们重写了除了service外的其他四个方法，因此如果继承自它的话，我们就只需要实现一个service方法

  ![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161024160.png)

  2、HttpServlet 是继承GenericServlet的基础上的进一步的扩展。提供将要被子类化以创建适用于web站点的HTTP servlet的抽象类。HTTPServlet的子类至少必须重写一个方法，该方法通常是以下这些方法之一：

  doGet、doPost、doPut、doDelete



  3、Servlet3.0之后使用注解配置Servlet，而代替了之前使用web.xml的配置方法（当然web.xml还是可以继续用的）

  在类名之前添加 @WebServlet("/hello")

  注意：配置Servlet的目的其实就是在配置访问当前类的地址





  来源：

  <https://www.bilibili.com/video/BV1Ga4y1Y7Ah?p=1>