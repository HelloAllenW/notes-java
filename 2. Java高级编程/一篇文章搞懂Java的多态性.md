众所周知，Java是一个面向对象（OOP）的编程语言，面向对象语言有封装、继承、多态这三大基本特征。下面我将通过这篇文章来告诉你什么是多态，并且通过抽象类、接口来更深入地理解多态。

\## 多态

猫和狗都是动物，它们之间既有共同点，又有自己的特点。你让猫和狗一起叫，它们都会叫，但是叫声不一样，这就是生活中的多态。OOP中，多态是指同一行为具有多种表现形式或形态的能力。

首先我们先看一个继承的例子：

\```

class Person {

​    private String name;

​    private int age;

​    public String getName() {...}

​    public void setName(String name) {...}

​    public int getAge() {...}

​    public void setAge(int age) {...}

}

class Student extends Person {

​    private int score;

​    public int getScore() { … }

​    public void setScore(int score) { … }

}

\```

\### 1. 向上转型

对于上步实现的继承，我们可以将`Person`类型的变量，指向`Student`类型的实例。这是因为`Student`继承自`Person`，因此，它拥有`Person`的全部功能。

\```

Person p = new Student();

\```

这种把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

\### 2. 多态概念

现在，我们考虑一种情况，如果子类覆写了父类的方法：

\```

public class Main {

​    public static void main(String[] args) {

​        Person p = new Student();

​        p.run(); // 应该打印Person.run还是Student.run?

​    }

}

class Person {

​    public void run() {

​        System.out.println("Person.run");

​    }

}

class Student extends Person {

​    @Override

​    public void run() {

​        System.out.println("Student.run");

​    }

}

\```

那么，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的`run()`方法？

\```

Person p = new Student();

p.run(); // 实际上调用的方法是Student的run()方法

\```

运行一下上面的代码就可以知道，实际上调用的方法是`Student`的`run()`方法。因此可得出结论：

**Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。**这个非常重要的特性在面向对象编程中称之为多态（Polymorphic）。

通过上面，我们可以得出，多态存在的三个必要条件：继承、重写（Override）、向上转型。

看到这些概念，如果对多态还不理解很正常，没关系不要放弃，继续往下看。

\### 3. 多态的作用

多态的特性就是，运行期才能动态决定调用的子类方法。对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。这种不确定性的方法调用，究竟有什么作用？

我们还是来举栗子。假设我们定义一种收入，需要给它报税，那么先定义一个`Income`类：

\```

class Income {

​    protected double income;

​    public double getTax() {

​        return income * 0.1; // 税率10%

​    }

}

\```

对于工资收入，可以减去一个基数，那么我们可以从`Income`派生出`SalaryIncome`，并覆写`getTax()`：

\```

class Salary extends Income {

​    @Override

​    public double getTax() {

​        if (income <= 5000) {

​            return 0;

​        }

​        return (income - 5000) * 0.2;

​    }

}

\```

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：

\```

class StateCouncilSpecialAllowance extends Income {

​    @Override

​    public double getTax() {

​        return 0;

​    }

}

\```

现在，我们要编写一个报税的财务软件，对于一个人的所有收入进行报税，可以这么写：

\```

public double totalTax(Income... incomes) {

​    double total = 0;

​    for (Income income: incomes) {

​        total = total + income.getTax();

​    }

​    return total;

}

\```

来试一下：

\```

public class Main {

​    public static void main(String[] args) {

​        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:

​        Income[] incomes = new Income[] {

​            new Income(3000),

​            new Salary(7500),

​            new StateCouncilSpecialAllowance(15000)

​        };

​        System.out.println(totalTax(incomes));

​    }

​    public static double totalTax(Income... incomes) {

​        double total = 0;

​        for (Income income: incomes) {

​            total = total + income.getTax();

​        }

​        return total;

​    }

}

class Income {

​    protected double income;

​    public Income(double income) {

​        this.income = income;

​    }

​    public double getTax() {

​        return income * 0.1; // 税率10%

​    }

}

class Salary extends Income {

​    public Salary(double income) {

​        super(income);

​    }

​    @Override

​    public double getTax() {

​        if (income <= 5000) {

​            return 0;

​        }

​        return (income - 5000) * 0.2;

​    }

}

class StateCouncilSpecialAllowance extends Income {

​    public StateCouncilSpecialAllowance(double income) {

​        super(income);

​    }

​    @Override

​    public double getTax() {

​        return 0;

​    }

}

\```

观察`totalTax()`方法：利用多态，`totalTax()`方法只需要和`Income`打交道，它完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在，就可以正确计算出总的税。如果我们要新增一种稿费收入，只需要从`Income`派生，然后正确覆写`getTax()`方法就可以。把新的类型传入`totalTax()`，不需要修改任何代码。

可见，多态具有一个非常强大的功能，就是允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

如果对多态还是不理解，可以看下一节中的“面向抽象编程”。

\## 抽象类

\### 1. 抽象类的引入

由于多态的存在，每个子类都可以覆写父类的方法，例如：

\```

class Person {

​    public void run() { … }

}

class Student extends Person {

​    @Override

​    public void run() { … }

}

class Teacher extends Person {

​    @Override

​    public void run() { … }

}

\```

如果父类`Person`的`run()`方法没有实际意义，能否去掉方法的执行语句？答案是不行。

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

\```

class Person {

​    public abstract void run();

}

\```

把一个方法声明为`abstract`，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，`Person`类也无法被实例化。编译器会告诉我们，无法编译`Person`类，因为它包含抽象方法。

必须把`Person`类本身也声明为`abstract`，才能正确编译它：

\```

abstract class Person {

​    public abstract void run();

}

\```

如果一个`class`定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用`abstract`修饰。因为无法执行抽象方法，因此这个类也必须申明为抽象类（abstract class）。

\### 2. 抽象类的意义

使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

\```

Person p = new Person(); // 编译错误

\```

既然无法实例化，那么抽象类到底有什么用呢？

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就必须覆写`run()`方法：

\```

public class Main {

​    public static void main(String[] args) {

​        Person p = new Student();

​        p.run();

​    }

}

abstract class Person {

​    public abstract void run();

}

class Student extends Person {

​    @Override

​    public void run() {

​        System.out.println("Student.run");

​    }

}

\```

\### 3. 面向抽象编程

当我们定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，我们可以通过抽象类`Person`类型去引用具体的子类的实例：

\```

Person s = new Student();

Person t = new Teacher();

\```

这种引用抽象类的好处在于，我们对其进行方法调用，并不关心`Person`类型变量的具体子类型：

\```

// 不关心Person变量的具体子类型:

s.run();

t.run();

\```

同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：

\```

// 同样不关心新的子类是如何实现run()方法的：

Person e = new Employee();

e.run();

\```

这种尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程。

面向抽象编程的本质就是：

\- 上层代码只定义规范（例如：`abstract class Person`）；

\- 不需要子类就可以实现业务逻辑（正常编译）；

\- 具体的业务逻辑由不同的子类实现，调用者并不关心。

\## 接口

\### 1. 接口的引入

在抽象类中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，多态就能发挥出威力。

如果一个抽象类没有字段，所有方法全部都是抽象方法：

\```

abstract class Person {

​    public abstract void run();

​    public abstract String getName();

}

\```

就可以把该抽象类改写为接口：`interface`。

\### 2. 接口的使用

在Java中，使用`interface`可以声明一个接口：

\```

interface Person {

​    void run();

​    String getName();

}

\```

所谓`interface`，就是比抽象类还要抽象的纯抽象接口，因为它连字段都不能有。因为接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。

当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：

\```

class Student implements Person {

​    private String name;

​    public Student(String name) {

​        this.name = name;

​    }

​    @Override

​    public void run() {

​        System.out.println(this.name + " run");

​    }

​    @Override

​    public String getName() {

​        return this.name;

​    }

}

\```

我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`，例如：

\```

class Student implements Person, Hello { // 实现了两个interface

​    ...

}

\```

\### 3. 接口继承

一个`interface`可以继承自另一个`interface`。`interface`继承自`interface`使用`extends`，它相当于扩展了接口的方法。例如：

\```

interface Hello {

​    void hello();

}

interface Person extends Hello {

​    void run();

​    String getName();

}

\```

此时，`Person`接口继承自`Hello`接口，因此，`Person`接口现在实际上有3个抽象方法签名，其中一个来自继承的`Hello`接口。

\### 4. default方法

在接口中，可以定义`default`方法。例如，把`Person`接口的`run()`方法改为`default`方法：

\```

public class Main {

​    public static void main(String[] args) {

​        Person p = new Student("Xiao Ming");

​        p.run();

​    }

}

interface Person {

​    String getName();

​    default void run() {

​        System.out.println(getName() + " run");

​    }

}

class Student implements Person {

​    private String name;

​    public Student(String name) {

​        this.name = name;

​    }

​    public String getName() {

​        return this.name;

​    }

}

\```

实现类可以不必覆写`default`方法。`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。