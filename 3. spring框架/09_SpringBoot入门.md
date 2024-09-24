1、SpringBoot简介

简化Spring应用开发的一个框架；

整个Spring技术栈的一个大整合；

J2EE开发的一站式解决方案；

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161057239.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161057512.png)

扩展介绍：微服务

一个应用应该是一组小型服务，可以通过HTTP的方式进行互通；

每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161057937.png)

2、HelloWorld：

（1）环境准备

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161058619.png)

（2）HelloWorld编写

需求：浏览器发送hello请求，服务器接收请求并处理，相应HelloWorld字符串。

a. 创建Maven项目并导入SpringBoot相关依赖：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161058054.png)

b. 编写启动SpringBoot应用的主程序：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161058356.png)

c. 编写相关的Controller和Service：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161058877.png)

d. 在主程序中运行main方法

e. 打包

在pom.xml中添加依赖，就可以将应用打成一个可执行的jar包；（不用再像之前那样打成war包然后部署到Tomcat上）

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161058542.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059995.png)

可以采用java -jar xxx的命令来直接执行jar包



3、原理分析

（1）pom文件探究

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059264.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059297.png)

（2）主程序（入口）类

主要学习注解的相关依赖： <https://www.bilibili.com/video/BV1Et411Y7tQ?p=6>



4、使用Spring Initializer快速创建Spring Boot项目

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059127.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059984.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161059511.png)



