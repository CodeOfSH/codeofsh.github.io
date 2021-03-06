---
title: Java复健系列(4)：Java中的异常处理
date: 2020-10-19 14:46:04
categories: Java
tags:
description: Java复健系列(4)：复习Java异常处理的相关知识
---

# 异常处理
Java异常是一个描述在代码段中发生的异常（也就是出错）情况的对象。当异常情况发生，一个代表该异常的对象被创建并且在导致该错误的方法中被抛出（throw）。该方法可以选择自己处理异常或传递该异常。两种情况下，该异常被捕获（catch）并处理。异常可能是由Java运行时系统产生，或者是由你的手工代码产生。被Java抛出的异常与违反语言规范或超出Java执行环境限制的基本错误有关。手工编码产生的异常基本上用于报告方法调用程序的出错状况。

Java异常处理通过5个关键字控制：try、catch、throw、throws和 finally。下面讲述它们如何工作的。程序声明了你想要的异常监控包含在一个try块中。如果在try块中发生异常，它被抛出。你的代码可以捕捉这个异常（用catch）并且用某种合理的方法处理该异常。系统产生的异常被Java运行时系统自动抛出。手动抛出一个异常，用关键字throw。任何被抛出方法的异常都必须通过throws子句定义。任何在方法返回前绝对被执行的代码被放置在finally块中。
```
try{
    // block of code to monitor for errors
}
catch (ExceptionType1 exOb) {
    // exception handler for ExceptionType1
}
catch (ExceptionType2 exOb) {
    // exception handler for ExceptionType2
}
// ...
finally {
    // block of code to be executed before try block ends
}
```

# 异常类型
所有异常类型都是内置类Throwable的子类。因此，Throwable在异常类层次结构的顶层。紧接着Throwable下面的是两个把异常分成两个不同分支的子类。一个分支是Exception。该类用于用户程序可能捕捉的异常情况。它也是你可以用来创建你自己用户异常类型子类的类。在Exception分支中有一个重要子类RuntimeException。该类型的异常自动为你所编写的程序定义并且包括被零除和非法数组索引这样的错误。

另一类分支由Error作为顶层，Error定义了在通常环境下不希望被程序捕获的异常。Error类型的异常用于Java运行时系统来显示与运行时系统本身有关的错误。堆栈溢出是这种错误的一例。本章将不讨论关于Error类型的异常处理，因为它们通常是灾难性的致命错误，不是你的程序可以控制的。
- Throwable
  - Error 系统错误
  - Exception 用户错误

如果我们没有提供任何我们自己的异常处理程序，所以异常被Java运行时系统的默认处理程序捕获。任何不是被你程序捕获的异常最终都会被该默认处理程序处理。默认处理程序显示一个描述异常的字符串，打印异常发生处的堆栈轨迹并且终止程序。

# try and catch
尽管由Java运行时系统提供的默认异常处理程序对于调试是很有用的，但通常你希望自己处理异常。这样做有两个好处。第一，它允许你修正错误。第二，它防止程序自动终止。

一个try和它的catch语句形成了一个单元。catch子句的范围限制于try语句前面所定义的语句。一个catch语句不能捕获另一个try声明所引发的异常（除非是嵌套的try语句情况）。

被try保护的语句声明必须在一个大括号之内（也就是说，它们必须在一个块中）。你不能单独使用try。

构造catch子句的目的是解决异常情况并且像错误没有发生一样继续运行。例如，下面的程序中，每一个for循环的反复得到两个随机整数。这两个整数分别被对方除，结果用来除12345。最后的结果存在a中。如果一个除法操作导致被零除错误，它将被捕获，a的值设为零，程序继续运行。
```
//Handle an exception and move on.
import java.util.Random;
class Demo {
    public static void main(String args[]) {
        int a=0, b=0, c=0;
        Random r = new Random();
        for(int i=0; i<30; i++) {
            try {
                b = r.nextInt();
                c = r.nextInt();
                a = 12345 / (b/c);
            } catch (ArithmeticException e) {
                System.out.println("Division by zero.");
                a = 0; // set a to zero and continue
            }
            System.out.println("a: " + a);
        }
    }
}
```
Throwable重载toString( )方法（由Object定义），所以它返回一个包含异常描述的字符串。你可以通过在println( )中传给异常一个参数来显示该异常的描述。
```
System.out.println("Exception: " + e);
```

# 多重catch
某些情况，由单个代码段可能引起多个异常。处理这种情况，你可以定义两个或更多的catch子句，每个子句捕获一种类型的异常。当异常被引发时，每一个catch子句被依次检查，第一个匹配异常类型的子句执行。当一个catch语句执行以后，其他的子句被旁路，执行从try/catch块以后的代码开始继续。如下代码中，如果a为0，则触发第一个异常，a为1则触发第二个异常。
```
class Demo {
    public static void main(String args[]) {
        try {
            int a = args.length;
            System.out.println("a = " + a);
            int b = 42 / a;
            int c[] = { 1 };
            c[42] = 99;
        } catch(ArithmeticException e) {
            System.out.println("Divide by 0: " + e);
        } catch(ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index oob: " + e);
        }
        System.out.println("After try/catch blocks.");
    }
}
```

当你用多catch语句时，记住异常子类必须在它们任何父类之前使用是很重要的。这是因为运用父类的catch语句将捕获该类型及其所有子类类型的异常。这样，如果子类在父类后面，子类将永远不会到达。而且，Java编译器中不能到达的代码是一个错误。编译器会在编译时提醒你相关内容。
```
//This program contains an error.
//A subclass must come before its superclass in a series of catch statements. If not,unreachable code will be created and acompile-time error will result.
//*/
class Demo {
    public static void main(String args[]) {
        try {
            int a = 0;
            int b = 42 / a;
        } catch(Exception e) {
            System.out.println("Generic Exception catch.");
        }
        /* This catch is never reached because
        ArithmeticException is a subclass of Exception. */
        catch(ArithmeticException e) { // ERROR - unreachable
            System.out.println("This is never reached.");
        }
    }
}
```
需要注意的是，一旦某个catch捕获到匹配的异常类型，将进入异常处理代码。一经处理结束，就意味着整个try-catch语句结束。其他的catch子句不再有匹配和捕获异常类型的机会。


# try嵌套
Try语句可以被嵌套。也就是说，一个try语句可以在另一个try块内部。每次进入try语句，异常的前后关系都会被推入堆栈。如果一个内部的try语句不含特殊异常的catch处理程序，堆栈将弹出，下一个try语句的catch处理程序将检查是否与之匹配。这个过程将继续直到一个catch语句匹配成功，或者是直到所有的嵌套try语句被检查耗尽。如果没有catch语句匹配，Java的运行时系统将处理这个异常。
```
//An example of nested try statements.
class Demo {
    public static void main(String args[]) {
        try {
            int a = args.length;
            /* If no command-line args are present,the following statement will generate a divide-by-zero exception. */
            int b = 42 / a;
            System.out.println("a = " + a);
            try { // nested try block
                /* If one command-line arg is used,then a divide-by-zero exception will be generated by the following code. */
                if(a==1) a = a/(a-a); // division by zero
                /* If two command-line args are used,then generate an out-of-bounds exception. */
                if(a==2) {
                    int c[] = { 1 };
                    c[42] = 99; // generate an out-of-bounds exception
                }
            } catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Array index out-of-bounds: " + e);
            }
        } catch(ArithmeticException e) {
            System.out.println("Divide by 0: " + e);
        }
    }
}
```

当有方法调用时，try语句的嵌套可以很隐蔽的发生。例如，你可以把对方法的调用放在一个try块中。在该方法内部，有另一个try语句。这种情况下，方法内部的try仍然是嵌套在外部调用该方法的try块中的。

# Throw
我们可以用throw抛出异常，语法为：
```
throw ThrowableInstance;
```
这里的ThrowableInstance一定是Throwable类类型或者Throwable子类类型的一个对象。简单的数据类型，例如int，char,以及非Throwable类，例如String或Object，不能用作异常。有两种方法可以获取Throwable对象：在catch子句中使用参数或者使用new操作符创建。

程序执行完throw语句之后立即停止；throw后面的任何语句不被执行，最邻近的try块用来检查它是否含有一个与异常类型匹配的catch语句。如果发现了匹配的块，控制转向该语句；如果没有发现，次包围的try块来检查，以此类推。如果没有发现匹配的catch块，默认异常处理程序中断程序的执行并且打印堆栈轨迹。
```
class TestThrow{
    static void proc(){
        try{
            throw new NullPointerException("demo");
        }catch(NullPointerException e){
            System.out.println("Caught inside proc");
            throw e;
        }
    }

    public static void main(String [] args){
        try{
            proc();
        }catch(NullPointerException e){
            System.out.println("Recaught: "+e);
        }
    }
}
```


# Throws
如果一个方法可以导致一个异常但不处理它，它必须指定这种行为以使方法的调用者可以保护它们自己而不发生异常。做到这点你可以在方法声明中包含一个throws子句。一个 throws 子句列举了一个方法可能抛出的所有异常类型。这对于除Error或RuntimeException及它们子类以外类型的所有异常是必要的。一个方法可以抛出的所有其他类型的异常必须在throws子句中声明。如果不这样做，将会导致编译错误。
```
type method-name(parameter-list) throws exception-list{
    // body of method
}
```
一个例子
```
class Demo {
    static void throwOne() throws IllegalAccessException {
      System.out.println("Inside throwOne.");
      throw new IllegalAccessException("demo");
   }
   public static void main(String args[]) {
      try {
         throwOne();
      } catch (IllegalAccessException e) {
         System.out.println("Caught " + e);
      }
   }
}
```

# Finally
如果一个方法打开一个文件项并关闭，然后退出，你不希望关闭文件的代码被异常处理机制旁路。finally关键字为处理这种意外而设计。finally创建一个代码块。该代码块在一个try/catch 块完成之后另一个try/catch出现之前执行。*finally块无论有没有异常抛出都会执行。*如果异常被抛出，finally甚至是在没有与该异常相匹配的catch子句情况下也将执行。一个方法将从一个try/catch块返回到调用程序的任何时候，经过一个未捕获的异常或者是一个明确的返回语句，finally子句在方法返回之前仍将执行。
```
//Demonstrate finally.
class Demo {
    // Through an exception out of the method.
    static void procA() {
        try {
           System.out.println("inside procA");
           throw new RuntimeException("demo");
        } finally {
           System.out.println("procA's finally");
        }
    }
    // Return from within a try block.
    static void procB() {
        try {
           System.out.println("inside procB");
           return;
        } finally {
           System.out.println("procB's finally");
        }
    }
    // Execute a try block normally.
    static void procC() {
        try {
           System.out.println("inside procC");
        } finally {
           System.out.println("procC's finally");
        }
    }
    public static void main(String args[]) {
       try {
          procA();
       } catch (Exception e) {
          System.out.println("Exception caught");
       }
       procB();
       procC();
    }
}
```
Output:
```
inside procA
procA’s finally
Exception caught
inside procB
procB’s finally
inside procC
procC’s finally
```

# 内置异常
在标准包java.lang中，Java定义了若干个异常类。这些异常一般是标准类RuntimeException的子类。因为java.lang实际上被所有的Java程序引入，多数从RuntimeException派生的异常都自动可用。而且，它们不需要被包含在任何方法的throws列表中。Java语言中，这被叫做未经检查的异常（unchecked exceptions ）。因为编译器不检查它来看一个方法是否处理或抛出了这些异常。java.lang中定义的未经检查的异常列于表10-1。表10-2列出了由 java.lang定义的必须在方法的throws列表中包括的异常，如果这些方法能产生其中的某个异常但是不能自己处理它。这些叫做受检查的异常（checked exceptions）。
unchecked exceptions
异常|说明
--|--
ArithmeticException|算术错误，如被0除
ArrayIndexOutOfBoundsException|数组下标出界
ArrayStoreException|数组元素赋值类型不兼容
ClassCastException|非法强制转换类型
IllegalArgumentException|调用方法的参数非法
IllegalMonitorStateException|非法监控操作，如等待一个未锁定线程
IllegalStateException|环境或应用状态不正确
IllegalThreadStateException|请求操作与当前线程状态不兼容
IndexOutOfBoundsException|某些类型索引越界
NullPointerException|非法使用空引用
NumberFormatException|字符串到数字格式非法转换
SecurityException|试图违反安全性
StringIndexOutOfBounds|试图在字符串边界之外索引
UnsupportedOperationException|遇到不支持的操作

checked exceptions
异常|说明
--|--
ClassNotFoundException|找不到类
CloneNotSupportedException|试图克隆一个不能实现Cloneable接口的对象
IllegalAccessException|对一个类的访问被拒绝
InstantiationException|试图创建一个抽象类或者抽象接口的对象
InterruptedException|一个线程被另一个线程中断
NoSuchFieldException|请求的字段不存在
NoSuchMethodException|请求的方法不存在

# 创建自定义异常子类
尽管Java的内置异常处理大多数常见错误，你也许希望建立你自己的异常类型来处理你所应用的特殊情况。这是非常简单的：只要定义Exception的一个子类就可以了（Exception当然是Throwable的一个子类）。你的子类不需要实际执行什么——它们在类型系统中的存在允许你把它们当成异常使用。Exception类自己没有定义任何方法。当然，它继承了Throwable提供的一些方法。因此，所有异常，包括你创建的，都可以获得Throwable定义的方法。这些方法显示在表10-3中。你还可以在你创建的异常类中覆盖一个或多个这样的方法。
方法|描述
Throwable fillInStackTrace( )|返回一个包含完整堆栈轨迹的Throwable对象，该对象可能被再次引发。
String getLocalizedMessage( )|返回一个异常的局部描述
String getMessage( )|返回一个异常的描述
void printStackTrace( )|显示堆栈轨迹
void printStackTrace(PrintStreamstream)|把堆栈轨迹送到指定的流
void printStackTrace(PrintWriterstream)|把堆栈轨迹送到指定的流
String toString( )|返回一个包含异常描述的String对象。当输出一个Throwable对象时，该方法被println( )调用

```
//This program creates a custom exception type.
class MyException extends Exception {
    private int detail;
    MyException(int a) {
        detail = a;
    }
    public String toString() {
        return "MyException[" + detail + "]";
    }
}
class ExceptionDemo {
    static void compute(int a) throws MyException {
        System.out.println("Called compute(" + a + ")");
        if(a > 10)
            throw new MyException(a);
        System.out.println("Normal exit");
    }
    public static void main(String args[]) {
        try {
            compute(1);
            compute(20);
        } catch (MyException e) {
            System.out.println("Caught " + e);
        }
    }
}
```
该例题定义了Exception的一个子类MyException。该子类非常简单：它只含有一个构造函数和一个重载的显示异常值的toString( )方法。ExceptionDemo类定义了一个compute( )方法。该方法抛出一个MyException对象。当compute( )的整型参数比10大时该异常被引发。

# 断言
断言用于证明和测试程序的假设，比如“这里的值大于 5”。
断言可以在运行时从代码中完全删除，所以对代码的运行速度没有影响。

断言有两种方法：
- 一种是 assert<<布尔表达式>> ；
- 另一种是 assert<<布尔表达式>> ：<<细节描述>>。

如果布尔表达式的值为false ， 将抛出AssertionError 异常；
```
public class Demo {
    public static void main(String[] args) {
        int x = 11;
        System.out.println("Testing assertion that x == 10");
        assert x == 10 : "Our assertion failed";
        System.out.println("Test passed");
    }
}
```

以上程序运行使用断言功能也需要使用额外的参数（并且需要一个数字的命令行参数），例如：
```
java -ea Demo
```

断言功能使得程序运行时抛出断言错误，注意是错误， 这意味着程序发生严重错误并且将强制退出。断言使用 boolean 值，如果其值不为 true 则 抛出 AssertionError 并终止程序的运行。

断言推荐使用方法。用于验证方法中的内部逻辑，包括：
- 内在不变式
- 控制流程不变式
- 后置条件和类不变式

运行时要屏蔽断言，可以用如下方法：
```
java –disableassertions 或 java –da 类名
```
运行时要允许断言，可以用如下方法：
```
java –enableassertions 或 java –ea类名
```

# 异常链
异常链顾名思义就是将异常发生的原因一个传一个串起来，即把底层的异常信息传给上层，这样逐层抛出。 Java API文档中给出了一个简单的模型：
```
try {   
    lowLevelOp();   
} catch (LowLevelException le) {   
    throw (HighLevelException) new HighLevelException().initCause(le);   
}
```
当程序捕获到了一个底层异常，在处理部分选择了继续抛出一个更高级别的新异常给此方法的调用者。 这样异常的原因就会逐层传递。这样，位于高层的异常递归调用getCause()方法，就可以遍历各层的异常原因。 这就是Java异常链的原理。异常链的实际应用很少，发生异常时候逐层上抛不是个好注意， 上层拿到这些异常又能奈之何？而且异常逐层上抛会消耗大量资源， 因为要保存一个完整的异常链信息.

# 深入理解
![Java Exception](./../../Pictures/java%20exception.jpg)

异常指不期而至的各种状况，如：文件找不到、网络连接失败、非法参数等。异常是一个事件，它发生在程序运行期间，干扰了正常的指令流程。Java通 过API中Throwable类的众多子类描述各种不同的异常。因而，Java异常都是对象，是Throwable子类的实例，描述了出现在一段编码中的 错误条件。当条件生成时，错误将引发异常。

## Exception and Error
Throwable： 有两个重要的子类：Exception（异常）和 Error（错误），二者都是 Java 异常处理的重要子类，各自都包含大量子类。
1. Error（错误）:是程序无法处理的错误，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。例如，Java虚拟机运行错误（Virtual MachineError），当 JVM 不再有继续执行操作所需的内存资源时，将出现 OutOfMemoryError。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止。这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，如Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。这些错误是不可查的，因为它们在应用程序的控制和处理能力之 外，而且绝大多数是程序运行时不允许出现的状况。对于设计合理的应用程序来说，即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况。在 Java中，错误通过Error的子类描述。
2. Exception（异常）:是程序本身可以处理的异常。Exception 类有一个重要的子类 RuntimeException。RuntimeException 类及其子类表示“JVM 常用操作”引发的错误。例如，若试图使用空值对象引用、除数为零或数组越界，则分别引发运行时异常NullPointerException、ArithmeticException和 ArrayIndexOutOfBoundException。

简单的说，异常能被处理，错误无法处理

## Checked Exception and Unchecked exceptions
Java的异常(包括Exception和Error)分为可查的异常（checked exceptions）和不可查的异常（unchecked exceptions）。
1. Checked Exception 可查异常（编译器要求必须处置的异常）：正确的程序在运行中，很容易出现的、情理可容的异常状况。可查异常虽然是异常状况，但在一定程度上它的发生是可以预计的，而且一旦发生这种异常状况，就必须采取某种方式进行处理。 除了RuntimeException及其子类以外，其他的Exception类及其子类都属于可查异常。这种异常的特点是Java编译器会检查它，也就是说，当程序中可能出现这类异常，要么用try-catch语句捕获它，要么用throws子句声明抛出它，否则编译不会通过。
2. Unchecked exceptions 不可查异常(编译器不要求强制处置的异常): 包括运行时异常（RuntimeException与其子类）和错误（Error）。


## 运行时异常和非运行时异常
1. 运行时异常：都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。
2. 非运行时异常 （编译异常）：是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

## 异常处理机制
Java异常处理机制中具体包括：抛出异常和捕获异常
- 抛出异常：当一个方法出现错误引发异常时，方法创建异常对象并交付运行时系统，异常对象中包含了异常类型和异常出现时的程序状态等异常信息。运行时系统负责寻找处置异常的代码并执行。
- 捕获异常：在方法抛出异常之后，运行时系统将转为寻找合适的异常处理器（exception handler）。潜在的异常处理器是异常发生时依次存留在调用栈中的方法的集合。当异常处理器所能处理的异常类型与方法抛出的异常类型相符时，即为合适 的异常处理器。运行时系统从发生异常的方法开始，依次回查调用栈中的方法，直至找到含有合适异常处理器的方法并执行。当运行时系统遍历调用栈而未找到合适 的异常处理器，则运行时系统终止。同时，意味着Java程序的终止。

- 由于运行时异常的不可查性，为了更合理、更容易地实现应用程序，Java规定，运行时异常将由Java运行时系统自动抛出，允许应用程序忽略运行时异常。
- 对于方法运行中可能出现的Error，当运行方法不欲捕捉时，Java允许该方法不做任何抛出声明。因为，大多数Error异常属于永远不能被允许发生的状况，也属于合理的应用程序不该捕捉的异常。
- 对于所有的可查异常，Java规定：一个方法必须捕捉，或者声明抛出方法之外。也就是说，当一个方法选择不捕捉可查异常时，它必须声明将抛出异常。

Java规定，对于可查异常必须捕捉、或者声明抛出。允许忽略不可查的RuntimeException和Error。

![Java Exception](./../../Pictures/java%20exception2.jpg)


# 面试问题
1. try{} 里有一个 return 语句，那么紧跟在这个 try 后的 finally{} 里的 code 会不会被执行，什么时候被执行，在 return 前还是后?
答案：会执行，在方法返回调用者前执行。注意：在finally中改变返回值的做法是不好的，因为如果存在finally代码块，try中的return语句不会立马返回调用者，而是记录下返回值待finally代码块执行完毕之后再向调用者返回其值，然后如果在finally中修改了返回值，就会返回修改后的值。显然，在finally中返回或者修改返回值会对程序造成很大的困扰，C#中直接用编译错误的方式来阻止程序员干这种龌龊的事情，Java中也可以通过提升编译器的语法检查级别来产生警告或错误，Eclipse中可以在如图所示的地方进行设置，强烈建议将此项设置为编译错误。

2. Java语言如何进行异常处理，关键字：throws、throw、try、catch、finally分别如何使用？
答：Java通过面向对象的方法进行异常处理，把各种不同的异常进行分类，并提供了良好的接口。在Java中，每个异常都是一个对象，它是Throwable类或其子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并可以对其进行处理。Java的异常处理是通过5个关键词来实现的：try、catch、throw、throws和finally。一般情况下是用try来执行一段程序，如果系统会抛出（throw）一个异常对象，可以通过它的类型来捕获（catch）它，或通过总是执行代码块（finally）来处理；try用来指定一块预防所有异常的程序；catch子句紧跟在try块后面，用来指定你想要捕获的异常的类型；throw语句用来明确地抛出一个异常；throws用来声明一个方法可能抛出的各种异常（当然声明异常时允许无病呻吟）；finally为确保一段代码不管发生什么异常状况都要被执行；try语句可以嵌套，每当遇到一个try语句，异常的结构就会被放入异常栈中，直到所有的try语句都完成。如果下一级的try语句没有对某种异常进行处理，异常栈就会执行出栈操作，直到遇到有处理这种异常的try语句或者最终将异常抛给JVM。

3. 运行时异常与受检异常有何异同？ 
异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生。受检异常跟程序运行的上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发。Java编译器要求方法必须声明抛出可能发生的受检异常，但是并不要求必须声明抛出未被捕获的运行时异常。异常和继承一样，是面向对象程序设计中经常被滥用的东西，在Effective Java中对异常的使用给出了以下指导原则： 
- 不要将异常处理用于正常的控制流（设计良好的API不应该强迫它的调用者为了正常的控制流而使用异常） 
- 对可以恢复的情况使用受检异常，对编程错误使用运行时异常 
- 避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生） 
- 优先使用标准的异常 
- 每个方法抛出的异常都要有文档 
- 保持异常的原子性 
- 不要在catch中忽略掉捕获到的异常

1. 列出一些你常见的运行时异常？
- ArithmeticException（算术异常） 
- ClassCastException （类转换异常） 
- IllegalArgumentException （非法参数异常） 
- IndexOutOfBoundsException （下标越界异常） 
- NullPointerException （空指针异常） 
- SecurityException （安全异常）