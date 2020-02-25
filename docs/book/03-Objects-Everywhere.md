[TOC]

# 第三章 万物皆对象

## 对象操纵

所有的编程语言都会操纵内存中的元素。在 C/C++ 中，对象的操纵是通过指针来完成的。

Java 利用万物皆对象的思想和单一语法方式来简化问题。我们所操纵的标识符实际上是对象的“引用”，代码示例：

```java
    String s;
```

这里创建了一个 **String** 对象的引用，而非对象。直接使用会出错：因为没有初始化。代码示例：

```java
    String s = "asdf"; // 双引号创建字符串
```

## 对象创建

“引用”用来关联“对象”。在 Java 中，通常我们使用`new`操作符来创建一个新对象。

```java
    String s = new String("asdf");
```

### 数据存储

那么，程序在运行时是如何存储的呢？尤其是内存是怎么分配的。有5个不同的地方可以存储数据：

1. **寄存器**（Registers）最快的存储区域，位于 CPU 内部。然而，寄存器的数量十分有限，所以寄存器根据需求进行分配。我们对其无法直接控制，也无法在程序里找到寄存器的踪迹。

2. **栈内存**（Stack）存在于常规内存 RAM（随机访问存储器，Random Access Memory）区域中，可通过栈指针获得处理器的直接支持。栈指针下移分配内存，上移释放内存，这是一种快速有效的内存分配方法，速度仅次于寄存器。创建程序时，Java 系统必须准确地知道栈内保存的所有项的生命周期。这种约束限制了程序的灵活性。因此，虽然在栈内存上存在一些 Java 数据，特别是对象引用，但 Java 对象却是保存在堆内存的。

3. **堆内存**（Heap）这是一种通用的内存池（也在 RAM 区域），所有 Java 对象都存在于其中。与栈内存不同，编译器不需要知道对象必须在堆内存上停留多长时间。因此，用堆内存保存数据更具灵活性。创建一个对象时，只需用 `new` 命令实例化对象即可，当执行代码时，会自动在堆中进行内存分配。这种灵活性是有代价的：分配和清理堆内存要比栈内存需要更多的时间。随着时间的推移，Java 的堆内存分配机制现在已经非常快。

4. **常量存储**（Constant storage）常量值通常直接放在程序代码中，因为它们永远不会改变。如需严格保护，可考虑将它们置于只读存储器 ROM （只读存储器，Read Only Memory）中(如字符串常量池)。

5. **非 RAM 存储**（Non-RAM storage）数据完全存在于程序之外，在程序未运行以及脱离程序控制后依然存在。两个例子：（1）序列化对象：对象被转换为字节流，通常被发送到另一台机器；（2）持久化对象：对象被放置在磁盘上。这些存储的方式都是将对象转存于另一个介质中，并在需要时恢复成常规的、基于 RAM 的对象。Java 为轻量级持久化提供了支持。而诸如 JDBC 和 Hibernate 这些类库为使用数据库存储和检索对象信息提供了更复杂的支持。

### 基本类型的存储

通常 `new` 出来的对象都是保存在堆内存中的，以此方式创建小而简单的变量是不划算的。所以对于这些基本类型的创建方法，Java 使用了和 C/C++ 一样的策略，不用new，而是直接存储"值"，并置于栈内存中，因此更加高效。

Java 确定了每种基本类型的内存占用大小。这些大小不会随着机器环境的变化而变化(方便移植)。

| 基本类型  |    大小 |  最小值  | 最大值  | 包装类型 |
| :------: | :------: | :------: | :------: | :------: |
| boolean | —  | — | — | Boolean |
| char | 16 bits | Unicode 0  | Unicode 2<sup>16</sup> -1  | Character |
| byte | 8 bits | -128 | +127 | Byte |
| short | 16 bits | - 2<sup>15</sup> | + 2<sup>15</sup> -1 | Short |
| int | 32 bits | - 2<sup>31</sup> | + 2<sup>31</sup> -1 | Integer |
| long | 64 bits | - 2<sup>63</sup> | + 2<sup>63</sup> -1 | Long |
| float | 32 bits | IEEE754 | IEEE754 | Float |
| double | 64 bits |IEEE754 | IEEE754 | Double |
| void | — | — | — | Void |

所有的数值类型都是有正/负符号的。布尔（boolean）类型的大小没有明确的规定。基本类型有对应的包装类型，如果你希望在堆内存里表示基本类型的数据，就需要用到它们的包装类。代码示例：

```java
char c = 'x';
Character ch = new Character(c);
```

或者你也可以使用下面的形式：

```java
Character ch = new Character('x');
```

基本类型自动转换成包装类型（自动装箱）

```java
Character ch = 'x';
```

相对的，包装类型转化为基本类型（自动拆箱）：

```java
char c = ch;
```

### 高精度数值

在 Java 中有两种类型的数据可用于高精度的计算: `BigInteger` 和 `BigDecimal`。尽管它们大致可以划归为“包装类型”，但是它们并没有对应的基本类型。

这两个类与对基本类型执行的操作相似。也就是，能对 int 或 float 做的运算，在 BigInteger 和 BigDecimal 也同样可以，只不过要通过调用方法来实现而非运算符。此外，由于涉及到的计算量更多，所以运算速度会慢一些。

BigInteger 支持任意精度的整数。可用于精确表示任意大小的整数值，同时在运算过程中不会丢失精度。
BigDecimal 支持任意精度的定点数字。例如，可用它进行精确的货币计算。

### 数组的存储

Java 的设计主要目标之一是安全性，在 Java 中，数组使用前需要被初始化，并且不能访问数组长度以外的数据。这种范围检查，是以每个数组上少量的内存开销及运行时检查下标的额外时间为代价的，但由此换来的安全性和效率的提高是值得的。

当我们创建对象数组时，实际上是创建了一个引用数组，初始值为 **null** 。在使用前，需要初始化。如果我们尝试使用为 **null** 的引用，则会在运行时报错。

我们还可创建基本类型的数组。编译器通过将该数组的内存全部置零来保证初始化。

## 代码注释

Java 中有两种类型的注释。第一种是传统的 C 风格：

```java
/* 这是
* 跨越多行的
* 注释
*/
```

第二种注释形式来自 C++ 。它是单行注释，以 `//` 开头并一直持续到行结束。

```java
// 这是单行注释
```

## 对象清理

在一些编程语言中，管理变量的生命周期需要大量的工作。一个变量需要存活多久？如果我们想销毁它，应该什么时候去做呢？变量生命周期的混乱会导致许多 bug，本小结向你介绍 Java 是如何通过释放存储来简化这个问题的。

### 作用域

大多数程序语言都有作用域的概念。作用域决定了在该范围内定义的变量名的可见性和生存周期。在 C、 C++ 和 Java 中，作用域是由大括号 `{}` 的位置决定的。例如：

```java
{
    int x = 12;
    // 仅 x 变量可用
    {
        int q = 96;
        // x 和 q 变量皆可用
    }
    // 仅 x 变量可用
    // 变量 q 不在作用域内
}
```

Java 的变量只有在其作用域内才可用。由于 Java 是一种自由格式的语言，额外的空格、制表符和回车并不会影响程序的执行结果。在 Java 中，你不能执行以下操作：

```java
{
    int x = 12;
    {
        int x = 96; // Illegal
    }
}
```

在上例中， Java 编译器会在提示变量 x 已经被定义过了，所以会报错。因为 Java 的设计者认为这样做会导致程序混乱。

### 对象作用域

对象与基本类型具有不同的生命周期。当我们使用 `new` 关键字来创建对象时，它的生命周期将会超出作用域。代码示例：

```java
{
    String s = new String("a string");
} 
// 作用域终点
```

上例中，引用 s 在作用域终点就结束了。但是，引用 s 指向的字符串对象依然还在占用内存。在这段代码中，我们无法在这个作用域之后访问这个对象，因为唯一对它的引用 s 已超出了作用域的范围。

只要你需要，`new` 出来的对象就会一直存活下去。Java 的垃圾收集器会检查所有 `new` 出来的对象并判断哪些不再可达，继而释放那些被占用的内存，供其他新的对象使用。也就是说，我们不必担心内存回收的问题了。你只需简单创建对象即可。当其不再被需要时，能自行被垃圾收集器释放。垃圾回收机制有效防止了因程序员忘记释放内存而造成的“内存泄漏”问题。

## 类的创建

### 类型

如果一切都是对象，是什么确定了对象的类型(对象的属性和行为)？是类。

```java
class ATypeName {
 // 这里是类的内部
}
```

在上例中，我们引入了一个新的类型，尽管这个类里只有一行注释。但是我们一样可以通过 `new` 关键字来创建一个这种类型的对象。如下：

```java
ATypeName a = new ATypeName();
```

### 字段

当我们创建好一个类之后，我们可以往类里存放两种类型的元素：方法（method）和字段（field）。类的字段可以是基本类型，也可以是引用类型。如果类的字段是对某个对象的引用，那么必须要初始化该引用将其关联到一个实际的对象上（通过之前介绍的创建对象的方法）。每个对象都有用来存储其字段的空间。通常，字段不在对象间共享。下面是一个具有某些字段的类的代码示例：

```java
class DataOnly {
    int i;
    double d;
    boolean b;
}
```

这个类除了存储数据之外什么也不能做。但是，我们仍然可以通过下面的代码来创建它的一个对象：

```java
    DataOnly data = new DataOnly();
```

我们必须通过这个对象的引用来指定字段值。格式：对象名称.方法名称或字段名称。代码示例：

```java
    data.i = 47;
    data.d = 1.1;
    data.b = false;
```

如果你想修改对象内部包含的另一个对象的数据，可以通过这样的格式修改。代码示例：

```java
    myPlane.leftTank.capacity = 100;
```

你可以用这种方式嵌套许多对象（尽管这样的设计会带来混乱）。

### 基本类型默认值

如果类的成员变量（字段）是基本类型，那么在类初始化时，这些类型将会被赋予一个初始值。

| 基本类型 | 初始值 |
| :-----: |:-----: |
| boolean | false |
| char | \u0000 (null) |
| byte | (byte) 0 |
| short |(short) 0 |
| int | 0 |
| long | 0L |
| float | 0.0f |
| double | 0.0d |

这些默认值仅在 Java 初始化类的时候才会被赋予。这种方式确保了基本类型的字段始终能被初始化，从而减少bug，为了安全，最好始终显式地初始化变量。

这种默认值的赋予并不适用于局部变量。如果使用了没有初始化的局部变量。 Java 将会报错。

### 方法使用

在 Java 中，我们使用术语 *方法*（method）来表示“做某事的方式”。 方法决定对象能接收哪些消息。基本组成部分包括名称、参数、返回类型、方法体。格式如：

```java
 [返回类型] [方法名](/*参数列表*/){
     // 方法体
 }
```

#### 返回类型

返回类型表明了方法的返回结果类型。参数列表则是传递给方法的参数类型及名称。方法名和参数列表统称为**方法签名**。签名作为方法的唯一标识。

Java 中的方法只能作为类的一部分创建。它只能被对象所调用，并且该对象必须有权限来执行调用。若对象调用错误的方法，则程序将在编译时报错。

我们可以像下面这样调用一个对象的方法：

```java
[对象引用].[方法名](参数1, 参数2, 参数3);
```

若方法不带参数，例如：

```java
int x = a.f();
```

上例中方法 `f` 的返回值类型必须和变量 `x` 的类型兼容。

#### 参数列表

方法参数列表指定了传递给方法的信息。参数列表必须指定每个对象的类型和名称。同样，我们并没有直接处理对象，而是在传递对象引用。但是引用的类型必须是正确的。如果方法需要 String 参数，则必须传入 String，否则编译器将报错。

```java
int storage(String s) {
    return s.length() * 2;
}
```

如果我们不想方法返回数据，则可以通过给方法标识 `void` 来表明这是一个无需返回值的方法。 代码示例：

```java
boolean flag() { 
    return true; 
}

double naturalLogBase() { 
    return 2.718; 
}

void nothing() {
     return;
}

void nothing2() { 

}
```

当返回类型为 **void** 时， **return** 关键字仅用于退出方法，因此末尾的 **return** 可被省略。但若方法返回类型为非 `void`，则编译器会强制我们返回相应类型的值。

## 程序编写

在看到第一个 Java 程序之前，我们还必须理解其他几个问题。

### 命名可见性

命名控制在任何一门编程语言中都是一个问题。如果你在两个模块中使用相同的命名，那么如何区分这两个名称，并防止两个名称发生“冲突”呢？

Java 为一个类库生成一个明确的名称，Java 创建者希望我们反向使用自己的域名，因为域名通常是唯一的。因此我的域名是 MindviewInc.com，所以我将我的 foibles 类库命名为 com.mindviewinc.utility.foibles。反转域名后，`.` 用来代表子目录的划分。

在 Java 1.0 和 Java 1.1 中，域扩展名 com、 edu、 org 和 net 等按惯例大写，因此类库中会出现这样类似的名称：Com.mindviewinc.utility.foibles。然而，在 Java 2 的开发过程中，他们发现这会导致问题，所以现在整个包名都是小写的。此机制意味着所有文件都自动存在于自己的命名空间中，文件中的每个类都具有唯一标识符。这样，Java 语言可以防止名称冲突。

### 使用其他组件

无论何时在程序中使用预先定义好的类，编译器都必须找到该类。

要解决此问题，你必须通过使用 **import** 关键字来告诉 Java 编译器具体要使用的类。**import** 让编译器导入一个包。例如：

```java
import java.util.ArrayList;
```

上例可以告诉编译器使用位于标准库 **util** 下的 ArrayList 类。但是，**util** 中包含许多类，我们可以使用通配符 `*` 来导入其中部分类，而无需显式得逐一声明这些类。代码示例：

```java
import java.util.*;
```

### static关键字

类是对象的外观及行为的描述。通常只有在 `new` 创建对象后，数据存储空间才被分配，对象的方法才能调用。这种方式在两种情况下是不足的。

1. 有时你只想为特定属性分配一个共享存储空间，而不去考虑究竟要创建多少对象，甚至根本就不创建对象。

2. 创建一个与此类的任何对象无关的方法。也就是说，即使没有创建对象，也能调用该方法。

**static** 关键字符合上述要求。当我们说某个事物是静态时，就意味着该字段或方法不依赖于特定的对象实例，只与类有关。相反，对于非静态属性和方法，我们必须要先创建对象来访问字段或方法。

我们可以在类的字段或方法前添加 `static` 关键字来表示这是一个静态字段或静态方法。 代码示例：

```java
class StaticTest {
    static int i = 47;
}
```

现在，即使你创建了两个 `StaticTest` 对象，但是静态变量 `i` 仍只占一份存储空间。两个对象都会共享相同的变量 `i`。 代码示例：

```java
StaticTest st1 = new StaticTest();
StaticTest st2 = new StaticTest();
```

`st1.i` 和 `st2.i` 指向同一块存储空间，因此它们的值都是 47。引用静态变量有两种方法。通过一个对象来定位它，例如 `st2.i`。或者通过类名引用它(推荐，一看就知道是静态属性)。

```java
StaticTest.i++;
```

`++` 运算符将会使变量结果 + 1。此时 `st1.i` 和 `st2.i` 的值都变成了 48。

同样适用静态方法。 代码示例：

```java
class Incrementable {
    static void increment() { 
      StaticTest.i++; 
    }
}

Incrementable sf = new Incrementable();
sf.increment();
```

当然了，首选的方法是直接通过类来调用它。代码示例：

```java
Incrementable.increment()；
```

当 `static` 关键字修饰方法时，无需创建对象就可以直接通过类的引用来调用该方法。`static` 关键字的这些特性对于应用程序入口点的 `main()` 方法尤为重要(不需要确认某个对象)。

## 小试牛刀

最后，我们开始编写第一个完整的程序。我们使用 Java 标准库中的 **Date** 类来展示一个字符串和日期。

```java

// objects/HelloDate.java
import java.util.*;

public class HelloDate {
    public static void main(String[] args) {
        System.out.println("Hello, it's: ");
        System.out.println(new Date());
    }
}
```

如果你想在代码中使用一些额外的类库，那么就必须使用 **import** 导入它们。有一些类库已经默认自动导入到每个文件里了。例如：`java.lang` 包。

现在打开 [Oracle](https://www.oracle.com/) 查看文档。你会看到 Java 附带的所有不同的类库。

使用浏览器的“查找”功能查找 **Date**，搜索结果中将会列出 **java.util.Date**，所以必须导入 **java.util.*** 才能使用 **Date**。

如果你在文档中选择 **java.lang**，然后选择 **System**，你会看到 **System** 类中有几个字段，如果你选择了 **out**，你会发现它是一个静态的 **PrintStream** 对象。所以，即使不使用 **new** 创建， **out** 对象就可以使用。现在我们重点说的是 **println()** 方法。 它的作用是 “将信息输出到控制台，并以换行符结束”。代码示例：

```java
System.out.println("A String of things");
```

每个 java 源文件中允许有多个类。同时，源文件的名称必须要和其中一个类名相同，否则编译器将会报错。每个独立的程序应该包含一个 `main()` 方法作为程序运行的入口。其方法签名和返回类型如下。代码示例：

```java
public static void main(String[] args) {
}
```

关键字 **public** 表示方法可以被外界访问到。**main()** 方法的参数是一个 字符串（**String**） 数组。 参数 **args** 并没有在当前的程序中使用到，但是 Java 编译器强制要求必须要有， 这是因为它们被用于接收从命令行输入的参数。

下面我们来看一段有趣的代码：

```java
System.out.println(new Date());
```

上面的示例中，我们创建了一个日期（**Date**）类型的对象并将其转化为字符串类型，输出到控制台中。 一旦这一行语句执行完毕，我们就不再需要该日期对象了。这时，Java 垃圾回收器就可以将其占用的内存回收，我们无需去主动清除它们。

查看 JDK 文档时，我们可以看到在 **System** 类下还有很多其他有用的方法。代码示例：

```java
// objects/ShowProperties.java
public class ShowProperties {
    public static void main(String[] args) {
        System.getProperties().list(System.out);
        System.out.println(System.getProperty("user.name"));
        System.out.println(System.getProperty("java.library.path"));
    }
}
```

输出结果(前20行):

```text
java.runtime.name=Java(TM) SE Runtime Environment
sun.boot.library.path=C:\Program
Files\Java\jdk1.8.0_112\jr...
java.vm.version=25.112-b15
java.vm.vendor=Oracle Corporation
java.vendor.url=http://java.oracle.com/
path.separator=;
java.vm.name=Java HotSpot(TM) 64-Bit Server VM
file.encoding.pkg=sun.io
user.script=
user.country=US
sun.java.launcher=SUN_STANDARD
sun.os.patch.level=
java.vm.specification.name=Java Virtual Machine
Specification
user.dir=C:\Users\Bruce\Documents\GitHub\on-ja...
java.runtime.version=1.8.0_112-b15
java.awt.graphicsenv=sun.awt.Win32GraphicsEnvironment
java.endorsed.dirs=C:\Program
Files\Java\jdk1.8.0_112\jr...
os.arch=amd64
java.io.tmpdir=C:\Users\Bruce\AppData\Local\Temp\
```

`main()` 方法中的第一行会输出所有的系统字段，也就是环境信息。 **list()** 方法将结果发送给它的参数 **System.out**。在本书的后面，我们还会接触到将结果输出到其他地方，例如文件中。另外，我们还可以请求特定的字段。该例中我们使用到了 **user.name** 和 **java.library.path**。 

### 编译和运行

要编译和运行本书中的代码示例，首先必须具有 Java 编程环境。

``` bash
javac HelloDate.java
```

此命令不应产生任何响应。如果我们收到任何类型的错误消息，则表示未正确安装 JDK，那就得检查这些问题。若执行不报错的话，此时可以键入：

```java
java HelloDate
```

我们将会得到正确的日期输出。这是我们编译和运行本书中每个程序（包含 `main()` 方法）的过程。此外，本书的源代码在根目录中也有一个名为 **build.gradle** 的文件，其中包含用于自动构建，测试和运行本书文件的 **Gradle** 配置。当你第一次运行 `gradlew` 命令时，**Gradle** 将自动安装（前提是已安装Java）。

## 编码风格

Java 编码规范要求类名的首字母大写。多个单词则采用驼峰法，例如：

```java
class AllTheColorsOfTheRainbow {
    // ...
}
```

对于方法、字段（成员变量）和变量名都采用驼峰法，但首字母不需要大写。示例：

```java
class AllTheColorsOfTheRainbow {
    int anIntegerRepresentingColors;
    void changeTheHueOfTheColor(int newHue) {
        // ...
    }
    // ...
}

```

## 本章小结

本章向你展示了简单的 Java 语言的基本概念。在接下来的两章，将会接触到 Java 的操作符，以及控制流程。

