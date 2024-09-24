## 1. 关键字和保留字

### 1. 关键字

被Java语言赋予了特殊含义，用作专门用途的字符串（单词）。关键字中所有字母都为小写。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161006439.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161006685.png)

### 2. 保留字

现有Java版本尚未使用，但以后版本可能会作为关键字使用。自己命名标识符时要避免使用这些保留字：

```goto、const```



## 2. 标识符

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161007214.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161007413.png)



## 3. 数据类型

### 1. 变量

对于每一种数据都定义了明确的具体数据类型（强类型语言），在内存中分配了不同大小的内存空间。

计算机内存的最小存储单元是字节（byte），一个字节就是一个8位二进制数，即8个bit。它的二进制表示范围从`00000000~11111111`，换算成十进制是`0~255`。 一个字节是1byte，1024字节是1K，1024K是1M，1024M是1G，1024G是1T。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161008198.png)

不同的数据类型占用的字节数不一样。我们看一下Java基本数据类型占用的字节数：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161008845.png)

byte恰好就是一个字节，而long和double需要8个字节。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161009732.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161009391.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161009641.png)

注意char类型使用单引号`'`，且仅有一个字符，要和双引号`"`的字符串类型区分开。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161010438.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161010834.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161010139.png)

### 2. 常量

定义变量的时候，如果加上final修饰符，这个变量就变成了常量：

```final double PI = 3.14; // PI是一个常量```

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161011221.png)



## 4. 进制

### 1. 二进制的整数有三种形式

原码：直接将一个数值换成二进制数，最高位是符号位

负数的反码：是对原码按位取反，只是最高位（符号位）确定为1

负数的补码：其反码加1

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161011204.png)

### 2. 计算机以二进制补码的形式保存所有的整数

正数的原码、反码、补码都相同

负数的补码是其反码+1

### 3. 为什么要使用原码、反码、补码表示形式呢？

计算机辨别“符号位”显然会让计算机的基础电路设计变得十分复杂! 于是人们想出了将符号位也参与运算的方法. 我们知道, 根据运算法则减去一个正数等于加上一个负数, 即: 1-1 = 1 + (-1) = 0 , 所以机器可以只有加法而没有减法, 这样计算机运算的设计就更简单了。

### 4. 进制转换

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161012356.png)



## 5. 运算符

运算符是一种特殊的符号，用以表示数据的运算、赋值和比较等。

### 1. 算术运算符

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161012654.png)

### 2. 赋值运算符

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161013674.png)

### 3. 比较运算符（关系运算符）

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161013112.png)

### 4. 逻辑运算符

& 逻辑与、| 逻辑或、！逻辑非、&&短路与、|| 短路或、^逻辑异或

### 5. 位运算符

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161013660.png)

### 6. 三元运算符

### 7. 运算符的优先级

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161014611.png)



## 6. 程序流程控制

（1）顺序结构

（2）分支结构：`if...else 、 switch-case`

（3）循环结构：`while、do...while、for`。

注：JDK1.5提供了foreach循环，方便遍历集合、数组元素

（4）特殊关键字的使用：`break、continue、return`

- break 语句用于终止某个语句块的执行。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161015699.png)

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161015420.png)

return并非专门用于结束循环的，它的功能是结束一个方法。当一个方法执行到return语句时，这个方法将被结束。与break和continue不同的是，return直接结束整个方法，不管这个return处于多少层循环之内。

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161015722.png)

