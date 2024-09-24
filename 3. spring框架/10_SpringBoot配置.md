1、配置文件：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161103122.png)

2、SpringBoot配置实例：

（1）创建两个测试bean

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161104274.png)

（2）在yml中进行bean值配置

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161104846.png)

（3）获取配置文件中的值和bean进行绑定

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161104914.png)

（4）单元测试

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161104466.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161104718.png)

（5）如果是properties，则配置如下，bean中获取绑定不变：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161105026.png)

（6）还可以使用@Value注解形式进行配置文件中值得获取

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161105571.png)

3、@propertySource、@ImportResource、@Bean

（1）@propertySource

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161105404.png)

（2）@ImportResource

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161105385.png)

（3）@Bean 给容器添加组件

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161105262.png)

4、 配置文件占位符

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161106233.png)

5、Profile

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161106105.png)

6、配置文件加载位置

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161106697.png)

7、自动配置原理（看的不是很懂）

<https://www.bilibili.com/video/BV1Et411Y7tQ?p=18>



8、@Conditional注解

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161106504.png)



![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161106073.png)

