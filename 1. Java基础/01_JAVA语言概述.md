## 1. Java语言的几种技术架构

- JAVAEE 企业版：是为开发企业环境下的应用程序提供的一套解决方案

- JAVASE 标准版：是为开发普通桌面和商务应用程序提供的解决方案

- JAVAME 小型版：是为开发电子消费产品和嵌入式设备提供的解决方案

- JAVA Card：支持一些Java小程序（Applets）运行在小内存设备（如智能卡）上的平台



## 2. JVM虚拟机和JDK、JRE之间的关系

JDK = JRE + 开发工具集（例如Javac编译工具等）

JRE = JVM + Java SE标准类库

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161003034.jpg)

Java介于编译型语言和解释型语言之间。编译型语言如C、C++，代码是直接编译成机器码执行，但是不同的平台（x86、ARM等）CPU的指令集不同，因此，需要编译出每一种平台的对应机器码。解释型语言如Python、Ruby没有这个问题，可以由解释器直接加载源码然后运行，代价是运行效率太低。而Java是将代码编译成一种“**字节码**”，它类似于抽象的CPU指令，然后，**针对不同平台编写虚拟机**，不同平台的虚拟机负责加载字节码并执行，这样就实现了“一次编写，到处运行”的效果。当然，这是针对Java开发者而言。对于虚拟机，需要为每个平台分别开发。**JRE中的JVM就是运行Java字节码的虚拟机**。



## 3. Java语言的环境搭建

### 1. 下载JRE（Java运行环境）

包括Java虚拟机（JVM）和Java程序所需的核心类库等，如果想要运行一个开发好的程序，计算机中只需要安装JRE即可。

### 2. 安装JDK（Java开发工具包）

JDK是提供给Java开发人员使用的，其中包含了Java的开发工具，也包括了JRE。所以安装了JDK，就不用再单独安装JRE了。其中的开发工具：编译工具、打包工具等 简单而言，使用JDK开发完成的Java程序，交给JRE去执行。

### 3. 配置环境变量

### 4. 验证是否成功

[下载JDK及环境变量配置](https://jingyan.baidu.com/article/4b52d702db5982fc5c774bc3.html)



## 4. Java基础知识体系

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161003048.png)