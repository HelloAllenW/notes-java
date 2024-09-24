\> 相比于JavaWeb中使用servlet作为控制器，Struts2使用filter代替MVC中的控制器C。使用filter过滤器来作为控制器，可以方便地在应用程序里对所有资源（包括静态资源）进行控制访问。



1、Struts2概述

Struts2是一个用来开发MVC应用程序的框架，它提供了Web应用程序开发过程中的一些常见问题的解决方案：

- 对来自用户的输入数据进行合法性验证
- 统一的布局
- 可扩展性
- 国际化和本地化
- 支持Ajax
- 表单的重复提交
- 文件的上传下载

Struts2 VS Struts1相比在体系结构方面更加优秀：

- 类更少 更高效：在Struts2中无需使用“ActionForm”来封装请求参数
- 扩展更容易：Struts2通过拦截器完成了框架的大部分工作，在Struts2中插入拦截器对象相当简便易行
- 更容易测试：即使不使用浏览器也可以对基于Struts2的应用进行测试



2、HelloWorld

（1）需求说明：首页index.jsp，点击后进入input.jsp然后输入表单点击提交，提交成功后到detail.jsp展示刚刚提交内容。

（2）目录结构

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161025482.png)

（3）下载Struts2并搭建Struts开发环境

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161026851.png)

关联当前文件的约束文档，以保证写代码时有提醒：

a. 添加dtd地址

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161026297.png)

b. 添加安装目录dtd和key type

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161027912.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161028670.png)

（4）struts.xml

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161028735.png)

（5）Product.java：是一个model，同时也是一个Action类

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161028801.png)

product.java中除了一些属性以及其get和set方法，还包括下面：不带参的构造器、供struts执行action时调用的方法、多个action方法

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161029841.png)

3、在Action中访问web资源

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161029605.png)

 \> 使用ServletActionContext

​     \> 实现ServletXxxAware接口



（1）解耦 - ActionContext 的使用：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161030891.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161030973.png)

（2）解耦 - XxxAware接口：定义一个成员变量即可

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161031684.png)

（3）耦合 - ServletActionContext

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161031561.png)

（4）耦合 - ServletXxxAware

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161031589.png)

4、ActionSupport

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161032606.png)

除此之外ActionSupport还实现了其他一些接口，因此在手工完成字段验证、显示错误信息、国际化等情况下，推荐继承ActionSupport





5、关于Struts2后续知识点的学习：

<https://www.bilibili.com/video/BV1zE41197bw?p=235>







------





再学Struts2：搭建Struts2工程

1、引入核心jar包

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161032085.png)

2、web.xml中配置入口

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161033923.png)

3、实现Action，其中的execute是专门用来处理请求的（类似于HttpServlet中的doGet、doPost）

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161033387.png)

4、配置struts.xml。其中name是请求的地址，class是请求的Action类，result是对应的请求结果

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161033039.png)

来源：<https://www.bilibili.com/video/BV1KE411P74D?p=1>