[TOC]

# 第四章 运算符

## 开始使用

运算符接受一个或多个参数并生成新值。加法 `+`、减法 `-`、乘法 `*`、除法 `/` 以及赋值 `=` 在任何编程语言中的工作方式都是类似的。所有运算符都能根据运算对象生成一个值。除此以外，一些运算符可改变运算对象的值，这叫作“副作用”（**Side Effect**）。

几乎所有运算符都只能操作基本类型（Primitives）。唯一的例外是 `=`、`==` 和 `!=`，它们能操作所有对象。除此以外，**String** 类支持 `+` 和 `+=`。

## 优先级

Java 对运算顺序作出了特别的规定。其中，最简单的规则就是先乘除后加减法。程序员经常都会忘记其他优先级规则，所以应该用括号明确规定运算顺序。代码示例:

```java
// operators/Precedence.java
public class Precedence {
    
    public static void main(String[] args) {
        int x = 1, y = 2, z = 3;
        int a = x + y - 2/2 + z; // [1]
        int b = x + (y - 2)/(2 + z); // [2]
        System.out.println("a = " + a);
        System.out.println("b = " + b);
    }
}
```

输出结果:

```
    a = 5
    b = 1
```

我们注意到，在 `System.out.println()` 语句中使用了 `+` 运算符。 但是在这里 `+` 代表的意思是字符串连接符。编译器会将 `+` 连接的非字符串尝试转换为字符串。上例中的输出结果说明了 a 和 b 都已经被转化成了字符串。

## 赋值

基本类型的赋值都是直接的，而不像对象，赋予的只是其内存的引用。举个例子，a = b ，如果 b 是基本类型，那么赋值操作会将 b 的值复制一份给变量 a， 此后若 a 的值发生改变是不会影响到 b 的。

如果是为对象赋值，我们实际上操作的是它的引用。所以我们将右边的对象赋予给左边时，赋予的只是该对象的引用。此时，两者指向的堆中的对象还是同一个。代码示例：

```java 
// operators/Assignment.java
// Assignment with objects is a bit tricky
class Tank {
    int level;
}

public class Assignment {

    public static void main(String[] args) {
        Tank t1 = new Tank();
        Tank t2 = new Tank();
        t1.level = 9;
        t2.level = 47;
        System.out.println("1: t1.level: " + t1.level +
            ", t2.level: " + t2.level);
        t1 = t2;
        System.out.println("2: t1.level: " + t1.level +
            ", t2.level: " + t2.level);
        t1.level = 27;
        System.out.println("3: t1.level: " + t1.level +
            ", t2.level: " + t2.level);
    }
}
```

输出结果：

```
1: t1.level: 9, t2.level: 47
2: t1.level: 47, t2.level: 47
3: t1.level: 27, t2.level: 27
```

由于赋予的只是对象的引用，改变 t1 也就改变了 t2。（t1 原始对象的引用在 t2 赋值给其时丢失，它引用的对象会在垃圾回收时被清理）。

代码示例：

```java
t1.level = t2.level;
```

较之前的做法，这样做保留了两个单独的对象，但是有点违背 Java 的设计原则。

### 方法调用中的别名现象

当我们把对象传递给方法时，会发生别名现象。

```java
// operators/PassObject.java
// 正在传递的对象可能不是你之前使用的
class Letter {
    char c;
}

public class PassObject {
    static void f(Letter y) {
        y.c = 'z';
    }
    
    public static void main(String[] args) {
        Letter x = new Letter();
        x.c = 'a';
        System.out.println("1: x.c: " + x.c);
        f(x);
        System.out.println("2: x.c: " + x.c);
     }
}
```

输出结果：

```
1: x.c: a
2: x.c: z
```

一旦传递了一个引用，那么实际上 `y.c ='z';` 是在方法 `f()` 之外改变对象。别名现象以及其解决方案是个复杂的问题，在附录中有包含：[对象传递和返回](./Appendix-Passing-and-Returning-Objects.md)。

## 算术运算符

Java 的基本算术运算符包括加号 `+`、减号 `-`、除号 `/`、乘号 `*` 以及取模 `%`（从整数除法中获得余数）。整数除法会直接砍掉小数，而不是进位。

Java 也可用 x += 4 来表示：将 x 的值加上4的结果再赋值给 x。更多代码示例：

```java
// operators/MathOps.java
// The mathematical operators
import java.util.*;

public class MathOps {
    public static void main(String[] args) {
        // Create a seeded random number generator:
        Random rand = new Random(47);
        int i, j, k;
        // Choose value from 1 to 100:
        j = rand.nextInt(100) + 1;
        System.out.println("j : " + j);
        k = rand.nextInt(100) + 1;
        System.out.println("k : " + k);
        i = j + k;
        System.out.println("j + k : " + i);
        i = j - k;
        System.out.println("j - k : " + i);
        i = k / j;
        System.out.println("k / j : " + i);
        i = k * j;
        System.out.println("k * j : " + i);
        i = k % j;
        System.out.println("k % j : " + i);
        j %= k;
        System.out.println("j %= k : " + j);
        // 浮点运算测试
        float u, v, w; // Applies to doubles, too
        v = rand.nextFloat();
        System.out.println("v : " + v);
        w = rand.nextFloat();
        System.out.println("w : " + w);
        u = v + w;
        System.out.println("v + w : " + u);
        u = v - w;
        System.out.println("v - w : " + u);
        u = v * w;
        System.out.println("v * w : " + u);
        u = v / w;
        System.out.println("v / w : " + u);
        // 下面的操作同样适用于 char, 
        // byte, short, int, long, and double:
        u += v;
        System.out.println("u += v : " + u);
        u -= v;
        System.out.println("u -= v : " + u);
        u *= v;
        System.out.println("u *= v : " + u);
        u /= v;
        System.out.println("u /= v : " + u);    
    }
}

```

输出结果：

```
j : 59
k : 56
j + k : 115
j - k : 3
k / j : 0
k * j : 3304
k % j : 56
j %= k : 3
v : 0.5309454
w : 0.0534122
v + w : 0.5843576
v - w : 0.47753322
v * w : 0.028358962
v / w : 9.940527
u += v : 10.471473
u -= v : 9.940527
u *= v : 5.2778773
u /= v : 9.940527
```

为了生成随机数字，程序首先创建一个 **Random** 对象。该对象通过调用方法 `nextInt()` 和 `nextFloat()`（还可以调用 `nextLong()` 或 `nextDouble()`），使用 **Random** 对象生成许多不同类型的随机数。`nextInt()` 的参数设置生成的数字的上限，下限为零，为了避免零除的可能性，结果偏移1。

### 一元加减运算符

一元加 `+` 减 `-` 运算符的操作和二元是相同的。编译器可自动识别使用何种方式解析运算：

```java
x = -a;   // 正确
x = a * -b; // 正确，但是程序员看着不直观
x = a * (-b);  // 推荐
```

一元减号可以得到数据的负值。一元加号的作用相反，不过它还会把较小的数值类型自动转换为 **int** 类型。

## 递增和递减

举个例子来说，假设 a 是一个 **int** 类型的值，则表达式 `++a` 就等价于 `a = a + 1`。递增和递减运算符不仅可以修改变量，还可以生成变量的值。

每种类型的运算符，都有两个版本；对于前递增和前递减（如 `++a` 或 `--a`），会先执行递增/减运算，再返回值。而对于后递增和后递减（如 `a++` 或 `a--`），会先返回值，再执行递增/减运算。代码示例：

```java
// operators/AutoInc.java
// 演示 ++ 和 -- 运算符
public class AutoInc {
    public static void main(String[] args) {
        int i = 1;
        System.out.println("i: " + i);
        System.out.println("++i: " + ++i); // 前递增
        System.out.println("i++: " + i++); // 后递增
        System.out.println("i: " + i);
        System.out.println("--i: " + --i); // 前递减
        System.out.println("i--: " + i--); // 后递减
        System.out.println("i: " + i);
    }
}
```

输出结果：

```
i: 1
++i: 2
i++: 2
i: 3
--i: 2
i--: 2
i: 1
```

对于前缀形式，我们将在执行递增/减操作后获得值；使用后缀形式，我们将在执行递增/减操作之前获得值。它们是唯一“副作用”的运算符 —— 修改了操作数的值。

## 关系运算符

关系运算符会通过产生一个布尔（**boolean**）结果来表示操作数之间的关系。如果关系为真，则结果为 **true**，如果关系为假，则结果为 **false**。关系运算符包括小于 `<`，大于 `>`，小于或等于 `<=`，大于或等于 `>=`，等于 `==` 和不等于 `！=`。

### 测试对象等价

关系运算符 `==` 和 `!=` 适用于所有对象之间的比较运算。代码示例：

```java
// operators/Equivalence.java
public class Equivalence1 {
    public static void main(String[] args) {
        Integer n1 = 47;
        Integer n2 = 47;
        System.out.println(n1 == n2);  // true
        System.out.println(n1 != n2);  // false
    }
}

public class Equivalence2 {
    public static void main(String[] args) {
        Integer n1 = 147;
        Integer n2 = 147;
        System.out.println(n1 == n2);  // false
        System.out.println(n1 != n2);  // true
    }
}

public class Equivalence3 {
    public static void main(String[] args) {
        int n1 = 147;
        int n2 = 147;
        System.out.println(n1 == n2);  // true
        System.out.println(n1 != n2);  // false
    }
}
```

因为 Integer 是引用类型，所以 Equivalence1 比较的是对象引用，输出实际上应该是先输出 **false**，再输出 **true**，但是因为 Integer 内部维护着一个 IntegerCache 的缓存，默认缓存范围是 [-128, 127]，所以 [-128, 127] 之间的值用 `==` 和 `!=` 比较也能能到正确的结果。但是不推荐用关系运算符比较，具体见 JDK 中的 Integer 类源码。

那么怎么比较两个对象的内容是否相同呢？你必须使用所有对象（不包括基本类型）中都存在的 `equals()` 方法，下面是如何使用 `equals()` 方法的示例：

```java
// operators/EqualsMethod.java
public class EqualsMethod {
    public static void main(String[] args) {
        Integer n1 = 47;
        Integer n2 = 47;
        System.out.println(n1.equals(n2));  // true
    }
}
```

上例的结果看起来是我们所期望的。但其实事情并非那么简单。下面我们来创建自己的类：

```java
// operators/EqualsMethod2.java
// 默认的 equals() 方法没有比较内容
class Value {
    int i;
}

public class EqualsMethod2 {
    public static void main(String[] args) {
        Value v1 = new Value();
        Value v2 = new Value();
        v1.i = v2.i = 100;
        System.out.println(v1.equals(v2));  // false
    }
}
```

上例的结果再次令人困惑：结果是 **false**。原因： `equals()` 的默认行为是比较对象的引用而非具体内容。因此，除非你在新类中覆写 `equals()` 方法，否则我们将获取不到想要的结果。在学习 [复用](./08-Reuse.md)（**Reuse**） 章节后我们才能接触到“覆写”（**Override**），并且直到 [附录:集合主题](./Appendix-Collection-Topics.md)，才能知道定义 `equals()` 方法的正确方式，但是现在明白 `equals()` 行为方式也可能为你节省一些时间。

大多数 Java 库类通过覆写 `equals()` 方法比较对象的内容而不是其引用。

## 逻辑运算符

每个逻辑运算符 `&&` （**AND**）、`||`（**OR**）和 `!`（**非**）根据参数的逻辑关系生成布尔值 `true` 或 `false`。示例：

```java
// operators/Bool.java
// 关系运算符和逻辑运算符
import java.util.*;
public class Bool {
    public static void main(String[] args) {
        Random rand = new Random(47);
        int i = rand.nextInt(100);
        int j = rand.nextInt(100);
        System.out.println("i = " + i);
        System.out.println("j = " + j);
        System.out.println("i > j is " + (i > j));
        System.out.println("i < j is " + (i < j));
        System.out.println("i >= j is " + (i >= j));
        System.out.println("i <= j is " + (i <= j));
        System.out.println("i == j is " + (i == j));
        System.out.println("i != j is " + (i != j));
        // 将 int 作为布尔处理不是合法的 Java 写法
        //- System.out.println("i && j is " + (i && j));
        //- System.out.println("i || j is " + (i || j));
        //- System.out.println("!i is " + !i);
        System.out.println("(i < 10) && (j < 10) is " + ((i < 10) && (j < 10)) );
        System.out.println("(i < 10) || (j < 10) is " + ((i < 10) || (j < 10)) );
    }
}
```

输出结果：

```
i = 58
j = 55
i > j is true
i < j is false
i >= j is true
i <= j is false
i == j is false
i != j is true
(i < 10) && (j < 10) is false
(i < 10) || (j < 10) is false
```

上面的例子中，我们将使用非布尔值的表达式注释掉了（你可以看到表达式前面是 //-）。但是，后续的表达式使用关系比较生成布尔值，然后对结果使用了逻辑运算。请注意，如果在预期为 **String** 类型的位置使用 **boolean** 类型的值，则结果会自动转为适当的格式（即 "true" 或 "false" 字符串）。

### 短路

逻辑运算符支持一种称为“短路”（short-circuiting）的现象。整个表达式会在运算到可以明确结果时就停止并返回结果，这意味着该逻辑表达式的后半部分不会被执行到。代码示例：

```java
// operators / ShortCircuit.java 
// 逻辑运算符的短路行为
public class ShortCircuit {

    static boolean test1(int val) {
        System.out.println("test1(" + val + ")");
        System.out.println("result: " + (val < 1));
        return val < 1;
    }

    static boolean test2(int val) {
        System.out.println("test2(" + val + ")");
        System.out.println("result: " + (val < 2));
        return val < 2;
    }

    static boolean test3(int val) {
        System.out.println("test3(" + val + ")");
        System.out.println("result: " + (val < 3));
        return val < 3;
    }

    public static void main(String[] args) {
        boolean b = test1(0) && test2(2) && test3(2);
        System.out.println("expression is " + b);
    }
}
```

输出结果：

```
test1(0)
result: true
test2(2)
result: false
expression is false
```

每个测试都对参数执行比较并返回 `true` 或 `false`。同时控制台也会在方法执行时打印他们的执行状态。 下面的表达式：

```java
test1（0）&& test2（2）&& test3（2）
```

可能你的预期是程序会执行 3 个 **test** 方法并返回。我们来分析一下：第一个方法的结果返回 `true`，因此表达式会继续走下去。紧接着，第二个方法的返回结果是 `false`。这就代表这整个表达式的结果肯定为 `false`，所以就没有必要再判断剩下的表达式部分了。

所以，运用“短路”可以节省部分不必要的运算，从而提高程序潜在的性能。

## 字面值常量

通常，当我们向程序中插入一个字面值常量（**Literal**）时，编译器会确切地识别它的类型。当类型不明确时，必须辅以字面值常量关联来帮助编译器识别。代码示例：

```java
// operators/Literals.java
public class Literals {
    public static void main(String[] args) {
        int i1 = 0x2f; // 16进制 (小写)
        System.out.println( "i1: " + Integer.toBinaryString(i1));
        int i2 = 0X2F; // 16进制 (大写)
        System.out.println( "i2: " + Integer.toBinaryString(i2)); 
        int i3 = 0177; // 8进制 (前导0)
        System.out.println( "i3: " + Integer.toBinaryString(i3));
        char c = 0xffff; // 最大 char 型16进制值
        System.out.println( "c: " + Integer.toBinaryString(c));
        byte b = 0x7f; // 最大 byte 型16进制值  10101111;
        System.out.println( "b: " + Integer.toBinaryString(b));
        short s = 0x7fff; // 最大 short 型16进制值
        System.out.println( "s: " + Integer.toBinaryString(s));
        long n1 = 200L; // long 型后缀
        long n2 = 200l; // long 型后缀 (容易与数值1混淆)
        long n3 = 200;
    
        // Java 7 二进制字面值常量:
        byte blb = (byte)0b00110101;
        System.out.println( "blb: " + Integer.toBinaryString(blb));
        short bls = (short)0B0010111110101111;
        System.out.println( "bls: " + Integer.toBinaryString(bls));
        int bli = 0b00101111101011111010111110101111;
        System.out.println( "bli: " + Integer.toBinaryString(bli));
        long bll = 0b00101111101011111010111110101111;
        System.out.println( "bll: " + Long.toBinaryString(bll));
        float f1 = 1;
        float f2 = 1F; // float 型后缀
        float f3 = 1f; // float 型后缀
        double d1 = 1d; // double 型后缀
        double d2 = 1D; // double 型后缀
        // (long 型的字面值同样适用于十六进制和8进制 )
    }
}
```

输出结果:

```
i1: 101111
i2: 101111
i3: 1111111
c: 1111111111111111
b: 1111111
s: 111111111111111
blb: 110101
bls: 10111110101111
bli: 101111101011111010111110101111
bll: 101111101011111010111110101111
```

在文本值的后面添加字符可以让编译器识别该文本值的类型。对于 **Long** 型数值，结尾使用大写 `L` 或小写 `l` 皆可（不推荐使用 `l`，因为容易与阿拉伯数值 1 混淆）。大写 `F` 或小写 `f` 表示 **float** 浮点数。大写 `D` 或小写 `d` 表示 **double** 双精度。

十六进制（以 16 为基数），适用于所有整型数据类型，由前导 `0x` 或 `0X` 表示，后跟 0-9 或 a-f （大写或小写）。如果我们在初始化某个类型的数值时，赋值超出其范围，那么编译器会报错（不管值的数字形式如何）。在上例的代码中，**char**、**byte** 和 **short** 的值已经是最大了。如果超过这些值，编译器将自动转型为 **int**，并且提示我们需要声明强制转换（强制转换将在本章后面定义），意味着我们已越过该类型的范围界限。

八进制（以 8 为基数）由 0~7 之间的数字和前导零 `0` 表示。

Java 7 引入了二进制的字面值常量，由前导 `0b` 或 `0B` 表示，它可以初始化所有的整数类型。

使用整型数值类型时，显示其二进制形式会很有用。在 Long 型和 Integer 型中这很容易实现，调用其静态的 `toBinaryString()` 方法即可。 但是请注意，若将较小的类型传递给 **Integer.**`tobinarystring()` 时，类型将自动转换为 **int**。

### 下划线

Java 7 中有一个深思熟虑的补充：可以在数字字面量中包含下划线 `_`，以使显示更清晰。这对于大数值的分组特别有用。代码示例：

```java
// operators/Underscores.java
public class Underscores {
    public static void main(String[] args) {
        double d = 341_435_936.445_667;
        System.out.println(d);
        int bin = 0b0010_1111_1010_1111_1010_1111_1010_1111;
        System.out.println(Integer.toBinaryString(bin));
        System.out.printf("%x%n", bin); // [1]
        long hex = 0x7f_e9_b7_aa;
        System.out.printf("%x%n", hex);
    }
}
```

输出结果:

```
3.41435936445667E8
101111101011111010111110101111
2fafafaf
7fe9b7aa
```

下面是合理使用的规则：

1. 仅限单 `_`，不能多条相连。
2. 数值开头和结尾不允许出现 `_`。
3. `F`、`D` 和 `L`的前后禁止出现 `_`。
4. 二进制前导 `b` 和 十六进制 `x` 前后禁止出现 `_`。

### 指数计数法

指数总是采用一种我认为很不直观的记号方法:

```java
// operators/Exponents.java
// "e" 表示 10 的几次幂
public class Exponents {
    public static void main(String[] args) {
        // 大写 E 和小写 e 的效果相同:
        float expFloat = 1.39e-43f;
        expFloat = 1.39E-43f;
        System.out.println(expFloat);
        double expDouble = 47e47d; // 'd' 是可选的
        double expDouble2 = 47e47; // 自动转换为 double
        System.out.println(expDouble);
    }
}
```

输出结果:

```
1.39E-43
4.7E48
```

在科学与工程学领域，**e** 代表自然对数的基数，约等于 2.718 （Java 里用一种更精确的 **double** 值 **Math.E** 来表示自然对数）。指数表达式 "1.39 x e-43"，意味着 “1.39 × 2.718 的 -43 次方”。然而，自 FORTRAN 语言发明后，人们自然而然地觉得e 代表 “10 的几次幂”。

注意如果编译器能够正确地识别类型，就不必使用后缀字符。对于下述语句：

```java
long n3 = 200;
```

它并不存在含糊不清的地方，所以 200 后面的 L 大可省去。然而，对于下述语句：

```java
float f4 = 1e-43f; //10 的幂数
```

编译器通常会将指数作为 **double** 类型来处理，所以假若没有这个后缀字符 `f`，编译器就会报错，提示我们应该将 **double** 型转换成 **float** 型。

## 位运算符

位运算符允许我们操作一个整型数字中的单个二进制位。位运算符会对两个整数对应的位执行布尔代数，从而产生结果。

位运算源自 C 语言的底层操作。我们经常要直接操纵硬件，频繁设置硬件寄存器内的二进制位。Java 的设计初衷是电视机顶盒嵌入式开发，所以这种底层的操作仍被保留了下来。但是，你可能不会使用太多位运算。

若两个输入位都是 1，则按位“与运算符” `&` 运算后结果是 1，否则结果是 0。若两个输入位里至少有一个是 1，则按位“或运算符” `|` 运算后结果是 1；只有在两个输入位都是 0 的情况下，运算结果才是 0。若两个输入位的某一个是 1，另一个不是 1，那么按位“异或运算符” `^` 运算后结果才是 1。按位“非运算符” `~` 属于一元运算符；它只对一个自变量进行操作（其他所有运算符都是二元运算符）。按位非运算后结果与输入位相反。例如输入 0，则输出 1；输入 1，则输出 0。

位运算符和逻辑运算符都使用了同样的字符，只不过数量不同。位短，所以位运算符只有一个字符。位运算符可与等号 `=` 联合使用以接收结果及赋值：`&=`，`|=` 和 `^=` 都是合法的（由于 `~` 是一元运算符，所以不可与 `=` 联合使用）。

我们将 **Boolean** 类型被视为“单位值”（one-bit value），所以它多少有些独特的地方。我们可以对 boolean 型变量执行与、或、异或运算，但不能执行非运算（大概是为了避免与逻辑“非”混淆）。对于布尔值，位运算符具有与逻辑运算符相同的效果，只是它们不会中途“短路”。此外，针对布尔值进行的位运算为我们新增了一个“异或”逻辑运算符，它并未包括在逻辑运算符的列表中。在移位表达式中，禁止使用布尔值，原因将在下面解释。

## 移位运算符

移位运算符面向的运算对象也是二进制的“位”。它们只能用于处理整数类型（基本类型的一种）。左移位运算符 `<<` 能将其左边的运算对象向左移动右侧指定的位数（在低位补 0）。右移位运算符 `>>` 则相反。右移位运算符有“正”、“负”值：若值为正，则在高位插入 0；若值为负，则在高位插入 1。Java 也添加了一种“不分正负”的右移位运算符（>>>），它使用了“零扩展”（zero extension）：无论正负，都在高位插入 0。这一运算符是 C/C++ 没有的。

如果移动 **char**、**byte** 或 **short**，则会在移动发生之前将其提升为 **int**，结果为 **int**。仅使用右值（rvalue）的 5 个低阶位。这可以防止我们移动超过 **int** 范围的位数。若对一个 **long** 值进行处理，最后得到的结果也是 **long**。

移位可以与等号 `<<=` 或 `>>=` 或 `>>>=` 组合使用。左值被替换为其移位运算后的值。但是，问题来了，当无符号右移与赋值相结合时，若将其与 **byte** 或 **short** 一起使用的话，则结果错误。取而代之的是，它们被提升为 **int** 型并右移，但在重新赋值时被截断。在这种情况下，结果为 -1。下面是代码示例：

```java
// operators/URShift.java
// 测试无符号右移
public class URShift {
    public static void main(String[] args) {
        int i = -1;
        System.out.println(Integer.toBinaryString(i));
        i >>>= 10;
        System.out.println(Integer.toBinaryString(i));
        long l = -1;
        System.out.println(Long.toBinaryString(l));
        l >>>= 10;
        System.out.println(Long.toBinaryString(l));
        short s = -1;
        System.out.println(Integer.toBinaryString(s));
        s >>>= 10;
        System.out.println(Integer.toBinaryString(s));
        byte b = -1;
        System.out.println(Integer.toBinaryString(b));
        b >>>= 10;
        System.out.println(Integer.toBinaryString(b));
        b = -1;
        System.out.println(Integer.toBinaryString(b));
        System.out.println(Integer.toBinaryString(b>>>10));
    }
}
```

输出结果：

```
11111111111111111111111111111111
1111111111111111111111
1111111111111111111111111111111111111111111111111111111111111111
111111111111111111111111111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
11111111111111111111111111111111
1111111111111111111111
```


在上例中，结果并未重新赋值给变量 **b** ，而是直接打印出来，因此一切正常。下面是一个涉及所有位运算符的代码示例：

```java
// operators/BitManipulation.java
// 使用位运算符
import java.util.*;
public class BitManipulation {
    public static void main(String[] args) {
        Random rand = new Random(47);
        int i = rand.nextInt();
        int j = rand.nextInt();
        printBinaryInt("-1", -1);
        printBinaryInt("+1", +1);
        int maxpos = 2147483647;
        printBinaryInt("maxpos", maxpos);
        int maxneg = -2147483648;
        printBinaryInt("maxneg", maxneg);
        printBinaryInt("i", i);
        printBinaryInt("~i", ~i);
        printBinaryInt("-i", -i);
        printBinaryInt("j", j);
        printBinaryInt("i & j", i & j);
        printBinaryInt("i | j", i | j);
        printBinaryInt("i ^ j", i ^ j);
        printBinaryInt("i << 5", i << 5);
        printBinaryInt("i >> 5", i >> 5);
        printBinaryInt("(~i) >> 5", (~i) >> 5);
        printBinaryInt("i >>> 5", i >>> 5);
        printBinaryInt("(~i) >>> 5", (~i) >>> 5);
        long l = rand.nextLong();
        long m = rand.nextLong();
        printBinaryLong("-1L", -1L);
        printBinaryLong("+1L", +1L);
        long ll = 9223372036854775807L;
        printBinaryLong("maxpos", ll);
        long lln = -9223372036854775808L;
        printBinaryLong("maxneg", lln);
        printBinaryLong("l", l);
        printBinaryLong("~l", ~l);
        printBinaryLong("-l", -l);
        printBinaryLong("m", m);
        printBinaryLong("l & m", l & m);
        printBinaryLong("l | m", l | m);
        printBinaryLong("l ^ m", l ^ m);
        printBinaryLong("l << 5", l << 5);
        printBinaryLong("l >> 5", l >> 5);
        printBinaryLong("(~l) >> 5", (~l) >> 5);
        printBinaryLong("l >>> 5", l >>> 5);
        printBinaryLong("(~l) >>> 5", (~l) >>> 5);
    }

    static void printBinaryInt(String s, int i) {
        System.out.println(
        s + ", int: " + i + ", binary:\n " +
        Integer.toBinaryString(i));
    }

    static void printBinaryLong(String s, long l) {
        System.out.println(
        s + ", long: " + l + ", binary:\n " +
        Long.toBinaryString(l));
    }
}
```

输出结果（前 32 行）：

```
-1, int: -1, binary:
11111111111111111111111111111111
+1, int: 1, binary:
1
maxpos, int: 2147483647, binary:
1111111111111111111111111111111
maxneg, int: -2147483648, binary:
10000000000000000000000000000000
i, int: -1172028779, binary:
10111010001001000100001010010101
~i, int: 1172028778, binary:
 1000101110110111011110101101010
-i, int: 1172028779, binary:
1000101110110111011110101101011
j, int: 1717241110, binary:
1100110010110110000010100010110
i & j, int: 570425364, binary:
100010000000000000000000010100
i | j, int: -25213033, binary:
11111110011111110100011110010111
i ^ j, int: -595638397, binary:
11011100011111110100011110000011
i << 5, int: 1149784736, binary:
1000100100010000101001010100000
i >> 5, int: -36625900, binary:
11111101110100010010001000010100
(~i) >> 5, int: 36625899, binary:
10001011101101110111101011
i >>> 5, int: 97591828, binary:
101110100010010001000010100
(~i) >>> 5, int: 36625899, binary:
10001011101101110111101011
    ...
```

结尾的两个方法 `printBinaryInt()` 和 `printBinaryLong()` 分别操作一个 **int** 和 **long** 值，并转换为二进制格式输出，同时附有简要的文字说明。除了演示 **int** 和 **long** 的所有位运算符的效果之外，本示例还显示 **int** 和 **long** 的最小值、最大值、+1 和 -1 值，以便我们了解它们的形式。注意高位代表符号：0 表示正，1 表示负。上面显示了 **int** 部分的输出。以上数字的二进制表示形式是带符号的补码（2's complement）。

## 三元运算符

三元运算符，也称为条件运算符。下面是它的表达式格式：

**布尔表达式 ? 值 1 : 值 2**

与 **if-else** 不同的是，三元运算符是有返回结果的。请看下面的代码示例：

```java
// operators/TernaryIfElse.java
public class TernaryIfElse {
    
static int ternary(int i) {
    return i < 10 ? i * 100 : i * 10;
}

static int standardIfElse(int i) {
    if(i < 10)
        return i * 100;
    else
        return i * 10;
}

    public static void main(String[] args) {
        System.out.println(ternary(9));      // 900
        System.out.println(ternary(10));     // 100
        System.out.println(standardIfElse(9)); // 900
        System.out.println(standardIfElse(10)); // 100
    }
}
```

可以看出，`ternary()` 中的代码更简短。然而，**standardIfElse()** 中的代码更易理解。所以我们在使用三元运算符时，要权衡一下。

## 字符串运算符

这个运算符在 Java 里有一项特殊用途：连接字符串。

我们注意到运用 `String +` 时有一些有趣的现象。若表达式以一个 **String** 类型开头（编译器会自动将双引号 `""` 标注的的字符序列转换为字符串），那么后续所有运算对象都必须是字符串。代码示例：

```java
// operators/StringOperators.java
public class StringOperators {
    public static void main(String[] args) {
        int x = 0, y = 1, z = 2;
        String s = "x, y, z ";
        System.out.println(s + x + y + z);
        // 将 x 转换为字符串
        System.out.println(x + " " + s);
        s += "(summed) = "; 
        // 级联操作
        System.out.println(s + (x + y + z));
        // Integer.toString()方法的简写:
        System.out.println("" + x);
    }
}
```

输出结果：

```
x, y, z 012
0 x, y, z
x, y, z (summed) = 3
0
```

上例中第 1 输出语句的执行结果是 `012` 而并非 `3`，这是因为编译器将其分别转换为其字符串形式然后与字符串变量 **s** 连接。在第 2 条输出语句中，编译器将开头的变量转换为了字符串，由此可以看出，这种转换与数据的位置无关，只要当中有一条数据是字符串类型，其他非字符串数据都将被转换为字符串形式并连接。最后一条输出语句，我们可以看出 `+=` 运算符可以拼接其右侧的字符串连接结果并重赋值给自身变量 `s`。括号 `()` 可以控制表达式的计算顺序，以便在显示 **int** 之前对其进行实际求和。

请注意主方法中的最后一个例子：我们经常会看到一个空字符串 `"" + x` 。这样可以隐式地将其转换为字符串，以代替 **toString()**。

## 常见陷阱

```java
while(x = y) {
// ...
}
```

显然，程序员原意是测试等价性 `==`，而非赋值 `=`。在C++中，变量 **y** 非 0 的话，赋值操作总会返回 `true`，于是会无限循环。而在 Java 中，编译器会试图把这个 **int** 型数据转换为预期的布尔类型(int不能转boolean)。所以，在编译期抛出错误。

## 类型转换

“类型转换”（Casting）的作用是“与一个模型匹配”。在适当的时候，Java 会将一种数据类型自动转换成另一种。例如，假设我们为 **float** 变量赋值一个整数值，计算机会将 **int** 自动转换成 **float**。我们可以在程序未自动转换时显式、强制地使此类型发生转换。

要执行强制转换，需要将所需的数据类型放在任何值左侧的括号内，如下所示：

```java
// operators/Casting.java
public class Casting {
    public static void main(String[] args) {
        int i = 200;
        long lng = (long)i;
        lng = i; // 没有必要的类型提升
        long lng2 = (long)200;
        lng2 = 200;
        // 类型收缩
        i = (int)lng2; // Cast required
    }
}
```

你可以这样地去转换一个数值类型的变量。但是上例这种做法是多余的：因为编译器会在必要时自动提升 **int** 型数据为 **long** 型。

当然，为了程序逻辑清晰或提醒自己留意，我们也可以显式地类型转换。在其他情况下，类型转换型只有在代码编译时才显出其重要性。在 Java 里，类型转换则是一种比较安全的操作。但是，若将数据类型进行“向下转换”的操作（将容量较大的数据类型转换成容量较小的类型），可能会发生信息丢失的危险。此时，编译器会提示进行转型。 对于“向上转换”（**Widening conversion**），则不必进行显式的类型转换，因为较大类型的数据肯定能容纳较小类型的数据，不会造成任何信息的丢失。

除了布尔类型的数据，Java 允许任何基本类型的数据转换为另一种基本类型的数据。此外，类是不能进行类型转换的。为了将一个类转换为另一个类型，需要使用特殊的方法（后面将会学习到如何在父子类之间进行向上/向下转型，例如，“橡树”可以转换为“树”，反之亦然。而对于“岩石”是无法转换为“树”的）。

注：只有相同类型能进行转换，如 int、long、继承的类。不同类型转换会报错，如 int 转为 boolean。

### 截断和舍入

在执行“向下转换”时，必须注意数据的截断和舍入问题。若从浮点值转换为整型值，Java 会做什么呢？例如：浮点数 29.7 被转换为整型值，结果会是 29 还是 30 呢？下面是代码示例：

```java
// operators/CastingNumbers.java
// 尝试转换 float 和 double 型数据为整型数据
public class CastingNumbers {
    public static void main(String[] args) {
        double above = 0.7, below = 0.4;
        float fabove = 0.7f, fbelow = 0.4f;
        System.out.println("(int)above: " + (int)above);  // (int)above: 0
        System.out.println("(int)below: " + (int)below);  // (int)below: 0
        System.out.println("(int)fabove: " + (int)fabove);  // (int)fabove: 0
        System.out.println("(int)fbelow: " + (int)fbelow); // (int)fbelow: 0
    }
}
```

答案是，从 **float** 和 **double** 转换为整数值时，小数位将被截断。若你想对结果进行四舍五入，可以使用 `java.lang.Math` 的 ` round()` 方法：

```java
// operators/RoundingNumbers.java
// float 和 double 类型数据的四舍五入
public class RoundingNumbers {
    public static void main(String[] args) {
        double above = 0.7, below = 0.4;
        float fabove = 0.7f, fbelow = 0.4f;
        System.out.println( "Math.round(above): " + Math.round(above));   // 1
        System.out.println( "Math.round(below): " + Math.round(below));   // 0
        System.out.println( "Math.round(fabove): " + Math.round(fabove));  // 1
        System.out.println( "Math.round(fbelow): " + Math.round(fbelow));  // 0
    }
}
```

因为 `round()` 方法是 `java.lang` 的一部分，所以我们无需通过 `import` 就可以使用。

### 类型提升

你会发现，如果我们对小于 **int** 的基本数据类型（即 **char**、**byte** 或 **short**）执行任何算术或按位操作，这些值会在执行操作之前类型提升为 **int**，并且结果值的类型为 **int**。若想重新使用较小的类型，必须使用强制转换（由于重新分配回一个较小的类型，结果可能会丢失精度）。通常，表达式中最大的数据类型是决定表达式结果的数据类型。**float** 型和 **double** 型相乘，结果是 **double** 型的；**int** 和 **long** 相加，结果是 **long** 型。

## Java没有sizeof

在 C/C++ 中，经常需要用到 `sizeof()` 方法来获取数据项被分配的字节大小。例如，一台计算机可用 32 位来保存整数，而另一台只用 16 位保存。显然，在第一台机器中，程序可保存更大的值。所以，移植是令 C/C++ 程序员颇为头痛的一个问题。

Java 不需要 ` sizeof()` 方法来满足这种需求，因为所有类型的大小在不同平台上是相同的。我们不必考虑这个层次的移植问题 —— Java 本身就是一种“与平台无关”的语言。

## 运算符总结

下面的代码示例是对上述所有示例的重复，只不过概括了所有的基本类型。这个文件能被正确地编译，因为我已经把编译不通过的那部分用注释 `//` 过滤了。代码示例：

```java
// operators/AllOps.java
// 测试所有基本类型的运算符操作
// 看看哪些是能被 Java 编译器接受的
public class AllOps {
    // 布尔值的接收测试：
    void f(boolean b) {}
    void boolTest(boolean x, boolean y) {
        // 算数运算符：
        //- x = x * y;
        //- x = x / y;
        //- x = x % y;
        //- x = x + y;
        //- x = x - y;
        //- x++;
        //- x--;
        //- x = +y;
        //- x = -y;
        // 关系运算符和逻辑运算符：
        //- f(x > y);
        //- f(x >= y);
        //- f(x < y);
        //- f(x <= y);
        f(x == y);
        f(x != y);
        f(!y);
        x = x && y;
        x = x || y;
        // 按位运算符：
        //- x = ~y;
        x = x & y;
        x = x | y;
        x = x ^ y;
        //- x = x << 1;
        //- x = x >> 1;
        //- x = x >>> 1;
        // 联合赋值：
        //- x += y;
        //- x -= y;
        //- x *= y;
        //- x /= y;
        //- x %= y;
        //- x <<= 1;
        //- x >>= 1;
        //- x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换：
        //- char c = (char)x;
        //- byte b = (byte)x;
        //- short s = (short)x;
        //- int i = (int)x;
        //- long l = (long)x;
        //- float f = (float)x;
        //- double d = (double)x;
    }

    void charTest(char x, char y) {
        // 算数运算符：
        x = (char)(x * y);
        x = (char)(x / y);
        x = (char)(x % y);
        x = (char)(x + y);
        x = (char)(x - y);
        x++;
        x--;
        x = (char) + y;
        x = (char) - y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        x= (char)~y;
        x = (char)(x & y);
        x = (char)(x | y);
        x = (char)(x ^ y);
        x = (char)(x << 1);
        x = (char)(x >> 1);
        x = (char)(x >>> 1);
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        x <<= 1;
        x >>= 1;
        x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换
        //- boolean bl = (boolean)x;
        byte b = (byte)x;
        short s = (short)x;
        int i = (int)x;
        long l = (long)x;
        float f = (float)x;
        double d = (double)x;
    }

    void byteTest(byte x, byte y) {
        // 算数运算符：
        x = (byte)(x* y);
        x = (byte)(x / y);
        x = (byte)(x % y);
        x = (byte)(x + y);
        x = (byte)(x - y);
        x++;
        x--;
        x = (byte) + y;
        x = (byte) - y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        //按位运算符：
        x = (byte)~y;
        x = (byte)(x & y);
        x = (byte)(x | y);
        x = (byte)(x ^ y);
        x = (byte)(x << 1);
        x = (byte)(x >> 1);
        x = (byte)(x >>> 1);
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        x <<= 1;
        x >>= 1;
        x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        short s = (short)x;
        int i = (int)x;
        long l = (long)x;
        float f = (float)x;
        double d = (double)x;
    }

    void shortTest(short x, short y) {
        // 算术运算符：
        x = (short)(x * y);
        x = (short)(x / y);
        x = (short)(x % y);
        x = (short)(x + y);
        x = (short)(x - y);
        x++;
        x--;
        x = (short) + y;
        x = (short) - y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        x = (short) ~ y;
        x = (short)(x & y);
        x = (short)(x | y);
        x = (short)(x ^ y);
        x = (short)(x << 1);
        x = (short)(x >> 1);
        x = (short)(x >>> 1);
        // Compound assignment:
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        x <<= 1;
        x >>= 1;
        x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        byte b = (byte)x;
        int i = (int)x;
        long l = (long)x;
        float f = (float)x;
        double d = (double)x;
    }

    void intTest(int x, int y) {
        // 算术运算符：
        x = x * y;
        x = x / y;
        x = x % y;
        x = x + y;
        x = x - y;
        x++;
        x--;
        x = +y;
        x = -y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        x = ~y;
        x = x & y;
        x = x | y;
        x = x ^ y;
        x = x << 1;
        x = x >> 1;
        x = x >>> 1;
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        x <<= 1;
        x >>= 1;
        x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        byte b = (byte)x;
        short s = (short)x;
        long l = (long)x;
        float f = (float)x;
        double d = (double)x;
    }

    void longTest(long x, long y) {
        // 算数运算符：
        x = x * y;
        x = x / y;
        x = x % y;
        x = x + y;
        x = x - y;
        x++;
        x--;
        x = +y;
        x = -y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        x = ~y;
        x = x & y;
        x = x | y;
        x = x ^ y;
        x = x << 1;
        x = x >> 1;
        x = x >>> 1;
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        x <<= 1;
        x >>= 1;
        x >>>= 1;
        x &= y;
        x ^= y;
        x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        byte b = (byte)x;
        short s = (short)x;
        int i = (int)x;
        float f = (float)x;
        double d = (double)x;
    }

    void floatTest(float x, float y) {
        // 算数运算符：
        x = x * y;
        x = x / y;
        x = x % y;
        x = x + y;
        x = x - y;
        x++;
        x--;
        x = +y;
        x = -y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        //- x = ~y;
        //- x = x & y;
        //- x = x | y;
        //- x = x ^ y;
        //- x = x << 1;
        //- x = x >> 1;
        //- x = x >>> 1;
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        //- x <<= 1;
        //- x >>= 1;
        //- x >>>= 1;
        //- x &= y;
        //- x ^= y;
        //- x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        byte b = (byte)x;
        short s = (short)x;
        int i = (int)x;
        long l = (long)x;
        double d = (double)x;
    }

    void doubleTest(double x, double y) {
        // 算术运算符：
        x = x * y;
        x = x / y;
        x = x % y;
        x = x + y;
        x = x - y;
        x++;
        x--;
        x = +y;
        x = -y;
        // 关系和逻辑运算符：
        f(x > y);
        f(x >= y);
        f(x < y);
        f(x <= y);
        f(x == y);
        f(x != y);
        //- f(!x);
        //- f(x && y);
        //- f(x || y);
        // 按位运算符：
        //- x = ~y;
        //- x = x & y;
        //- x = x | y;
        //- x = x ^ y;
        //- x = x << 1;
        //- x = x >> 1;
        //- x = x >>> 1;
        // 联合赋值：
        x += y;
        x -= y;
        x *= y;
        x /= y;
        x %= y;
        //- x <<= 1;
        //- x >>= 1;
        //- x >>>= 1;
        //- x &= y;
        //- x ^= y;
        //- x |= y;
        // 类型转换：
        //- boolean bl = (boolean)x;
        char c = (char)x;
        byte b = (byte)x;
        short s = (short)x;
        int i = (int)x;
        long l = (long)x;
        float f = (float)x;
    }
}
```

**注意** ：**boolean** 类型的运算是受限的。你能为其赋值 `true` 或 `false`，也可测试它的值是否是 `true` 或 `false`。但你不能对其作加减等其他运算。

在 **char**，**byte** 和 **short** 类型中，我们可以看到算术运算符的“类型转换”效果。我们必须要显式强制类型转换才能将结果重新赋值为原始类型。对于 **int** 类型的运算则不用转换，因为默认就是 **int** 型。虽然我们不用再停下来思考这一切是否安全，但是两个大的 int 型整数相乘时，结果有可能超出 **int** 型的范围，这种情况下结果会发生溢出。下面的代码示例：

```java
// operators/Overflow.java
// 厉害了！内存溢出
public class Overflow {
    public static void main(String[] args) {
        int big = Integer.MAX_VALUE;
        System.out.println("big = " + big);
        int bigger = big * 4;
        System.out.println("bigger = " + bigger);
    }
}
```

输出结果：

```text
big = 2147483647
bigger = -4
```

编译器没有报错或警告，运行时一切看起来都无异常。

对于 **char**，**byte** 或者 **short**，混合赋值并不需要类型转换。即使转型，也会获得与相同的结果。总之，除 **boolean** 以外，其他任何两种基本类型间都可进行类型转换。当进行向下转换类型时，需要注意结果范围是否溢出，否则会丢失精度。

## 本章小结

如果你已接触过一门 C 语法风格编程语言，那么你在学习 Java 的运算符时实际上没有任何曲线。如果你觉得有难度，那么我推荐你要先去 www.OnJava8.com 观看 《Thinking in C》 的视频教程来补充一些前置知识储备。

