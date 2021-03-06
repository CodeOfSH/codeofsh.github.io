---
title: Java复健系列(2)：Java的类与对象
date: 2020-10-18 14:14:39
categories: Java
tags:
description: Java复健系列(2)：复习Java类、构造函数、访问修饰符等基本知识
---

# 类的定义
对示例的说明：
- public 是类的修饰符，表明该类是公共类，可以被其他类访问。修饰符将在下节讲解。
- class 是定义类的关键字。
- Dog 是类名称。
- name、age是类的成员变量，也叫属性；bark()、hungry() 是类中的函数，也叫方法。

一个类可以包含以下类型变量：
- 局部变量：在方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
- 成员变量：成员变量是定义在类中、方法体之外的变量。这种变量在创建对象的时候实例化（分配内存）。成员变量可以被类中的方法和特定类的语句访问。
- 类变量：类变量也声明在类中，方法体之外，但必须声明为static类型。static 也是修饰符的一种，将在下节讲解。

## 构造方法
- 在类实例化的过程中自动执行的方法叫做构造方法，它不需要你手动调用。构造方法可以在类实例化的过程中做一些初始化的工作。
- 构造方法的名称必须与类的名称相同，并且没有返回值。
- 每个类都有构造方法。如果没有显式地为类定义构造方法，Java编译器将会为该类提供一个默认的构造方法。

- 构造方法不能被显示调用。
- 构造方法不能有返回值，因为没有变量来接收返回值。
- 构造方法前可以添加修饰符

特性：
- 名字与类名相同。
- 没有返回值，但不能用 void 声明构造函数。
- 生成类的对象时自动执行，无需调用。

## 在 Java 中定义一个不做事且没有参数的构造方法的作用
Java 程序在执行子类的构造方法之前，如果没有用 super()来调用父类特定的构造方法，则会调用父类中“没有参数的构造方法”。因此，如果父类中只定义了有参数的构造方法，而在子类的构造方法中又没有用 super()来调用父类中特定的构造方法，则编译时将发生错误，因为 Java 程序在父类中找不到没有参数的构造方法可供执行。解决办法是在父类里加上一个不做事且没有参数的构造方法。

## 创建对象
对象是类的一个实例，创建对象的过程也叫类的实例化。对象是以类为模板来创建的。

在Java中，使用new关键字来创建对象，一般有以下三个步骤：
- 声明：声明一个对象，包括对象名称和对象类型。
- 实例化：使用关键字new来创建一个对象。
- 初始化：使用new创建对象时，会调用构造方法初始化对象。

## 对象实例与对象引用有何不同?
对象引用指向对象实例（对象引用存放在栈内存中）。一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。

## 成员变量与局部变量的区别有哪些？
1. 从语法形式上看:成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
2. 从变量在内存中的存储方式来看:如果成员变量是使用static修饰的，那么这个成员变量是属于类的，如果没有使用static修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
3. 从变量在内存中的生存时间上看:成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
4. 成员变量如果没有被赋初值:则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

# 访问修饰符
修饰符|说明
--|--
public|公有，对所有类可见
protected|受保护的，对同一包内类和所有子类可见
private|私有的，同一类内可见
默认|同一包内可见，默认无修饰符

protected and private 不能修饰类和接口

## public
由于类的继承性，类所有的public方法和变量都能被其子类继承。

## protected
基类的 protected 成员是包内可见的，并且对子类可见；
若子类与基类不在同一包中，那么在子类中，子类实例可以访问其从基类继承而来的protected方法，而不能访问基类实例的protected方法。

## private
private变量通过get/set函数访问

## 默认的：不使用任何关键字
不使用任何修饰符声明的属性和方法，对同一个包内的类是可见的。接口里的变量都隐式声明为public static final，而接口里的方法默认情况下访问权限为public。

## 关键字的继承
- 父类中声明为public的方法在子类中也必须为public。
- 父类中声明为protected的方法在子类中要么声明为protected，要么声明为public。不能声明为private。
- 父类中默认修饰符声明的方法，能够在子类中声明为private。
- 父类中声明为private的方法，不能够被继承。

# 作用域
- 类级变量：又称全局级变量或静态变量，需要使用static关键字修饰，你可以与 C/C++ 中的 static 变量对比学习。类级变量在类定义后就已经存在，占用内存空间，可以通过类名来访问，不需要实例化。
- 对象实例级变量：就是成员变量，实例化后才会分配内存空间，才能访问。
- 方法级变量：就是在方法内部定义的变量，就是局部变量。
- 块级变量：就是定义在一个块内部的变量，变量的生存周期就是这个块，出了这个块就消失了，比如 if、for 语句的块。块是指由大括号包围的代码，例如：
```
{
int age = 3;
    String name = "www.yq1012.com";
    // 正确，在块内部可以访问 age 和 name 变量
    System.out.println( name + "已经" + age + "岁了");
}
// 错误，在块外部无法访问 age 和 name 变量
System.out.println( name + "已经" + age + "岁了");

```

说明
- 方法内部除了能访问方法级的变量，还可以访问类级和实例级的变量。
- 块内部能够访问类级、实例级变量，如果块被包含在方法内部，它还可以访问方法级的变量。
- 方法级和块级的变量必须被显示地初始化，否则不能访问。

## This
this 关键字用来表示当前对象本身，或当前类的一个实例，通过 this 可以调用本对象的所有方法和属性。this 只有在类实例化后才有意义。

### 使用this区分同名变量
成员变量与方法内部的变量重名时，希望在方法内部调用成员变量，这时候只能使用this。
```
public class Demo {
	public String name;
    public int age;
    //构造方法
    public Demo(){
        this("程序员", 3);
    }
    public Demo(String name, int age){
        this.name = name;
        this.age = age;
    }
```
### 作为方法名来初始化对象
- 在构造方法中调用另一个构造方法，调用动作必须置于最起始的位置。
- 不能在构造方法以外的任何方法内调用构造方法。
- 在一个构造方法内只能调用一个构造方法。

### 作为参数传递
需要在某些完全分离的类中调用一个方法，并将当前对象的一个引用作为参数传递时。

## 成员变量与局部变量的区别有哪些？
1. 从语法形式上看:成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
2. 从变量在内存中的存储方式来看:如果成员变量是使用static修饰的，那么这个成员变量是属于类的，如果没有使用static修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
3. 从变量在内存中的生存时间上看:成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
4. 成员变量如果没有被赋初值:则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），

# 方法重载
在Java中，同一个类中的多个方法可以有相同的名字，只要它们的参数列表不同就可以，这被称为方法重载(method overloading)。参数列表又叫参数签名，包括参数的类型、参数的个数和参数的顺序，只要有一个不同就叫做参数列表不同。

重载是面向对象的一个基本特性。方法名称相同时，编译器会根据调用方法的参数个数、参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错，这叫做重载分辨。

- 参数列表不同包括：个数不同、类型不同和顺序不同。
- 仅仅参数变量名称不同是不可以的。
- 跟成员方法一样，构造方法也可以重载。
- 声明为final的方法不能被重载。
- 声明为static的方法不能被重载，但是能够被再次声明。

# 类创建时的基本运行顺序
- main
- class init
- class 构造函数
- new 完成，继续main
- 执行完毕

# 声明规则
- 一个源文件中只能有一个public类。
- 一个源文件可以有多个非public类。
- 源文件的名称应该和public类的类名保持一致。例如：源文件中public类的类名是Employee，那么源文件应该命名为Employee.java。
- 如果一个类定义在某个包中，那么package语句应该在源文件的首行。
- 如果源文件包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面。
- import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。
- 类有若干种访问级别，并且类也分不同的类型：抽象类和final类等。这些将在后续章节介绍。
- 除了上面提到的几种类型，Java还有一些特殊的类，如内部类、匿名类。

# 其他
## StringBuilder与StringBuffer
简单的来说：String 类中使用 final 关键字修饰字符数组来保存字符串，private final char value[]，所以 String 对象是不可变的。在 Java 9 之后，String 类的实现改用 byte 数组存储字符串 private final byte[] value;

而 StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串char[]value 但是没有用 final 关键字修饰，所以这两种对象都是可变的。

线程安全性：String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。

性能：每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

1. 操作少量的数据: 适用 String
2. 单线程操作字符串缓冲区下操作大量数据: 适用 StringBuilder
3. 多线程操作字符串缓冲区下操作大量数据: 适用 StringBuffer

## Object类
Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```
public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }/
```

## BigDecimal

### BigDecimal 的用处

《阿里巴巴Java开发手册》中提到：**浮点数之间的等值判断，基本数据类型不能用==来比较，包装数据类型不能用 equals 来判断。** 具体原理和浮点数的编码方式有关，这里就不多提了，我们下面直接上实例：

```java
float a = 1.0f - 0.9f;
float b = 0.9f - 0.8f;
System.out.println(a);// 0.100000024
System.out.println(b);// 0.099999964
System.out.println(a == b);// false
```
具有基本数学知识的我们很清楚的知道输出并不是我们想要的结果（**精度丢失**），我们如何解决这个问题呢？一种很常用的方法是：**使用使用 BigDecimal 来定义浮点数的值，再进行浮点数的运算操作。**

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
BigDecimal c = new BigDecimal("0.8");

BigDecimal x = a.subtract(b); 
BigDecimal y = b.subtract(c); 

System.out.println(x); /* 0.1 */
System.out.println(y); /* 0.1 */
System.out.println(Objects.equals(x, y)); /* true */
```

### BigDecimal 的大小比较

`a.compareTo(b)` : 返回 -1 表示 `a` 小于 `b`，0 表示 `a` 等于 `b` ， 1表示 `a` 大于 `b`。

```java
BigDecimal a = new BigDecimal("1.0");
BigDecimal b = new BigDecimal("0.9");
System.out.println(a.compareTo(b));// 1
```
### BigDecimal 保留几位小数

通过 `setScale`方法设置保留几位小数以及保留规则。保留规则有挺多种，不需要记，IDEA会提示。

```java
BigDecimal m = new BigDecimal("1.255433");
BigDecimal n = m.setScale(3,BigDecimal.ROUND_HALF_DOWN);
System.out.println(n);// 1.255
```

### BigDecimal 的使用注意事项

注意：我们在使用BigDecimal时，为了防止精度丢失，推荐使用它的 **BigDecimal(String)** 构造方法来创建对象。

### 总结

BigDecimal 主要用来操作（大）浮点数，BigInteger 主要用来操作大整数（超过 long 类型）。

BigDecimal 的实现利用到了 BigInteger, 所不同的是 BigDecimal 加入了小数位的概念
