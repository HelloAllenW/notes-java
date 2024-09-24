Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。

\## 为什么需要泛型？

\### 1. 适用于多种数据类型执行相同的代码

\```

private static int add(int a, int b) {

​    System.out.println(a + "+" + b + "=" + (a + b));

​    return a + b;

}

private static float add(float a, float b) {

​    System.out.println(a + "+" + b + "=" + (a + b));

​    return a + b;

}

private static double add(double a, double b) {

​    System.out.println(a + "+" + b + "=" + (a + b));

​    return a + b;

}

\```

如果没有泛型，要实现不同类型的加法，每种类型都需要重载一个add方法；通过泛型，我们可以复用为一个方法：

\```

private static <T extends Number> double add(T a, T b) {

​    System.out.println(a + "+" + b + "=" + (a.doubleValue() + b.doubleValue()));

​    return a.doubleValue() + b.doubleValue();

}

\```

\### 2. 泛型中的类型在使用时指定，不需要强制类型转换（类型安全，编译器会检查类型）

看下这个例子：

\```

List list = new ArrayList();

list.add("xxString");

list.add(100d);

list.add(new Person()); 

\```

我们在使用上述list中，list中的元素都是Object类型（无法约束其中的类型），所以在取出集合元素时需要人为的强制类型转化到具体的目标类型，且很容易出现`java.lang.ClassCastException`异常。

\```

// 获取到Object，必须强制转型为String:

String first = (String) list.get(0);

// 很容易出现ClassCastException，因为容易“误转型”：

list.add(new Integer(123));

// ERROR: ClassCastException:

String second = (String) list.get(1);

\```

引入泛型，它将提供类型的约束，提供编译前的检查：

\```

// 保证了list中只能放String, 不能放其它类型的元素

List<String> list = new ArrayList<String>();

\```

\## 使用泛型

\### 1.  从 `ArrayList<T>` 看泛型类的使用

Java标准库提供的`ArrayList`内部可以看做就是一个`Object[]`数组，配合存储一个当前分配的长度，就可以充当“可变数组”。

\```

public class ArrayList<T> {

​    private T[] array;

​    private int size;

​    public void add(T e) {...}

​    public void remove(int index) {...}

​    public T get(int index) {...}

}

\```

使用`ArrayList`时，如果不定义泛型类型时，泛型类型实际上就是`Object`：

\```

// 编译器警告:

List list = new ArrayList();

list.add("Hello");

list.add("World");

String first = (String) list.get(0);

String second = (String) list.get(1);

\```

此时，只能把`<T>`当作`Object`使用，没有发挥泛型的优势。

当我们定义泛型类型`<String>`后，`List<T>`的泛型接口变为强类型`List<String>`：

\```

// 无编译器警告:

List<String> list = new ArrayList<String>();

list.add("Hello");

list.add("World");

// 无强制转型:

String first = list.get(0);

String second = list.get(1);

\```

当我们定义泛型类型`<Number>`后，`List<T>`的泛型接口变为强类型`List<Number>`：

\```

List<Number> list = new ArrayList<Number>();

list.add(new Integer(123));

list.add(new Double(12.34));

Number first = list.get(0);

Number second = list.get(1);

\```

编译器如果能自动推断出泛型类型，就可以省略后面的泛型类型。例如，对于下面的代码：

\```

List<Number> list = new ArrayList<Number>();

\```

编译器看到泛型类型`List<Number>`就可以自动推断出后面的`ArrayList<T>`的泛型类型必须是`ArrayList<Number>`，因此，可以把代码简写为：

\```

// 可以省略后面的Number，编译器可以自动推断泛型类型：

List<Number> list = new ArrayList<>();

\```

\### 2. 从 `Comparable<T>`看泛型接口的使用

除了`ArrayList<T>`使用了泛型，有些接口也使用泛型。例如，`Arrays.sort(Object[])`可以对任意数组进行排序，但待排序的元素必须实现`Comparable<T>`这个泛型接口：

\```

public interface Comparable<T> {

​    /**

​     \* 返回负数: 当前实例比参数o小

​     \* 返回0: 当前实例与参数o相等

​     \* 返回正数: 当前实例比参数o大

​     */

​    int compareTo(T o);

}

\```

可以直接对`String`数组进行排序：

\```

// sort

import java.util.Arrays;

public class Main {

​    public static void main(String[] args) {

​        String[] ss = new String[] { "Orange", "Apple", "Pear" };

​        Arrays.sort(ss);

​        System.out.println(Arrays.toString(ss));

​    }

}

\```

这是因为`String`本身已经实现了`Comparable<String>`接口。如果换成我们自定义的`Person`类型试试：

\```

// sort

import java.util.Arrays;

public class Main {

​    public static void main(String[] args) {

​        Person[] ps = new Person[] {

​            new Person("Bob", 61),

​            new Person("Alice", 88),

​            new Person("Lily", 75),

​        };

​        Arrays.sort(ps);

​        System.out.println(Arrays.toString(ps));

​     }

}

class Person {

​    String name;

​    int score;

​    Person(String name, int score) {

​        this.name = name;

​        this.score = score;

​    }

​    public String toString() {

​        return this.name + "," + this.score;

​    }

}

\```

运行程序，我们会得到`ClassCastException`，即无法将`Person`转型为`Comparable`。我们修改代码，让`Person`实现`Comparable<T>`接口：

\```

// sort

import java.util.Arrays;

public class Main {

​    public static void main(String[] args) {

​        Person[] ps = new Person[] {

​            new Person("Bob", 61),

​            new Person("Alice", 88),

​            new Person("Lily", 75),

​        };

​        Arrays.sort(ps);

​        System.out.println(Arrays.toString(ps));

​    }

}

class Person implements Comparable<Person> {

​    String name;

​    int score;

​    Person(String name, int score) {

​        this.name = name;

​        this.score = score;

​    }

​    public int compareTo(Person other) {

​        return this.name.compareTo(other.name);

​    }

​    public String toString() {

​        return this.name + "," + this.score;

​    }

}

\```

运行上述代码，可以正确实现按`name`进行排序。也可以修改比较逻辑，例如，按`score`从高到低排序。

\## 编写泛型

\### 1. 泛型类如何定义使用？

（1）一个简单的泛型类：

\```

class Point<T>{         // 此处可以随便写标识符号，T是type的简称  

​    private T var ;     // var的类型由T指定，即：由外部指定  

​    public T getVar(){  // 返回值的类型由外部决定  

​        return var ;  

​    }  

​    public void setVar(T var){  // 设置的类型也由外部决定  

​        this.var = var ;  

​    }  

}  

public class GenericsDemo06{  

​    public static void main(String args[]){  

​        Point<String> p = new Point<String>() ;     // 里面的var类型为String类型  

​        p.setVar("it") ;                            // 设置字符串  

​        System.out.println(p.getVar().length()) ;   // 取得字符串的长度  

​    }  

}

\```

（2）多元泛型

\```

class Notepad<K,V>{       // 此处指定了两个泛型类型  

​    private K key ;     // 此变量的类型由外部决定  

​    private V value ;   // 此变量的类型由外部决定  

​    public K getKey(){  

​        return this.key ;  

​    }  

​    public V getValue(){  

​        return this.value ;  

​    }  

​    public void setKey(K key){  

​        this.key = key ;  

​    }  

​    public void setValue(V value){  

​        this.value = value ;  

​    }  

} 

public class GenericsDemo09{  

​    public static void main(String args[]){  

​        Notepad<String,Integer> t = null ;        // 定义两个泛型类型的对象  

​        t = new Notepad<String,Integer>() ;       // 里面的key为String，value为Integer  

​        t.setKey("汤姆") ;        // 设置第一个内容  

​        t.setValue(20) ;            // 设置第二个内容  

​        System.out.print("姓名；" + t.getKey()) ;      // 取得信息  

​        System.out.print("，年龄；" + t.getValue()) ;       // 取得信息  

  

​    }  

}

\```

\### 2. 泛型接口如何定义使用？

简单的泛型接口：

\```

interface Info<T>{        // 在接口上定义泛型  

​    public T getVar() ; // 定义抽象方法，抽象方法的返回值就是泛型类型  

}  

class InfoImpl<T> implements Info<T>{   // 定义泛型接口的子类  

​    private T var ;             // 定义属性  

​    public InfoImpl(T var){     // 通过构造方法设置属性内容  

​        this.setVar(var) ;    

​    }  

​    public void setVar(T var){  

​        this.var = var ;  

​    }  

​    public T getVar(){  

​        return this.var ;  

​    }  

} 

public class GenericsDemo24{  

​    public static void main(String arsg[]){  

​        Info<String> i = null;        // 声明接口对象  

​        i = new InfoImpl<String>("汤姆") ;  // 通过子类实例化对象  

​        System.out.println("内容：" + i.getVar()) ;  

​    }  

}

\```

\### 3. 泛型方法如何定义使用？

泛型方法，是在调用方法的时候指明泛型的具体类型。

\- 定义泛型方法语法格式

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202403191428666.png)

\- 调用泛型方法语法格式

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202403191429317.png)

说明一下，定义泛型方法时，必须在返回值前边加一个`<T>`，来声明这是一个泛型方法，持有一个泛型`T`，然后才可以用泛型T作为方法的返回值。

`Class<T>`的作用就是指明泛型的具体类型，而`Class<T>`类型的变量c，可以用来创建泛型类的对象。

为什么要用变量c来创建对象呢？既然是泛型方法，就代表着我们不知道具体的类型是什么，也不知道构造方法如何，因此没有办法去new一个对象，但可以利用变量c的newInstance方法去创建对象，也就是利用反射创建对象。

泛型方法要求的参数是`Class<T>`类型，而`Class.forName()`方法的返回值也是`Class<T>`，因此可以用`Class.forName()`作为参数。其中，`forName()`方法中的参数是何种类型，返回的`Class<T>`就是何种类型。在本例中，`forName()`方法中传入的是User类的完整路径，因此返回的是`Class<User>`类型的对象，因此调用泛型方法时，变量c的类型就是`Class<User>`，因此泛型方法中的泛型T就被指明为User，因此变量obj的类型为User。

当然，泛型方法不是仅仅可以有一个参数`Class<T>`，可以根据需要添加其他参数。

**为什么要使用泛型方法呢**？因为泛型类要在实例化的时候就指明类型，如果想换一种类型，不得不重新new一次，可能不够灵活；而泛型方法可以在调用的时候指明类型，更加灵活。

**java 中泛型标记符：**

\- **E** - Element (在集合中使用，因为集合中存放的是元素)

\- **T** - Type（Java 类）

\- **K** - Key（键）

\- **V** - Value（值）

\- **N** - Number（数值类型）

\- **？** - 表示不确定的 java 类型

\## 有界的类型参数

可能有时候，你会想限制那些被允许传递到一个类型参数的类型种类范围。例如，一个操作数字的方法可能只希望接受Number或者Number子类的实例。这就是有界类型参数的目的。

要声明一个有界的类型参数，首先列出类型参数的名称，后跟extends关键字，最后紧跟它的上界。

\### 1. 上限

为传入的泛型类型实参进行上边界的限制，如：类型实参只准传入某种类型的父类或某种类型的子类。

\```

class Info<T extends Number>{    // 此处泛型只能是数字类型

​    private T var ;        // 定义泛型变量

​    public void setVar(T var){

​        this.var = var ;

​    }

​    public T getVar(){

​        return this.var ;

​    }

​    public String toString(){    // 直接打印

​        return this.var.toString() ;

​    }

}

public class demo1{

​    public static void main(String args[]){

​        Info<Integer> i1 = new Info<Integer>() ;        // 声明Integer的泛型对象

​    }

}

\```

\### 2. 下限

\```

class Info<T>{

​    private T var ;        // 定义泛型变量

​    public void setVar(T var){

​        this.var = var ;

​    }

​    public T getVar(){

​        return this.var ;

​    }

​    public String toString(){    // 直接打印

​        return this.var.toString() ;

​    }

}

public class GenericsDemo21{

​    public static void main(String args[]){

​        Info<String> i1 = new Info<String>() ;        // 声明String的泛型对象

​        Info<Object> i2 = new Info<Object>() ;        // 声明Object的泛型对象

​        i1.setVar("hello") ;

​        i2.setVar(new Object()) ;

​        fun(i1) ;

​        fun(i2) ;

​    }

​    public static void fun(Info<? super String> temp){    // 只能接收String或Object类型的泛型，String类的父类只有Object类

​        System.out.print(temp + ", ") ;

​    }

}

\```

\### 3. 类型通配符

（1）类型通配符一般是使用 ? 代替具体的类型参数。例如 `List<?>` 在逻辑上是 `List<String`、`List<Integer>` 等所有 List<具体类型实参> 的父类。

\```

import java.util.*;

 

public class GenericTest {

​    public static void main(String[] args) {

​        List<String> name = new ArrayList<String>();

​        List<Integer> age = new ArrayList<Integer>();

​        List<Number> number = new ArrayList<Number>();

​        

​        name.add("icon");

​        age.add(18);

​        number.add(314);

 

​        getData(name);

​        getData(age);

​        getData(number);

   }

 

   public static void getData(List<?> data) {

​      System.out.println("data :" + data.get(0));

   }

}

\```

输出结果为：

\```

data :icon

data :18

data :314

\```

**解析：** 因为 `getData()` 方法的参数是 `List<?>` 类型的，所以 `name`，`age`，`number` 都可以作为这个方法的实参，这就是通配符的作用。

（2）类型通配符上限通过形如List来定义，如此定义就是通配符泛型值接受Number及其下层子类类型。

\```

import java.util.*;

 

public class GenericTest {

​     

​    public static void main(String[] args) {

​        List<String> name = new ArrayList<String>();

​        List<Integer> age = new ArrayList<Integer>();

​        List<Number> number = new ArrayList<Number>();

​        

​        name.add("icon");

​        age.add(18);

​        number.add(314);

 

​        //getUperNumber(name);//1

​        getUperNumber(age);//2

​        getUperNumber(number);//3

​       

   }

 

   public static void getData(List<?> data) {

​      System.out.println("data :" + data.get(0));

   }

   

   public static void getUperNumber(List<? extends Number> data) {

​          System.out.println("data :" + data.get(0));

​       }

}

\```

输出结果：

\```

data :18

data :314

\```

**解析：** 在 //1 处会出现错误，因为 `getUperNumber()` 方法中的参数已经限定了参数泛型上限为 `Number`，所以泛型为 `String` 是不在这个范围之内，所以会报错。

（3）类型通配符下限通过形如 `List<? super Number>` 来定义，表示类型只能接受 `Number` 及其上层父类类型，如 `Object` 类型的实例。

\## Java泛型的本质：擦拭法

\### 1. 擦拭法

泛型是一种类似”模板代码“的技术，不同语言的泛型实现方式不一定相同。Java语言的泛型实现方式是擦拭法（Type Erasure）。Java泛型擦拭法这个特性是从JDK 1.5才开始加入的，因此为了兼容之前的版本，Java泛型的实现采取了“伪泛型”的策略，即Java在语法上支持泛型，但是在编译阶段会进行所谓的“类型擦除”（Type Erasure），将所有的泛型表示（尖括号中的内容）都替换为具体的类型（其对应的原生态类型），就像完全没有泛型一样。

例如，我们编写了一个泛型类`Pair<T>`，这是编译器看到的代码：

\```

public class Pair<T> {

​    private T first;

​    private T last;

​    public Pair(T first, T last) {

​        this.first = first;

​        this.last = last;

​    }

​    public T getFirst() {

​        return first;

​    }

​    public T getLast() {

​        return last;

​    }

}

\```

而虚拟机根本不知道泛型。这是虚拟机执行的代码：

\```

public class Pair {

​    private Object first;

​    private Object last;

​    public Pair(Object first, Object last) {

​        this.first = first;

​        this.last = last;

​    }

​    public Object getFirst() {

​        return first;

​    }

​    public Object getLast() {

​        return last;

​    }

}

\```

因此，Java使用擦拭法实现泛型，导致了：

\- 编译器把类型`<T>`视为`Object`；

\- 编译器根据`<T>`实现安全的强制转型。

使用泛型的时候，我们编写的代码也是编译器看到的代码：

\```

Pair<String> p = new Pair<>("Hello", "world");

String first = p.getFirst();

String last = p.getLast();

\```

而虚拟机执行的代码并没有泛型：

\```

Pair p = new Pair("Hello", "world");

String first = (String) p.getFirst();

String last = (String) p.getLast();

\```

所以，Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。

了解了Java泛型的实现方式——擦拭法，我们就知道了Java泛型的局限：

\### 2. 局限一：`<T>`不能是基本类型

例如`int`，因为实际类型是`Object`，`Object`类型无法持有基本类型：

\```

Pair<int> p = new Pair<>(1, 2); // compile error!

\```

\### 3. 局限二：无法取得带泛型的`Class`

观察以下代码：

\```

public class Main {

​    public static void main(String[] args) {

​        Pair<String> p1 = new Pair<>("Hello", "world");

​        Pair<Integer> p2 = new Pair<>(123, 456);

​        Class c1 = p1.getClass();

​        Class c2 = p2.getClass();

​        System.out.println(c1==c2); // true

​        System.out.println(c1==Pair.class); // true

​    }

}

class Pair<T> {

​    private T first;

​    private T last;

​    public Pair(T first, T last) {

​        this.first = first;

​        this.last = last;

​    }

​    public T getFirst() {

​        return first;

​    }

​    public T getLast() {

​        return last;

​    }

}

\```

因为`T`是`Object`，我们对`Pair<String>`和`Pair<Integer>`类型获取`Class`时，获取到的是同一个`Class`，也就是`Pair`类的`Class`。

换句话说，所有泛型实例，无论`T`的类型是什么，`getClass()`返回同一个`Class`实例，因为编译后它们全部都是`Pair<Object>`。

\### 4. 局限三：无法判断带泛型的类型

\```

Pair<Integer> p = new Pair<>(123, 456);

// Compile error:

if (p instanceof Pair<String>) {

}

\```

原因和前面一样，并不存在`Pair<String>.class`，而是只有唯一的`Pair.class`。

\### 5. 局限四：不能实例化`T`类型

\```

public class Pair<T> {

​    private T first;

​    private T last;

​    public Pair() {

​        // Compile error:

​        first = new T();

​        last = new T();

​    }

}

\```

上述代码无法通过编译，因为构造方法的两行语句：

\```

first = new T();

last = new T();

\```

擦拭后实际上变成了：

\```

first = new Object();

last = new Object();

\```

这样一来，创建`new Pair<String>()`和创建`new Pair<Integer>()`就全部成了`Object`，显然编译器要阻止这种类型不对的代码。

要实例化`T`类型，我们必须借助额外的`Class<T>`参数：

\```

public class Pair<T> {

​    private T first;

​    private T last;

​    public Pair(Class<T> clazz) {

​        first = clazz.newInstance();

​        last = clazz.newInstance();

​    }

}

\```

上述代码借助`Class<T>`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class<T>`。例如：

\```

Pair<String> pair = new Pair<>(String.class);

\```

因为传入了`Class<String>`的实例，所以我们借助`String.class`就可以实例化`String`类型。

\## 补充：泛型继承相关的问题

\### 1. 不恰当的覆写方法

有些时候，一个看似正确定义的方法会无法通过编译。例如：

\```

public class Pair<T> {

​    public boolean equals(T t) {

​        return this == t;

​    }

}

\```

这是因为，定义的`equals(T t)`方法实际上会被擦拭成`equals(Object t)`，而这个方法是继承自`Object`的，编译器会阻止一个实际上会变成覆写的泛型方法定义。

换个方法名，避开与`Object.equals(Object)`的冲突就可以成功编译：

\```

public class Pair<T> {

​    public boolean same(T t) {

​        return this == t;

​    }

}

\```

\### 2. 获取父类的泛型类型

一个类可以继承自一个泛型类。例如：父类的类型是`Pair<Integer>`，子类的类型是`IntPair`，可以这么继承：

\```

public class IntPair extends Pair<Integer> {

}

\```

使用的时候，因为子类`IntPair`并没有泛型类型，所以，正常使用即可：

\```

IntPair ip = new IntPair(1, 2);

\```

前面讲了，我们无法获取`Pair<T>`的`T`类型，即给定一个变量`Pair<Integer> p`，无法从`p`中获取到`Integer`类型。

但是，在父类是泛型类型的情况下，编译器就必须把类型`T`（对`IntPair`来说，也就是`Integer`类型）保存到子类的class文件中，不然编译器就不知道`IntPair`只能存取`Integer`这种类型。

在继承了泛型类型的情况下，子类可以获取父类的泛型类型。例如：`IntPair`可以获取到父类的泛型类型`Integer`。获取父类的泛型类型代码比较复杂：

\```

import java.lang.reflect.ParameterizedType;

import java.lang.reflect.Type;

public class Main {

​    public static void main(String[] args) {

​        Class<IntPair> clazz = IntPair.class;

​        Type t = clazz.getGenericSuperclass();

​        if (t instanceof ParameterizedType) {

​            ParameterizedType pt = (ParameterizedType) t;

​            Type[] types = pt.getActualTypeArguments(); // 可能有多个泛型类型

​            Type firstType = types[0]; // 取第一个泛型类型

​            Class<?> typeClass = (Class<?>) firstType;

​            System.out.println(typeClass); // Integer

​        }

​    }

}

class Pair<T> {

​    private T first;

​    private T last;

​    public Pair(T first, T last) {

​        this.first = first;

​        this.last = last;

​    }

​    public T getFirst() {

​        return first;

​    }

​    public T getLast() {

​        return last;

​    }

}

class IntPair extends Pair<Integer> {

​    public IntPair(Integer first, Integer last) {

​        super(first, last);

​    }

}

\```