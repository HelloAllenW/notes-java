你是一个程序员，你用代码写了一个博客应用服务，并将它部署在了云平台上。
但应用服务太过受欢迎，访问量太大，经常会挂。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuEPEgGh5DzUuiaHf5VAEsy1atZsFtCcPl8VVN4snRTED3kwuet9bdTdA/640?wx_fmt=jpeg&from=appmsg&wxfrom=13)

所以你用了一些工具自动重启挂掉的应用服务，并且将应用服务部署在了好几个服务器上，总算抗住了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuubKKXsQWEcibcBjUUuHzyMEq5f2jVicP1Q5VK2SXs6pfhib72cOqPpl9A/640?wx_fmt=jpeg&from=appmsg&wxfrom=13)

后来你又上线了商城应用服务和语音应用服务，随着**应用服务变多**，需求也千奇百怪。有的应用服务不希望被外网访问到，有的部署的时候要求内存得大于 xxGB 才能正常跑。
你每次都需要登录到各个服务器上，执行**手动**操作更新。不仅容易出错，还贼**浪费时间**。

**原本就没时间找女朋友的你，现在哭得更大声了。**

那么问题就来了，有没有一个办法，可以解决上面的问题？当然有，**没有什么是加一个中间层不能解决的，如果有，那就再加一层**。
这次我们要加的中间层，叫 **Kubernetes**。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibux495Hxcfx6ZAnFYGWU7T7PtoUpbCAu28aTA0UsFkPb0NuU2j73ccfQ/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">Kubernetes的位置</center>

## Kubernetes 是什么？

Kubernetes，它是 **G 家**开源的神器，因为单词太长，所以我们习惯省略中间 8 个字母，简称它为 **k8s**。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuh8RnALIyKQY1YCHtdYPJqo3eOLXWOPvVPNTsc9rcwQwv4mfrEPcNMA/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">k8s名称的由来</center>

它介于**应用服务**和**服务器**之间，能够通过策略，协调和管理多个应用服务，只需要一个 **yaml** 文件配置，定义应用的部署顺序等信息，就能自动部署应用到各个服务器上，还能让它们挂了自动重启，自动扩缩容。

听起来有些厉害，它是怎么实现这些功能的呢？

## Kubernetes 架构原理

为了实现上面的功能，Kubernetes 会将我们的服务器划为两部分，一部分叫**控制平面**（control plane，以前叫master），另一部分叫**工作节点**，也就是 **Node**。简单来说它们的关系就是老板和打工人， 用现在流行的说法就是训练师和帕鲁。控制平面负责控制和管理各个 Node，而 Node 则负责实际运行各个应用服务。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibugDPZkvAPQwa9bMj5JibQyyibHcdt2evOYbz34x9bBIz5rsYfjgUn2DMQ/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">k8s控制平面和Node的关系</center>

我们依次看下这两者的内部架构。

### 控制平面内部组件

- • 以前我们需要登录到每台服务器上，手动执行各种命令，现在我们只需要调用 k8s 的提供的 api 接口，就能操作这些服务资源，这些接口都由 **API Server** 组件提供。
- • 以前我们需要到处看下哪台服务器 cpu 和内存资源充足，然后才能部署应用，现在这部分决策逻辑由 **Scheduler**（调度器）来完成。
- • 找到服务器后，以前我们会手动创建，关闭服务，现在这部分功能由 **Controller Manager**（控制器管理器）来负责。
- • 上面的功能都会产生一些数据，这些数据需要被保存起来，方便后续做逻辑，因此 k8s 还会需要一个**存储层**，用来存放各种数据信息，目前是用的 **etcd**，这部分源码实现的很解耦，后续可能会扩展支持其他中间件。

以上就是控制平面内部的组件。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibu8AAicDefm2ickPiaYgy7VRib0lqbOiagWfCV3fCCYCPsp8oOGLdib4sr6fmg/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">k8s控制平面组件</center>

我们接下来再看看 Node 里有哪些组件。

### Node 内部组件

Node 是实际的工作节点，它既可以是**裸机服务器**，也可以是**虚拟机**。它会负责实际运行各个应用服务。多个应用服务**共享**一台 Node 上的内存和 CPU 等计算资源。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuNRd6W9yLEOTIwq19q94tbyrJsiaYx0ibpkk0RQKTXLk8WLHwb2XEsCGA/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">Node可以是裸机服务器或虚拟机</center>

在文章开头，我们聊到了部署多个应用服务的场景。以前我们需要上传代码到服务器，而用了 k8s 之后，我们只需要将服务代码打包成**Container Image**(容器镜像)，就能一行命令将它部署。

如果你不了解容器镜像的含义，你可以简单理解为它其实就是将**应用代码**和依赖的**系统环境**打了个压缩包，在任意一台机器上解压这个压缩包，就能正常运行服务。为了下载和部署镜像，Node 中会有一个 **Container runtime** 组件。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuDU4bzfDaJOicFEfRCG8TkbW9VjpqsMNm0H0A2agIKNp6UuosUMF02bQ/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">将容器镜像粗略理解为压缩包</center>

每个应用服务都可以认为是一个 **Container**（容器）, 并且大多数时候，我们还会为应用服务搭配一个日志收集器 Container 或监控收集器 Container，多个 Container 共同组成一个一个 **Pod**，它运行在 Node 上。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuL2qXw4FvAb5zCjpLicpvUgDRIocRhCO29vlWNXY91UFcVWknyGPCckw/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">一个pod内有多个容器</center>

k8s 可以将 pod 从某个 Node 调度到另一个 Node，还能以 pod 为单位去做重启和动态扩缩容的操作。
所以说 **Pod 是 k8s 中最小的调度单位**。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuqp2HoV9pI5K0PLT9GHYo1ibpxQ4XZibFw6rJ0GUwqvowGG1st1GZGvwA/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">Node调度Pod</center>

另外，前面提到控制平面会用 **Controller Manager** （通过API Server）控制 Node 创建和关闭服务，那 Node 也得有个组件能接收到这个命令才能去做这些动作，这个组件叫 **kubelet**，它主要负责管理和监控 Pod。最后，Node 中还有个 **Kube Proxy** ，它负责 Node 的网络通信功能，有了它，外部请求就能被转发到 Pod 内。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuficlOmImg7MpXJJhsyuIHovibSArN1W8efuPVhzyvBT9ow5lIPCrgLJQ/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">控制平面和Node的组件</center>

### Cluster

**控制平面和Node** 共同构成了一个 **Cluster**，也就是**集群**。在公司里，我们一般会构建多个集群, 比如测试环境用一个集群，生产环境用另外一个集群。同时，为了将集群内部的服务暴露给外部用户使用，我们一般还会部署一个入口控制器，比如 **Ingress 控制器（比如Nginx）**，它可以提供一个入口让外部用户访问集群内部服务。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuAYpibMpga6zDscjndcWTCBmLKWUFA0tJLMXk775c11LMtl2ibpDqfJxA/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">生产和测试环境</center>

### kubectl 是什么

上面提到说我们可以使用 k8s 提供的 API 去创建服务，但问题就来了，这是需要我们自己写代码去调用这些 API 吗？答案是不需要，k8s 为我们准备了一个命令行工具 **kubectl**，我们只需要执行命令，它内部就会调用 k8s 的 API。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibu8sBfqhR1aQYjK365xdS694Uy7iclGKPEsYnO3Epvws5WRj75zaITjvQ/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">kubectl调用k8s的API</center>

接下来我们以部署服务为例子，看下 k8s 是怎么工作的。

### 怎么部署服务？

首先我们需要编写 **YAML 文件**，在里面定义 Pod 里用到了哪些镜像，占用多少内存和 CPU 等信息。然后使用 kubectl 命令行工具执行 `kubectl apply -f xx.yaml` ，此时 kubectl 会读取和解析 YAML 文件，将解析后的对象通过 API 请求发送给 Kubernetes 控制平面内 的 **API Server**。API Server 会根据要求，驱使 **Scheduler** 通过 **etcd** 提供的数据寻找合适的 **Node**， **Controller Manager** 会通过 API Server 控制 Node 创建服务，Node 内部的 **kubelet** 在收到命令后会开始基于 **Container runtime** 组件去拉取镜像创建容器，最终完成 **Pod** 的创建。

至此服务完成创建。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibu7p7LjxJiamVXg6wzeXk5V39V5AYoaSvBjeXITsaHcGclh2rTq6iaO5kg/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">部署应用服务</center>

整个过程下来，我们只需要写一遍 yaml 文件，和执行一次 kubectl 命令，比以前省心太多了！部署完服务后，我们来看下服务是怎么被调用的。

### 怎么调用服务？

以前外部用户小明，直接在浏览器上发送 http 请求，就能打到我们服务器上的 Nginx，然后转发到部署的服务内。用了 k8s 之后，外部请求会先到达 Kubernetes 集群的 Ingress 控制器，然后请求会被转发到 Kubernetes 内部的某个 Node 的 **Kube Proxy** 上，再找到对应的 pod，然后才是转发到内部**容器服务**中，处理结果原路返回，到这就完成了一次服务调用。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/icMUEqOiagpkjfBjJIFsy8zmliazZGEHibibuUoXiblibOWWtjOUs0V2cfdPmlm4RH1ichhJnuTfKheqb5R0icm4Xh24G8g/640?wx_fmt=jpeg&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

<center style="color: #999; font-size:0.8em">用户调用k8s内应用服务的流程</center>

到这里我们就大概了解了 k8s 的工作原理啦，它本质上就是应用服务和服务器之间的**中间层**，通过暴露一系列 API 能力让我们简化服务的部署运维流程。

并且，不少中大厂基于这些 API 能力搭了自己的服务管理平台，程序员不再需要敲 kubectl 命令，直接在界面上点点几下，就能完成服务的部署和扩容等操作，是真的嘎嘎好用。



## 总结

1. 在单机时代，一个应用部署在一个服务器上，当访问量过大时，应用服务经常会挂掉。

2. 这时可以采用自动重启的工具，并且应用服务部署在多个服务器上，便可以抗住大流量。

3. 再后来上线了语音服务、商城服务等，随着应用增多，需求也开始千奇百怪，比如有的应用不希望被外网访问到，有的部署的时候对内存大小有要求，这样的话需要每次登录到每一个服务器上手动操作更新。

4. 为了解决上述3的问题，我们在应用和服务器之间添加了一个中间层：k8s。k8s只需要通过一个`ymal`文件，便能自动协调和管理多个应用服务。

5. k8s的架构原理

   （1）k8s将服务器划分为两部分：<b>control plane</b>和<b>Node</b>。控制平面负责管理Node，Node负责运行各个应用服务。

   （2）控制平面内部组件：以前我们需要输入命令登录到各个服务器上，现在我们只需要调用k8s提供的API接口就能操作这些服务器资源，而这些接口都是由<b>api-server</b>组件提供；以前我们部署应用之前需要查看哪台服务器内存资源充足，现在由<b>schedule</b>调度器来完成；找到服务器后，以前我们需要手动创建或关闭服务，现在这部分功能由<b>controller manager</b>来负责；上面功能产生的数据会保存到存储层<b>ETCD</b>。

   （3）Node内部组件：以前我们需要上传代码到服务器，而用了k8s后只需要将服务代码打包成<b>Container Image</b>（容器镜像可以理解为就是将应用代码和系统环境打了个压缩包），然后就能一行命令将其部署；为了下载和解压容器镜像，Node中有一个<b>container runtime</b>；每个应用服务都可以认为是一个container，并且大多数时候我们都会为应用搭配一个日志收集器container或监控采集器container，这多个container共同组成一个<b>Pod</b>，它运行在Node上，k8s可以将Pod从某个Node调度到另一个Node，还能以Pod为单位做重启和动态扩缩容的操作，所以说Pod是K8S中最小的调度单位；控制平面会通过controller mgr控制Node创建和关闭服务，而Node会通过组件<b>kubectl</b>来接收这个动作，它主要负责管理和监控Pod（kubectl的另外一个作用是命令行工具，通过这个工具可以调用k8s提供的API）；Node中还有个<b>kube proxy</b>，它负责Node的网络通信功能，有了它，外部请求就能被转发到Pod中。

   （4）控制平面和Node共同构成了一个<b>Cluster</b>，也就是集群。为了让集群暴漏给外部使用，我们会使用<b>Ingress</b>控制器，它能够提供一个入口，让外部用户访问集群内部服务。

6. pod 是基于container的，而docker是container 的一种实现方式。Podman 也是一种容器实现。



## 参考文档

[k8s 到底是什么，架构是怎么样的？](https://mp.weixin.qq.com/s/dckA1ezcABndN5WSg1BOBA)

[K8S教程](https://k8s.easydoc.net/)

[【万字长文】K8s部署前后端分离web应用避坑指南之一：从源代码到docker compose到k8s云集群（macOS-2023版）](https://cloud.tencent.com/developer/article/2351935)

