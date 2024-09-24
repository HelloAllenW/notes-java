1.包装类

2.JavaBean

public class Person {

​    private String name;

​    private int age;





​    public String getName() { return this.name; }

​    public void setName(String name) { this.name = name; }





​    public int getAge() { return this.age; }

​    public void setAge(int age) { this.age = age; }

}



Java中上面这种class被称为 JavaBean。

要枚举一个JavaBean的所有属性，可以直接使用Java核心库提供的Introspector