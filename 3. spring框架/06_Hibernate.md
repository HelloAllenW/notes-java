1、什么是Hibernate？

（1）一个框架

（2）一个Java领域的持久化框架。关于持久化的含义如下：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161035555.png)

（3）一个ORM框架

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161035898.png)

（4）流行的ORM框架

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161035955.png)

（5）Hibernate相当于是对Jdbc的底层进一步封装

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161036337.png)

2、Hibernate的安装使用

（1）安装Hibernate插件（Eclipse为例）

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161037590.png)

（2）准备Hibernate环境

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161037299.png)

（3）Hibernate开发步骤

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161037146.png)

（4）hibernate.cfg.xml基本信息配置

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161038980.png)

（5）创建持久化类

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161038027.png)

（6）创建对象关系映射文件

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161039828.png)

（7）通过Hibernate API编写访问数据库的代码

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161039892.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161039546.png)

再学Hibernate

一、认识Hibernate

Hibernate是一款优秀的持久化ORM（Object Relation Mapping）框架。

- 为什么要做持久化和ORM设计？

用JDBC的API编程访问数据库，代码量较大，特别是访问字段较多的表的时候，代码显得繁琐、累赘，容易出错，程序员需要耗费大量的时间、精力去编写具体的数据库访问的SQL语句，还要十分小心其中大量重复的源代码是否有疏漏，并不能集中精力于业务逻辑开发上面。

ORM则建立了Java对象与数据库对象之间的映射关系，程序员不需要编写复杂的SQL语句，直接操作Java对象即可，从而大大降低了代码量，也使程序员更加专注于业务逻辑的实现。

- 持久化和ORM设计同时可以降低耦合度

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161040383.png)