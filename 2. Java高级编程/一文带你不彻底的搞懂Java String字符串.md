### 1. 字符串的表示

（1）在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：

\```

String s1 = "Hello!";

\```

（2）实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的：

\```

String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});

\```

因为`String`太常用了，所以Java提供了`"..."`这种字符串字面量表示方法。

（3）字面量创建字符串(`String str1 = "hello world";`)和构造函数创建字符串(`String str2 = new String("hello world");`)的区别：

运行期间字面常量 "hello world" 被存储在运行时常量池，所以每次创建字符串时`JVM `执行引擎会先在运行时常量池查找是否存在相同的字面常量，如果存在，则直接将引用指向已经存在的字面常量；否则在运行时常量池开辟一个空间来存储该字面常量，并将引用指向该字面常量。

而通过 new 关键字来生成对象是在堆区进行的，而在堆区进行对象生成的过程是不会去检测该对象是否已经存在的。因此通过 new 来创建对象，创建出的一定是不同的对象，即使字符串的内容是相同的。

\### 2. 字符串不可变性

**由于String类的本质是final修饰的char数组，所以String是不可变的。**

我们来看一个例子：

\```

public class Main {

​    public static void main(String[] args) {

​        String s = "Hello";

​        System.out.println(s); // Hello

​        s = s.toUpperCase();

​        System.out.println(s); // HELLO

​    }

}

\```

\### 3. 字符串比较

当我们想要比较两个字符串是否相同时，要特别注意，我们实际上是想比较字符串的内容是否相同。必须使用`equals()`方法而不能用`==`，因为 **== 比较的是两个对象的地址值，equals() 比较的是字面值。**

我们看下面的例子：

\```

public class Main {

​    public static void main(String[] args) {

​        String s1 = "hello";

​        String s2 = "hello";

​        System.out.println(s1 == s2);

​        System.out.println(s1.equals(s2));

​    }

}

\```

从表面上看，两个字符串用`==`和`equals()`比较都为`true`，但实际上那只是Java编译器在编译期，会自动把所有相同的字符串当作一个对象放入常量池，自然`s1`和`s2`的引用就是相同的。

所以，这种`==`比较返回`true`纯属巧合。换一种写法，`==`比较就会失败：

\```

public class Main {

​    public static void main(String[] args) {

​        String s1 = "hello";

​        String s2 = "HELLO".toLowerCase();

​        System.out.println(s1 == s2);

​        System.out.println(s1.equals(s2));

​    }

}

\```

结论：两个字符串比较，必须总是使用`equals()`方法。

要忽略大小写比较，使用`equalsIgnoreCase()`方法。

\### 4. 搜索、提取子串

`String`类还提供了多种方法来搜索子串、提取子串。常用的方法有：

\```

// 是否包含子串:

"Hello".contains("ll"); // true

\```

注意到`contains()`方法的参数是`CharSequence`而不是`String`，因为`CharSequence`是`String`实现的一个接口。

搜索子串的更多的例子：

\```

"Hello".indexOf("l"); // 2

"Hello".lastIndexOf("l"); // 3

"Hello".startsWith("He"); // true

"Hello".endsWith("lo"); // true

\```

提取子串的例子：

\```

"Hello".substring(2); // "llo"

"Hello".substring(2, 4); "ll"

\```

注意索引号是从`0`开始的。

\### 5. 去除首尾空白字符

使用`trim()`方法可以移除字符串首尾空白字符。空白字符包括空格，`\t`，`\r`，`\n`：

\```

"  \tHello\r\n ".trim(); // "Hello"

\```

注意：`trim()`并没有改变字符串的内容，而是返回了一个新字符串。

另一个`strip()`方法也可以移除字符串首尾空白字符。它和`trim()`不同的是，类似中文的空格字符`\u3000`也会被移除：

\```

"\u3000Hello\u3000".strip(); // "Hello"

" Hello ".stripLeading(); // "Hello "

" Hello ".stripTrailing(); // " Hello"

\```

`String`还提供了`isEmpty()`和`isBlank()`来判断字符串是否为空和空白字符串：

\```

"".isEmpty(); // true，因为字符串长度为0

"  ".isEmpty(); // false，因为字符串长度不为0

"  \n".isBlank(); // true，因为只包含空白字符

" Hello ".isBlank(); // false，因为包含非空白字符

\```

\### 6. 替换子串

要在字符串中替换子串，有两种方法。一种是根据字符或字符串替换：

\```

String s = "hello";

s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'

s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"

\```

另一种是通过正则表达式替换：

\```

String s = "A,,B;C ,D";

s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"

\```

上面的代码通过正则表达式，把匹配的子串统一替换为`","`。关于正则表达式的用法我们会在后面详细讲解。

\### 7. 分割字符串

要分割字符串，使用`split()`方法，并且传入的也是正则表达式：

\```

String s = "A,B,C,D";

String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}

\```

\### 8. 拼接字符串

\#### （1）`concat()`

和 `+` 的区别：`concat`方法是通过复制数组再通过 char 数组进行拼接生成一个新的对象，所以地址值会有变动，而 + 号不会。

\#### （2）join()

拼接字符串使用静态方法`join()`，它用指定的字符串连接字符串数组：

\```

String[] arr = {"A", "B", "C"};

String s = String.join("***", arr); // "A***B***C"

\```

\#### （3）`+`

Java编译器对`String`做了特殊处理，使得我们可以直接用`+`拼接字符串。

考察下面的循环代码：

\```

String s = "";

for (int i = 0; i < 1000; i++) {

​    s = s + "," + i;

}

\```

虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响`GC`效率。

**StringBuilder：**

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：

\```

StringBuilder sb = new StringBuilder(1024);

for (int i = 0; i < 1000; i++) {

​    sb.append(',');

​    sb.append(i);

}

String s = sb.toString();

\```

`StringBuilder`还可以进行链式操作：

\```

public class Main {

​    public static void main(String[] args) {

​        var sb = new StringBuilder(1024);

​        sb.append("Mr ")

​          .append("Bob")

​          .append("!")

​          .insert(0, "Hello, ");

​        System.out.println(sb.toString());

​    }

}

\```

注意：对于普通的字符串`+`操作，并不需要我们将其改写为`StringBuilder`，因为Java编译器在编译时就自动把多个连续的`+`操作编码为`StringConcatFactory`的操作。在运行期，`StringConcatFactory`会自动把字符串连接操作优化为数组复制或者`StringBuilder`操作。

你可能还听说过`StringBuffer`，这是Java早期的一个`StringBuilder`的线程安全版本，它通过同步来保证多个线程操作`StringBuffer`也是安全的，但是同步会带来执行速度的下降。`StringBuilder`和`StringBuffer`接口完全相同，现在几乎没有必要使用`StringBuffer`。

\- 如果要操作少量的数据用 String ：字符串常量，字符串长度不可变。

\- 单线程操作大量数据用`StringBuilder` ：字符串变量（非线程安全）。

\- 多线程操作大量数据，用`StringBuffer`：字符串变量（Synchronized，即线程安全）。

\#### （4）`StringJoiner`  和 `String.join()`

我们有这样一个拼接字符串的需求，采用StringBuilder实现：

\```

public class Main {

​    public static void main(String[] args) {

​        String[] names = {"Bob", "Alice", "Grace"};

​        var sb = new StringBuilder();

​        sb.append("Hello ");

​        for (String name : names) {

​            sb.append(name).append(", ");

​        }

​        // 注意去掉最后的", ":

​        sb.delete(sb.length() - 2, sb.length());

​        sb.append("!");

​        System.out.println(sb.toString()); // Hello Bob, Alice, Grace!

​    }

}

\```

类似用分隔符拼接数组的需求很常见，所以Java标准库还提供了一个`StringJoiner`来干这个事：

\```

public class Main {

​    public static void main(String[] args) {

​        String[] names = {"Bob", "Alice", "Grace"};

​        var sj = new StringJoiner(", ");

​        for (String name : names) {

​            sj.add(name);

​        }

​        System.out.println(sj.toString()); // Bob, Alice, Grace

​    }

}

\```

慢着！用`StringJoiner`的结果少了前面的`"Hello "`和结尾的`"!"`！遇到这种情况，需要给`StringJoiner`指定“开头”和“结尾”：

\```

public class Main {

​    public static void main(String[] args) {

​        String[] names = {"Bob", "Alice", "Grace"};

​        var sj = new StringJoiner(", ", "Hello ", "!");

​        for (String name : names) {

​            sj.add(name);

​        }

​        System.out.println(sj.toString()); // Hello Bob, Alice, Grace!

​    }

}

\```

`String`还提供了一个静态方法`join()`，这个方法在内部使用了`StringJoiner`来拼接字符串，在不需要指定“开头”和“结尾”的时候，用`String.join()`更方便：

\```

String[] names = {"Bob", "Alice", "Grace"};

var s = String.join(", ", names);

\```

\### 9. 格式化字符串

字符串提供了`format()`静态方法，可以传入其他参数，替换占位符，然后生成新的字符串：

\```

public class Main {

​    public static void main(String[] args) {

​        String s = "Hi %s, your score is %d!";

​        System.out.println(String.format("Hi %s, your score is %.2f!", "Bob", 59.5));

​    }

}

\```

有几个占位符，后面就传入几个参数。参数类型要和占位符一致。我们经常用这个方法来格式化信息。常用的占位符有：

\- `%s`：显示字符串；

\- `%d`：显示整数；

\- `%x`：显示十六进制整数；

\- `%f`：显示浮点数。

占位符还可以带格式，例如`%.2f`表示显示两位小数。如果你不确定用啥占位符，那就始终用`%s`，因为`%s`可以显示任何数据类型。

\### 10. 类型转换

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

\```

String.valueOf(123); // "123"

String.valueOf(45.67); // "45.67"

String.valueOf(true); // "true"

String.valueOf(new Object()); // 类似java.lang.Object@636be97c

\```

要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

\```

int n1 = Integer.parseInt("123"); // 123

int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255

\```

把字符串转换为`boolean`类型：

\```

boolean b1 = Boolean.parseBoolean("true"); // true

boolean b2 = Boolean.parseBoolean("FALSE"); // false

\```

要特别注意，`Integer`有个`getInteger(String)`方法，它不是将字符串转换为`int`，而是把该字符串对应的系统变量转换为`Integer`：

\```

Integer.getInteger("java.version"); // 版本号，11

\```

\### 11. 转换为char[]

`String`和`char[]`类型可以互相转换，方法是：

\```

char[] cs = "Hello".toCharArray(); // String -> char[]

String s = new String(cs); // char[] -> String

\```

如果修改了`char[]`数组，`String`并不会改变：

\```

public class Main {

​    public static void main(String[] args) {

​        char[] cs = "Hello".toCharArray();

​        String s = new String(cs);

​        System.out.println(s);

​        cs[0] = 'X';

​        System.out.println(s);

​    }

}

\```

这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会复制一份，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。

从`String`的不变性设计可以看出，如果传入的对象有可能改变，我们需要复制而不是直接引用。

例如，下面的代码设计了一个`Score`类保存一组学生的成绩：

\```

public class Main {

​    public static void main(String[] args) {

​        int[] scores = new int[] { 88, 77, 51, 66 };

​        Score s = new Score(scores);

​        s.printScores();

​        scores[2] = 99;

​        s.printScores();

​    }

}

class Score {

​    private int[] scores;

​    public Score(int[] scores) {

​        this.scores = scores;

​    }

​    public void printScores() {

​        System.out.println(Arrays.toString(scores));

​    }

}

\```

观察两次输出，由于`Score`内部直接引用了外部传入的`int[]`数组，这会造成外部代码对`int[]`数组的修改，影响到`Score`类的字段。如果外部代码不可信，这就会造成安全隐患。

\### 12. 字符编码

在早期的计算机系统中，为了给字符编码，美国国家标准学会（American National Standard Institute：ANSI）制定了一套英文字母、数字和常用符号的编码，它占用一个字节，编码范围从`0`到`127`，最高位始终为`0`，称为`ASCII`编码。例如，字符`'A'`的编码是`0x41`，字符`'1'`的编码是`0x31`。

如果要把汉字也纳入计算机编码，很显然一个字节是不够的。`GB2312`标准使用两个字节表示一个汉字，其中第一个字节的最高位始终为`1`，以便和`ASCII`编码区分开。例如，汉字`'中'`的`GB2312`编码是`0xd6d0`。

类似的，日文有`Shift_JIS`编码，韩文有`EUC-KR`编码，这些编码因为标准不统一，同时使用，就会产生冲突。

为了统一全球所有语言的编码，全球统一码联盟发布了`Unicode`编码，它把世界上主要语言都纳入同一个编码，这样，中文、日文、韩文和其他语言就不会冲突。

`Unicode`编码需要两个或者更多字节表示，我们可以比较中英文字符在`ASCII`、`GB2312`和`Unicode`的编码：

英文字符`'A'`的`ASCII`编码和`Unicode`编码：

\```ascii

​         ┌────┐

ASCII:   │ 41 │

​         └────┘

​         ┌────┬────┐

Unicode: │ 00 │ 41 │

​         └────┴────┘

\```

英文字符的`Unicode`编码就是简单地在前面添加一个`00`字节。

中文字符`'中'`的`GB2312`编码和`Unicode`编码：

\```ascii

​         ┌────┬────┐

GB2312:  │ d6 │ d0 │

​         └────┴────┘

​         ┌────┬────┐

Unicode: │ 4e │ 2d │

​         └────┴────┘

\```

那我们经常使用的`UTF-8`又是什么编码呢？因为英文字符的`Unicode`编码高字节总是`00`，包含大量英文的文本会浪费空间，所以，出现了`UTF-8`编码，它是一种变长编码，用来把固定长度的`Unicode`编码变成1～4字节的变长编码。通过`UTF-8`编码，英文字符`'A'`的`UTF-8`编码变为`0x41`，正好和`ASCII`码一致，而中文`'中'`的`UTF-8`编码为3字节`0xe4b8ad`。

`UTF-8`编码的另一个好处是容错能力强。如果传输过程中某些字符出错，不会影响后续字符，因为`UTF-8`编码依靠高字节位来确定一个字符究竟是几个字节，它经常用来作为传输编码。

在Java中，`char`类型实际上就是两个字节的`Unicode`编码。如果我们要手动把字符串转换成其他编码，可以这样做：

\```

byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐

byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换

byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换

byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换

\```

注意：转换编码后，就不再是`char`类型，而是`byte`类型表示的数组。

如果要把已知编码的`byte[]`转换为`String`，可以这样做：

\```

byte[] b = ...

String s1 = new String(b, "GBK"); // 按GBK转换

String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换

\```

始终牢记：Java的`String`和`char`在内存中总是以Unicode编码表示。

对于不同版本的JDK，`String`类在内存中有不同的优化方式。具体来说，早期JDK版本的`String`总是以`char[]`存储，它的定义如下：

\```

public final class String {

​    private final char[] value;

​    private final int offset;

​    private final int count;

}

\```

而较新的JDK版本的`String`则以`byte[]`存储：如果`String`仅包含ASCII字符，则每个`byte`存储一个字符，否则，每两个`byte`存储一个字符，这样做的目的是为了节省内存，因为大量的长度较短的`String`通常仅包含ASCII字符：

\```

public final class String {

​    private final byte[] value;

​    private final byte coder; // 0 = LATIN1, 1 = UTF16

\```

对于使用者来说，`String`内部的优化不影响任何已有代码，因为它的`public`方法签名是不变的。