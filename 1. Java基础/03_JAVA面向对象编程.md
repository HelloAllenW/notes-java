## 1. 相关基础说法

（1）面向过程POP。面向对象， 英文是Object-Oriented Programming，简称OOP，是一种通过对象的方式，把现实世界映射到计算机模型的一种编程方法。

（2）面向对象的三大特征：封装、继承、多态

（3）万事万物皆对象

（4）对象也称为实例，是抽象类的具体表现



## 2. 类的成员构成

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161017313.JPG)



## 3. 内存解析

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202405161017102.png)



## 4. 匿名对象

我们也可以不定义对象的句柄，而直接调用这个对象的方法。这样的对象叫做匿名对象。如 new Person().shout()；

如果一个对象只需要进行一次方法调用，那么就可以使用匿名对象。



## 5. 方法

（1）引入方法

一个class可以包含多个field， 为了避免外部代码直接去访问field，我们可以用private修饰field，拒绝外部访问。然后需要使用方法（method）来让外部代码可以间接修改field。 在方法内部，我们就有机会检查外部修改field是否满足规定。

所以，一个类通过定义方法，就可以给外部代码暴露一些操作的接口，同时，内部自己保证逻辑一致性。

（2）定义为private方法，只有class内部方法可以调用。

（3）在方法内部，可以使用一个隐含的变量this，它始终指向当前实例。因此，通过this.field就可以访问当前实例的字段。如果没有命名冲突，可以省略this。

（4）可变参数传参

```
class Group {
    private String[] names;
    public void setNames(String... names) {
        this.names = names;
    }
}
```



## 6. 构造方法

（1） 由于构造方法是如此特殊，所以构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有返回值（也没有void），调用构造方法，必须用new操作符。

（2） 如果一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法，它没有参数，也没有执行语句。类似这样：

```
class Person {
    public Person() {
    }
}
```

如果我们自定义了一个构造方法，那么，编译器就不再自动创建默认构造方法

（3） 可以定义多个构造方法，在通过new操作符调用的时候，编译器通过构造方法的参数数量、位置和类型自动区分



## 7. 方法重载

在一个类中，我们可以定义多个方法。如果有一系列方法，它们的功能都是类似的，只有参数有所不同，那么，可以把这一组方法名做成同名方法。这种方法名相同，但各自的参数不同，称为方法重载（Overload）。

注意：方法重载的返回值类型通常都是相同的。

方法重载的目的是，功能类似的方法使用同一名字，更容易记住，因此，调用起来更简单。



## 8. 继承

Java使用extends关键字来实现继承：

```
class Person { // 编译器会自动加上extends Object
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}
```

```
class Student extends Person {
    // 不要重复name和age字段/方法,

    // 只需要定义新增score字段/方法:
    private int score;
    public int getScore() { … }
    public void setScore(int score) { … }
}
```

（1）Java只允许一个class继承自一个类，因此，一个类有且仅有一个父类。只有Object特殊，它没有父类。

（2）继承有个特点，就是子类无法访问父类的private字段或者private方法。 这使得继承的作用被削弱了。为了让子类可以访问父类的字段，我们需要把private改为protected。用protected修饰的字段可以被子类访问。 protected关键字可以把字段和方法的访问权限控制在继承树内部。

（3） 子类不会继承任何父类的构造方法， 子类的构造方法可以通过super()调用父类的构造方法

```
class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 必须使用super调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

（4）阻止继承

正常情况下，只要某个class没有final修饰符，那么任何类都可以从该class继承。

从Java 15开始，允许使用sealed修饰class，并通过permits明确写出能够从该class继承的子类名称。

```
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

（5） 可以安全地向上转型为更抽象的类型。可以强制向下转型，最好借助instanceof判断；

（6）区分继承和组合

在使用继承时，我们要注意逻辑一致性。

考察下面的Book类：

```
class Book {
	protected String name;
	public String getName() {...}
	public void setName(String name) {...}
}
```

这个Book类也有name字段，那么，我们能不能让Student继承自Book呢？

```
class Student extends Book {
	protected int score;
}
```

显然，从逻辑上讲，这是不合理的，Student不应该从Book继承，而应该从Person继承。

究其原因，是因为Student是Person的一种，它们是is关系，而Student并不是Book。实际上Student和Book的关系是has关系。

具有has关系不应该使用继承，而是使用组合，即Student可以持有一个Book实例：

```
class Student extends Person {
	protected Book book;
	protected int score;
}
```

因此，继承是is关系，组合是has关系。



## 9. 多态

（1） 覆写（Override）和 Overload的区别。

只有方法签名相同，并且返回值也相同，才是Override。 加上@Override可以让编译器帮助检查是否进行了正确的覆写。

```
class Person {
    public void run() { … }
}
```

```
class Student extends Person {
    // 不是Override，因为参数不同:
    public void run(String s) { … }
    // 不是Override，因为返回值不同:
    public int run() { … }
    @Override // Compile error!
    public void run(String s) {}
}
```

（2）多态的概念

如果子类覆写了父类的方法：

```
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

```
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

那么，一个实际类型为Student，引用类型为Person的变量，

`Person p = new Student();`

调用其run()方法，调用的是Person还是Student的run()方法？

运行一下上面的代码就可以知道，实际上调用的方法是Student的run()方法。因此可得出结论：

Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。

这个非常重要的特性在面向对象编程中称之为多态。它的英文拼写非常复杂：Polymorphic。

（3）覆写Object的方法

（4）super的使用

（5）final的使用



## 10. 抽象类

（1）抽象方法到抽象类

由于多态的存在，每个子类都可以覆写父类的方法。如果父类Person的run()方法没有实际意义，能否去掉方法的执行语句？答案是不行。 但是父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法。把一个方法声明为abstract，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，Person类也无法被实例化。编译器会告诉我们，无法编译Person类，因为它包含抽象方法。必须把Person类本身也声明为abstract，才能正确编译它：

```
abstract class Person {
    public abstract void run();
}
```

使用abstract修饰的类就是抽象类。我们无法实例化一个抽象类：

```Person p = new Person(); // 编译错误```

无法实例化的抽象类有什么用？

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。

例如，Person类定义了抽象方法run()，那么，在实现子类Student的时候，就必须覆写run()方法。

（2）抽象编程

当我们定义了抽象类Person，以及具体的Student、Teacher子类的时候，我们可以通过抽象类Person类型去引用具体的子类的实例：

```
Person s = new Student();
Person t = new Teacher();
```

这种引用抽象类的好处在于，我们对其进行方法调用，并不关心Person类型变量的具体子类型：

```
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：

```
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```

这种尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程。

所以，抽象编程是基于多态的基础的。

面向抽象编程的本质就是：

- 上层代码只定义规范（例如：abstract class Person）；
- 不需要子类就可以实现业务逻辑（正常编译）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心。



## 11. 接口

静态字段和静态方法

（1）静态字段

在一个class中定义的字段，我们称之为实例字段。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

还有一种字段，是用static修饰的字段，称为静态字段：static field。

实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，所有实例都会共享该字段。

虽然实例可以访问静态字段，但是它们指向的其实都是Person class的静态字段。所以，所有实例共享一个静态字段。

因此，不推荐用实例变量.静态字段去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为类名.静态字段来访问静态对象。

推荐用类名来访问静态字段。可以把静态字段理解为描述class本身的字段（非实例字段）。

（2）静态方法

有静态字段，就有静态方法。用static修饰的方法称为静态方法。

因为静态方法属于class而不属于实例，因此，静态方法内部，无法访问this变量，也无法访问实例字段，它只能访问静态字段。

通过实例变量也可以调用静态方法，但这只是编译器自动帮我们把实例改写成类名而已。

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

（3）接口的静态字段

因为interface是一个纯抽象类，所以它不能定义实例字段。但是，interface是可以有静态字段的，并且静态字段必须为final类型：

```
public interface Person {
	public static final int MALE = 1;
	public static final int FEMALE = 2;
}
```

实际上，因为interface的字段只能是public static final类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```
public interface Person {
	// 编译器会自动加上public statc final:
	int MALE = 1;
	int FEMALE = 2;
}
```

编译器会自动把该字段变为public static final类型。



## 12. 包

在现实中，如果小明写了一个Person类，小红也写了一个Person类，现在，小白既想用小明的Person，也想用小红的Person，怎么办？

如果小军写了一个Arrays类，恰好JDK也自带了一个Arrays类，如何解决类名冲突？

在Java中，我们使用package来解决名字冲突。

JDK的核心类使用java.lang包，编译器会自动导入；

JDK的其它常用类定义在java.util.*，java.math.*，java.text.*，……；

包名推荐使用倒置的域名，例如org.apache。



## 13. 作用域

Java内建的访问权限包括public、protected（ 定义为protected的字段和方法可以被子类访问，以及子类的子类）、private（ private访问权限被限定在class的内部）和package权限；

Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；

final修饰符不是访问权限，它可以修饰class（用final修饰class可以阻止被继承）、field（ 用final修饰field可以阻止被重新赋值）和method（用final修饰method可以阻止被子类覆写）；

一个.java文件只能包含一个public类，但可以包含多个非public类。 如果有public类，文件名必须和public类的名字相同。



## 14. 内部类

Java的内部类可分为Inner Class、Anonymous Class和Static Nested Class三种：

- Inner Class和Anonymous Class本质上是相同的，都必须依附于Outer Class的实例，即隐含地持有Outer.this实例，并拥有Outer Class的private访问权限；
- Static Nested Class是独立类，但拥有Outer Class的private访问权限。



## 15. classpath和jar

（1）到底什么是classpath？

classpath是JVM用到的一个环境变量，它用来指示JVM如何搜索class。

因为Java是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正可以被JVM执行的字节码。因此，JVM需要知道，如果要加载一个`abc.xyz.Hello`的类，应该去哪搜索对应的`Hello.class`文件。

所以，classpath就是一组目录的集合，它设置的搜索路径与操作系统相关。例如，在Windows系统上，用;分隔，带空格的目录用""括起来，可能长这样：

C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"

现在我们假设classpath是.;C:\work\project1\bin;C:\shared，当JVM在加载abc.xyz.Hello这个类时，会依次查找：

- <当前目录>\abc\xyz\Hello.class
- C:\work\project1\bin\abc\xyz\Hello.class
- C:\shared\abc\xyz\Hello.class

注意到.代表当前目录。如果JVM在某个路径下找到了对应的class文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。

（2）classpath的设定方法有两种：

在系统环境变量中设置classpath环境变量，不推荐；在启动JVM时设置classpath变量，推荐。

java -classpath .;C:\work\project1\bin;C:\shared abc.xyz.Hello

// 简写：

java -cp .;C:\work\project1\bin;C:\shared abc.xyz.Hello

（3）jar包

如果有很多.class文件，散落在各层目录中，肯定不便于管理。如果能把目录打一个包，变成一个文件，就方便多了。

jar包就是用来干这个事的，它可以把package组织的目录层级，以及各个目录下的所有文件（包括.class文件和其他文件）都打成一个jar文件，这样一来，无论是备份，还是发给客户，就简单多了。

jar包实际上就是一个zip格式的压缩文件，而jar包相当于目录。如果我们要执行一个jar包的class，就可以把jar包放到classpath中：

java -cp ./hello.jar abc.xyz.Hello

这样JVM会自动在hello.jar文件里去搜索某个类。

（4）那么问题来了：如何创建jar包？

因为jar包就是zip包，所以，直接在资源管理器中，找到正确的目录，点击右键，在弹出的快捷菜单中选择“发送到”，“压缩(zipped)文件夹”，就制作了一个zip文件。然后，把后缀从.zip改为.jar，一个jar包就创建成功。 这里需要特别注意的是，jar包里的第一层目录，不能是bin，而应该是hong、ming、mr。

jar包还可以包含一个特殊的/META-INF/MANIFEST.MF文件，MANIFEST.MF是纯文本，可以指定Main-Class和其它信息。JVM会自动读取这个MANIFEST.MF文件，如果存在Main-Class，我们就不必在命令行指定启动的类名，而是用更方便的命令：

java -jar hello.jar

在大型项目中，不可能手动编写MANIFEST.MF文件，再手动创建zip包。Java社区提供了大量的开源构建工具，例如[Maven](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945359327200)，可以非常方便地创建jar包。



## 16. 版本问题：java版本和class版本

我们通常说的Java 8，Java 11，Java 17，是指JDK的版本，也就是JVM的版本，更确切地说，就是`java.exe`这个程序的版本：

```
$ java -version

java version "17" 2021-09-14 LTS
```

而每个版本的JVM，它能执行的class文件版本也不同。例如，Java 11对应的class文件版本是55，而Java 17对应的class文件版本是61。

如果用Java 11编译一个Java程序，输出的class文件版本默认就是55，这个class既可以在Java 11上运行，也可以在Java 17上运行，因为Java 17支持的class文件版本是61，表示“最多支持到版本61”。

如果用Java 17编译一个Java程序，输出的class文件版本默认就是61，它可以在Java 17、Java 18上运行，但不可能在Java 11上运行，因为Java 11支持的class版本最多到55。如果使用低于Java 17的JVM运行，会得到一个`UnsupportedClassVersionError`

在开发阶段，多个版本的JDK可以同时安装，当前使用的JDK版本可由`JAVA_HOME`环境变量切换。



## 17. 模块：从jar到jmod

从Java 9开始，JDK又引入了模块（Module）。那么什么是模块？这要从头说起：

（1）java9之前，jar作为class容器存在的依赖问题

我们知道，`.class`文件是JVM看到的最小可执行文件，而一个大型程序需要编写很多Class，并生成一堆`.class`文件，很不便于管理。所以，**jar文件就是class文件的容器**。

在Java 9之前，一个大型Java程序会生成自己的jar文件，同时引用依赖的第三方jar文件，而JVM自带的Java标准库，实际上也是以jar文件形式存放的，这个文件叫`rt.jar`，一共有60多M。

如果是自己开发的程序，除了一个自己的app.jar以外，还需要一堆第三方的jar包，运行一个Java程序，一般来说，命令行写这样：

`java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Main`

如果漏写了某个运行时需要用到的jar，那么在运行期极有可能抛出`ClassNotFoundException`。

所以，jar只是用于存放class的容器，它并不关心class之间的依赖。

（2）解决jar存在的依赖问题，java9引入了模块

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题。如果a.jar必须依赖另一个b.jar才能运行，那我们应该给a.jar加点说明啥的，让程序在编译和运行的时候能自动定位到b.jar，这种**自带“依赖关系”的class容器就是模块**。

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的`rt.jar`分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在`$JAVA_HOME/jmods`目录下找到它们：

- java.base.jmod
- [java.compiler.jmod](http://java.compiler.jmod/)
- java.datatransfer.jmod
- java.desktop.jmod
- ...

这些`.jmod`文件每一个都是一个模块，模块名就是文件名。例如：模块java.base对应的文件就是java.base.jmod。模块之间的依赖关系已经被写入到模块内的`module-info.class`文件了。**所有的模块都直接或间接地依赖java.base模块**，只有java.base模块不依赖任何模块，它可以被看作是“`根模块`”，好比所有的类都是从Object直接或间接继承而来。

把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）。此外，模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本。

（3）编写模块

编写模块和普通项目是完全一样的， 仅仅在src目录下多了一个`module-info.java`这个文件，这就是模块的描述文件。这个文件内容如下：

```
module hello.world {
   exports com.itranswarp.sample; // 必须导出，不然外部无法调用

   requires java.base; // 可不写，任何模块都会自动引入java.base
   requires java.xml; // 表示这个模块需要引用的其他模块名。
}
```

（4）打包模块和JRE

我们可以把它打成一个jar包，通过命令 `java -jar hello.jar`来运行。然后继续使用JDK自带的`jmod`命令把一个jar包转换成模块 `x.jmod`。我们必须通过jre来运行jmod。