## 1. Docker简介

Docker是一个开源的应用容器引擎。

Docker支持将软件编译成一个镜像；然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像。

（类似于我们装系统时，系统里面已经配置好了驱动和各类常用软件）。

运行中的这个镜像称为容器，容器启动时非常快速的。



## 2. Docker核心概念

docker主机（Host）：安装了Docker程序的机器（Docker直接安装在操作系统上）

docker客户端（Client）：连接docker主机并进行操作

docker镜像（Images）：软件打包好的镜像（比如mysql镜像、tomcat镜像），放在docker仓库中。docker镜像是用于创建docker容器的模板

docker仓库（Registry）：用来保存各种打包好的软件镜像

docker容器（Container）：镜像（比如mysql镜像）启动后的实例称为一个容器；容器是独立运行的一个或一组应用



## 3. Docker使用步骤

（1）安装Docker

（2）去Docker仓库找到这个软件对应的镜像

（3）使用Docker运行这个镜像，这个镜像就会生成一个Docker容器

（4）对容器的启动停止就是对软件的启动停止



## 4. 在Linux上安装Docker

（1）查看centos版本：`uname -r`  （Docker要求CentOS系统的内核版本高于3.1）

升级软件包及内核：`yum update`

（2）安装docker：`yum install docker`

（3）启动docker：`systemctl start docker`

（4）将docker服务设为开机启动：`systemctl enable docker`

（5）停止docker： `systemctl stop docker`

（6）更换镜像源（可选）：`sudo vim /etc/docker/daemon.json`

```
{

    "registry-mirrors"：["https://m9r2r2uj.mirror.aliyuncs.com"]

}
```

（7）其他命令：

查看Linux的IP： `ip addr`

重启docker：`systemctl restart docker`



## 5. 在Docker上的常用操作

（1）镜像操作

| 操作 | 命令                                        | 说明                                                    |
| ---- | ------------------------------------------- | ------------------------------------------------------- |
| 检索 | docker search 关键字eg: docker search redis | 我们经常去docker hub上检索镜像的详细信息，如镜像的TAG   |
| 拉取 | docker pull 镜像名:tag                      | :tag是可选的，tag表示标签，多为软件的版本，默认是latest |
| 列表 | docker images                               | 查看所有本地镜像                                        |
| 删除 | docker rmi image-id                         | 删除指定的本地镜像（传入的是镜像ID）                    |

（2）容器操作

软件镜像（QQ安装程序）-> 运行镜像 -> 产生一个容器（正在运行的软件，运行的QQ）

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161121679.png)



## 6. 实战

> 记录自己使用docker过程

安装Java （Dockerfile文件中已经有命令安装java，所以一般部署时不需要单独安装java）

1. 拉取java镜像，默认获取latest的镜像

`docker pull java`

2. 查看刚刚下载的镜像

`docker images`

3. 启动java镜像容器

`docker run -d -it --name java java`

run：启动一个镜像容器

-d：指定容器运行于后台

-it：-i 和 -t 的缩写；

\* -i：以交互模式运行容器，通常与 -t 同时使用

\* -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用

–name：指定容器名字，后续可以通过名字进行容器管理

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161122702.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161122121.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161122963.png)

构建项目镜像，项目打jar包然后上传：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161123214.png)

[Dockerfile文件详解 ](https://www.cnblogs.com/panwenbin-logs/p/8007348.html)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161123447.png)

-t 后面是指定此镜像的tag

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161124958.png)

使用attach进入容器，执行命令：docker attach 容器ID或别名

查看Docker容器中的日志信息， docker logs 容器名称



参考文档：

[Docker 入门教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[docker 和传统虚拟机有什么区别？](https://mp.weixin.qq.com/s/Yk30idFtZNkT_uDu-CKNLQ)

[Docker 是什么？ 和 k8s 之间是什么关系？](https://mp.weixin.qq.com/s/_ldWjMNgyAsglGexSbsqEw)