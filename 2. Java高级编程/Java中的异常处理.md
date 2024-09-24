## 异常的分类

Java内置了一套异常处理机制，总是使用异常来表示错误。异常是一种class，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了。异常class的继承关系如下：

![](https://cdn.jsdelivr.net/gh/HelloAllenW/BlogAssets/images/202403201012266.png)

`Throwable` 是 Java 语言中所有错误与异常的超类。`Throwable`有两个体系：`Error` 和 `Exception`。

（1）**Error** 表示严重的错误，程序对此一般无能为力，例如：

\- `OutOfMemoryError`：内存耗尽

\- `NoClassDefFoundError`：无法加载某个Class

\- `StackOverflowError`：栈溢出

（2）**Exception** 是程序本身可以捕获并且可以处理的异常。Exception 这种异常又分为两类：运行时异常和编译时异常。

运行时异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

\- `NullPointerException`：对某个null的对象调用方法或字段

\- `IndexOutOfBoundsException`：数组索引越界

运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。

编译时异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

\- `NumberFormatException`：数值类型的格式错误

\- `FileNotFoundException`：未找到文件

\- `SocketException`：读取网络失败

（3）Java规定：

\- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为可查异常（`Checked Exception`）。

\- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。这种类型的异常称为不可查异常。

用一个例子来说明：

\```

public class Main {

​    public static void main(String[] args) {

​        byte[] bs = toGBK("中文");

​        System.out.println(Arrays.toString(bs));

​    }

​    static byte[] toGBK(String s) {

​        try {

​            // 用指定编码转换String为byte[]:

​            return s.getBytes("GBK");

​        } catch (UnsupportedEncodingException e) {

​            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:

​            System.out.println(e); // 打印异常信息

​            return s.getBytes(); // 尝试使用用默认编码

​        }

​    }

}

\```

1、如果我们不捕获`UnsupportedEncodingException`，会出现编译失败的问题。

\```

public class Main {

​    public static void main(String[] args) {

​        byte[] bs = toGBK("中文");

​        System.out.println(Arrays.toString(bs));

​    }

​    static byte[] toGBK(String s) {

​        return s.getBytes("GBK");

​    }

}

\```

错误信息类似：`unreported exception UnsupportedEncodingException; must be caught or declared to be thrown`，并且准确地指出需要捕获的语句是`return s.getBytes("GBK");`。意思是说，像`UnsupportedEncodingException`这样的`Checked Exception`，必须被捕获。

这是因为`String.getBytes(String)`方法定义是：

\```

public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {

​    ...

}

\```

在方法定义的时候，使用`throws xxx`表示该方法可能抛出的异常类型。调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。

2、当然，我们也可以不捕获它，而是在方法定义处用`throws`表示`toGBK()`方法可能会抛出`UnsupportedEncodingException`，就可以让`toGBK()`方法通过编译器检查：

\```

public class Main {

​    public static void main(String[] args) {

​        byte[] bs = toGBK("中文");

​        System.out.println(Arrays.toString(bs));

​    }

​    static byte[] toGBK(String s) throws UnsupportedEncodingException {

​        return s.getBytes("GBK");

​    }

}

\```

3、上述代码仍然会得到编译错误，但这一次，编译器提示的不是调用`return s.getBytes("GBK");`的问题，而是`byte[] bs = toGBK("中文");`。因为在main()方法中，调用`toGBK()`，没有捕获它声明的可能抛出的`UnsupportedEncodingException`。修复方法是在main()方法中捕获异常并处理：

\```

public class Main {

​    public static void main(String[] args) {

​        try {

​            byte[] bs = toGBK("中文");

​            System.out.println(Arrays.toString(bs));

​        } catch (UnsupportedEncodingException e) {

​            System.out.println(e);

​        }

​    }

​    static byte[] toGBK(String s) throws UnsupportedEncodingException {

​        // 用指定编码转换String为byte[]:

​        return s.getBytes("GBK");

​    }

}

\```

可见，只要是方法声明的`Checked Exception`，不在调用层捕获，也必须在更高的调用层捕获。所有未捕获的异常，最终也必须在main()方法中捕获，不会出现漏写try的情况。这是由编译器保证的。main()方法也是最后捕获Exception的机会。

4、如果是测试代码，上面的写法就略显麻烦。如果不想写任何try代码，可以直接把main()方法定义为`throws Exception`：

\```

public class Main {

​    public static void main(String[] args) throws Exception {

​        byte[] bs = toGBK("中文");

​        System.out.println(Arrays.toString(bs));

​    }

​    static byte[] toGBK(String s) throws UnsupportedEncodingException {

​        // 用指定编码转换String为byte[]:

​        return s.getBytes("GBK");

​    }

}

\```

因为main()方法声明了可能抛出Exception，也就声明了可能抛出所有的Exception，因此在内部就无需捕获了。代价就是一旦发生异常，程序会立刻退出。

5、还有一些童鞋喜欢在`toGBK()`内部“消化”异常：

\```

static byte[] toGBK(String s) {

​    try {

​        return s.getBytes("GBK");

​    } catch (UnsupportedEncodingException e) {

​        // 什么也不干

​    }

​    return null;

}

\```

这种捕获后不处理的方式是非常不好的，即使真的什么也做不了，也要先把异常记录下来：

\```

static byte[] toGBK(String s) {

​    try {

​        return s.getBytes("GBK");

​    } catch (UnsupportedEncodingException e) {

​        // 先记下来再说:

​        e.printStackTrace();

​    }

​    return null;

}

\```

所有异常都可以调用`printStackTrace()`方法打印异常栈，这是一个简单有用的快速打印异常的方法。

\## 捕获异常

1、多catch语句

多个catch语句只有一个能被执行， 所以存在多个catch的时候，子类必须写在前面。

\```

public static void main(String[] args) {

​    try {

​        process1();

​        process2();

​        process3();

​    } catch (IOException e) {

​        System.out.println("IO error");

​    } catch (UnsupportedEncodingException e) { // 永远捕获不到

​        System.out.println("Bad encoding");

​    }

}

\```

2、finally语句

finally语句块保证有无错误都会执行。

某些情况下，可以没有catch，只使用`try ... finally`结构。例如：

\```

void process(String file) throws IOException {

​    try {

​        ...

​    } finally {

​        System.out.println("END");

​    }

}

\```

因为方法声明了可能抛出的异常，所以可以不写catch。

3、捕获多种异常

如果处理`IOException`和`NumberFormatException`的代码是相同的，那么我们可以把它两用`|`合并到一起：

\```

public static void main(String[] args) {

​    try {

​        process1();

​        process2();

​        process3();

​    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException

​        System.out.println("Bad input");

​    } catch (Exception e) {

​        System.out.println("Unknown error");

​    }

}

\```

\## 抛出异常

1、异常的传播

当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止

2、抛出异常

抛出异常分两步：创建某个Exception的实例；用`throw`语句抛出。

\```

public static double method(int value) {

​    if(value == 0) {

​        throw new ArithmeticException("参数不能为0"); //抛出一个运行时异常

​    }

​    return 5.0 / value;

}

\```

如果在catch中抛出异常，不会影响finally的执行。`JVM`会先执行finally，然后抛出异常。

finally抛出异常后，原来在catch中准备抛出的异常就“消失”了，因为只能抛出一个异常。没有被抛出的异常称为“被屏蔽”的异常（Suppressed Exception）。

在极少数的情况下，我们需要获知所有的异常。如何保存所有的异常信息？方法是先用origin变量保存原始异常，然后调用`Throwable.addSuppressed()`，把原始异常添加进来，最后在finally抛出。

\## 自定义异常

当我们在代码中需要抛出异常时，尽量使用JDK已定义的异常类型。但在一个大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。

一个常见的做法是自定义一个`BaseException`作为“根异常”。然后，派生出各种业务类型的异常。

BaseException需要从一个适合的Exception派生，通常建议从RuntimeException派生：

\```

public class BaseException extends RuntimeException {

}

\```

其他业务类型的异常就可以从BaseException派生：

\```

public class UserNotFoundException extends BaseException {

}

public class LoginFailedException extends BaseException {

}

...

\```

自定义的BaseException应该提供多个构造方法：

\```

public class BaseException extends RuntimeException {

​    public BaseException() {

​        super();

​    }

​    public BaseException(String message, Throwable cause) {

​        super(message, cause);

​    }

​    public BaseException(String message) {

​        super(message);

​    }

​    public BaseException(Throwable cause) {

​        super(cause);

​    }

}

\```

\## NullPointerException

NullPointerException是Java代码常见的逻辑错误，应当早暴露，早修复；

可以启用Java 14的增强异常信息来查看NullPointerException的详细错误信息。

\## 断言

断言是一种调试方式，断言失败会抛出`AssertionError`，只能在开发和测试阶段启用断言；

对可恢复的错误不能使用断言，而应该抛出异常；

断言很少被使用，更好的方法是编写单元测试。

\## 日志

1、JDK Logging

日志是为了替代System.out.println()，可以定义格式，重定向到文件等；

日志可以存档，便于追踪问题；

日志记录可以按级别分类，便于打开或关闭某些级别；

可以根据配置文件调整日志，无需修改代码；

Java标准库提供了java.util.logging来实现日志功能。

2、Commons Logging

Commons Logging是使用最广泛的日志模块；

Commons Logging的API非常简单；

Commons Logging可以自动检测并使用其他日志模块。

3、Log4j

通过Commons Logging实现日志，不需要修改代码即可使用Log4j；

使用Log4j只需要把log4j2.xml和相关jar放入classpath；

如果要更换Log4j，只需要移除log4j2.xml和相关jar；

只有扩展Log4j时，才需要引用Log4j的接口（例如，将日志加密写入数据库的功能，需要自己开发）。

4、SLF4J和Logback

SLF4J和Logback可以取代Commons Logging和Log4j；

始终使用SLF4J的接口写入日志，使用Logback只需要配置，不需要修改代码。