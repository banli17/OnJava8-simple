[TOC]

# 第五章 控制流

在 Java 中，涉及的关键字包括 **if-else，while，do-while，for，return，break** 和选择语句 **switch**。 Java 并不支持备受诟病的 **goto**。

## true和false

所有的条件语句都利用条件表达式的“真”或“假”来决定执行路径。代码示例：

```java
// control/TrueFalse.java
public class TrueFalse {
	public static void main(String[] args) {
		System.out.println(1 == 1);  // true
		System.out.println(1 == 2);  // false
	}
}
```

**注意**：在 Java 中使用数值作为布尔值是非法的，即便这种操作在 C/C++ 中是被允许的（在这些语言中，“真”为非零，而“假”是零）。

## if-else

**if-else** 语句是控制程序执行流程最基本的形式。代码示例：

```java
// control/IfElse.java
public class IfElse {
  static int result = 0;
  static void test(int testval, int target) {
    if(testval > target)
      result = +1;
    else if(testval < target) // [1]
      result = -1;
    else
      result = 0; // Match
  }

  public static void main(String[] args) {
    test(10, 5);
    System.out.println(result);  
    test(5, 10);
    System.out.println(result);  
    test(5, 5);
    System.out.println(result); 
  }
}
```

输出结果：

```
1
-1
0
```

<sub>**注解**：`else if` 并非新关键字，它仅是 `else` 后紧跟的一条新 `if` 语句。</sub>

## 迭代语句

**while**，**do-while** 和 **for** 用来控制循环语句（有时也称迭代语句）。

### while

代码示例：

```java
// control/WhileTest.java
// 演示 while 循环
public class WhileTest {
  static boolean condition() {
    boolean result = Math.random() < 0.99; 
    System.out.print(result + ", ");  
    return result;
  }
  public static void main(String[] args) {
    while(condition())
      System.out.println("Inside 'while'");
    System.out.println("Exited 'while'");
  }
}
```

输出结果：

```
true, Inside 'while'
true, Inside 'while'
true, Inside 'while'
true, Inside 'while'
true, Inside 'while'
...________...________...________...________...
true, Inside 'while'
true, Inside 'while'
true, Inside 'while'
true, Inside 'while'
false, Exited 'while'
```

**Math.random()** 方法产生 [0, 1) 之间的一个 **double** 值。

### do-while

```java
do 
	statement
while(Boolean-expression);
```

### for

**for** 循环的形式是：

```java
for(initialization; Boolean-expression; step)
  statement
```

初始化 (initialization) 表达式、布尔表达式 (Boolean-expression) ，或者步进 (step) 运算，都可以为空。每次迭代之前都会判断布尔表达式的结果是否成立。一旦计算结果为 `false`，则跳出循环体。

**for** 循环通常用于“计数”任务。代码示例：

```java
// control/ListCharacters.java

public class ListCharacters {
  public static void main(String[] args) {
    for(char c = 0; c < 128; c++)
      if(Character.isLowerCase(c))
        System.out.println("value: " + (int)c +
          " character: " + c);
  }
}
```

输出结果（前 10 行）：

```
value: 97 character: a
value: 98 character: b
value: 99 character: c
value: 100 character: d
value: 101 character: e
value: 102 character: f
value: 103 character: g
value: 104 character: h
value: 105 character: i
value: 106 character: j
  ...
```

**注意**：变量 **c** 是在 **for** 循环执行时才被定义的，并不是在主方法的开头。**c** 的作用域范围仅在 **for** 循环体内。

上例使用了 **java.lang.Character** 包装类，该类不仅包含了基本类型 `char` 的值，还封装了一些方法。例如静态方法 `isLowerCase()` 来判断字符是否为小写。

#### 逗号操作符

Java 逗号运算符（这里并非指参数之间逗号）仅有一种用法：在 **for** 循环的初始化和步进控制中定义多个变量。我们可以使用逗号分隔多个语句，并按顺序计算这些语句。**注意**：要求定义的变量类型相同。代码示例：

```java
// control/CommaOperator.java
public class CommaOperator {
  public static void main(String[] args) {
    for(int i = 1, j = i + 10; i < 5; i++, j = i * 2) {
      System.out.println("i = " + i + " j = " + j);
    }
  }
}
```

输出结果：

```
i = 1 j = 11
i = 2 j = 4
i = 3 j = 6
i = 4 j = 8
```


上例中 **int** 类型声明包含了 `i` 和 `j`。实际上，在初始化部分我们可以定义任意数量的同类型变量。**注意**：在 Java 中，仅允许 **for** 循环在控制表达式中定义变量。 我们不能将此方法与其他的循环语句和选择语句中一起使用。

## for-in 语法 

Java 5 引入了更为简洁的“增强版 **for** 循环”语法来操纵数组和集合。（更多细节，可参考 [数组](./21-Arrays.md) 和 [集合](./12-Collections.md) 章节内容）。大部分文档也称其为 **for-each** 语法，但为了不与 Java 8 新添的 `forEach()` 产生混淆，所以称之为 **for-in** 循环。

**for-in** 无需你去创建 **int** 变量和步进来控制循环计数。 代码示例：

```java
// control/ForInFloat.java
import java.util.*;

public class ForInFloat {
  public static void main(String[] args) {
    Random rand = new Random(47);
    float[] f = new float[10];
    for(int i = 0; i < 10; i++)
      f[i] = rand.nextFloat();
    for(float x : f)
      System.out.println(x);
  }
}
```

输出结果：

```
0.72711575
0.39982635
0.5309454
0.0534122
0.16020656
0.57799757
0.18847865
0.4170137
0.51660204
0.73734957
```

```java
for(float x : f) {
```

这条语句定义了一个 **float** 类型的变量 `x`，继而将每一个 `f` 的元素赋值给它。

**String** 类有一个方法 `toCharArray()`，返回值类型为 **char** 数组，可以在 **for-in** 循环中遍历它。代码示例：

```java
// control/ForInString.java

public class ForInString {
  public static void main(String[] args) {
    for(char c: "An African Swallow".toCharArray())
      System.out.print(c + " ");
  }
}
```

输出结果：

```
A n   A f r i c a n   S w a l l o w
```

很快我们能在 [集合](./12-Collections.md) 章节里学习到，**for-in** 循环适用于任何可迭代（*iterable*）的对象。

*for-in* 语法可以节省编码时间，提高了代码可读性。

## return

在 Java 中有几个关键字代表无条件分支，这意味无需任何测试即可发生。这些关键字包括 **return**，**break**，**continue** 和跳转到带标签语句的方法，类似于其他语言中的 **goto**。

**return** 关键字有两方面的作用：1.指定一个方法返回值 (在方法返回类型非 **void** 的情况下)；2.退出当前方法，并返回作用 1 中值。我们可以利用 `return` 的这些特点来改写上例 `IfElse.java` 文件中的 `test()` 方法。代码示例：

```java
// control/TestWithReturn.java

public class TestWithReturn {
  static int test(int testval, int target) {
    if(testval > target)
      return +1;
    if(testval < target)
      return -1;
    return 0; // Match
  }

  public static void main(String[] args) {
    System.out.println(test(10, 5));
    System.out.println(test(5, 10));
    System.out.println(test(5, 5));
  }
}
```

输出结果：

```
1
-1
0
```

如果在方法签名中定义了返回值类型为 **void**，那么在代码执行结束时会有一个隐式的 **return**。 也就是说我们不用在总是在方法中显式地包含 **return** 语句。 **注意**：如果你的方法声明的返回值类型为非 **void** 类型，那么则必须确保每个代码路径都返回一个值。

## break 和 continue

**break** 表示跳出当前循环体。而 **continue** 表示停止本次循环，开始下一次循环。

```java
// control/BreakAndContinue.java
// Break 和 continue 关键字

import static onjava.Range.*;

public class BreakAndContinue {
  public static void main(String[] args) {
    for(int i = 0; i < 100; i++) { // [1]
      if(i == 74) break; // 跳出循环
      if(i % 9 != 0) continue; // 下一次循环
      System.out.print(i + " ");
    }
    System.out.println();
    // 使用 for-in 循环:
    for(int i : range(100)) { // [2]
      if(i == 74) break; // 跳出循环
      if(i % 9 != 0) continue; // 下一次循环
      System.out.print(i + " ");
    }
    System.out.println();
    int i = 0;
    //  "无限循环":
    while(true) { // [3]
      i++;
      int j = i * 27;
      if(j == 1269) break; // 跳出循环
      if(i % 10 != 0) continue; // 循环顶部
      System.out.print(i + " ");
    }
  }
}
```

输出结果:

```
0 9 18 27 36 45 54 63 72
0 9 18 27 36 45 54 63 72
10 20 30 40
```

还有一种无限循环的形式： `for(;;)`。 在编译器看来，它与 `while(true)` 无异，使用哪种完全取决于你的编程品味。

## 臭名昭著的 goto

**goto** 起源于[汇编](https://en.wikipedia.org/wiki/Assembly_language)（assembly language）语言中的程序控制：“若条件 A 成立，则跳到这里；否则跳到那里”。

问题不在 **goto**，而在于过度使用 **goto**。在极少数情况下，**goto** 实际上是控制流程的最佳方式。

尽管 **goto** 仍是 Java 的一个保留字，但其并未被正式启用。然而，在 **break** 和 **continue** 这两个关键字的身上，我们仍能看出一些 **goto** 的影子。之所以把它们纳入 **goto** 问题中一起讨论，是由于它们使用了相同的机制：标签。

“标签”是后面跟一个冒号的标识符。代码示例：

```java
label1:
```

对 Java 来说，唯一用到标签的地方是在循环语句之前。代码示例：

```java
label1:
outer-iteration { 
  inner-iteration {
  // ...
  break; // [1] 
  // ...
  continue; // [2] 
  // ...
  continue label1; // 跳到外层循环
  // ...
  break label1; // 中断所有循环
  } 
}
```

下面是 **for** 循环的一个例子：

```java
// control/LabeledFor.java
// 搭配“标签 break”的 for 循环中使用 break 和 continue

public class LabeledFor {
  public static void main(String[] args) {
    int i = 0;
    outer: // 此处不允许存在执行语句
    for(; true ;) { // 无限循环
      inner: // 此处不允许存在执行语句
      for(; i < 10; i++) {
        System.out.println("i = " + i);
        if(i == 2) {
          System.out.println("continue");
          continue;
        }
        if(i == 3) {
          System.out.println("break");
          i++; // 否则 i 永远无法获得自增 
               // 获得自增 
          break;
        }
        if(i == 7) {
          System.out.println("continue outer");
          i++;  // 否则 i 永远无法获得自增 
                // 获得自增 
          continue outer;
        }
        if(i == 8) {
          System.out.println("break outer");
          break outer;
        }
        for(int k = 0; k < 5; k++) {
          if(k == 3) {
            System.out.println("continue inner");
            continue inner;
          }
        }
      }
    }
    // 在此处无法 break 或 continue 标签
  }
}
```

输出结果：

```
i = 0
continue inner
i = 1
continue inner
i = 2
continue
i = 3
break
i = 4
continue inner
i = 5
continue inner
i = 6
continue inner
i = 7
continue outer
i = 8
break outer
```

注意 **break** 会中断 **for** 循环，而且在抵达 **for** 循环的末尾之前，递增表达式不会执行。由于 **break** 跳过了递增表达式，所以递增会在 `i==3` 的情况下直接执行。在 `i==7` 的情况下，`continue outer` 语句也会到达循环顶部，而且也会跳过递增，所以它也是直接递增的。

如果没有 **break outer** 语句，就没有办法在一个内部循环里找到出外部循环的路径。这是由于 **break** 本身只能中断最内层的循环（对于 **continue** 同样如此）。 当然，若想在中断循环的同时退出方法，简单地用一个 **return** 即可。

下面这个例子向大家展示了带标签的 **break** 以及 **continue** 语句在 **while** 循环中的用法：

```java
// control/LabeledWhile.java
// 带标签的 break 和 conitue 在 while 循环中的使用

public class LabeledWhile {
  public static void main(String[] args) {
    int i = 0;
    outer:
    while(true) {
      System.out.println("Outer while loop");
      while(true) {
        i++;
        System.out.println("i = " + i);
        if(i == 1) {
          System.out.println("continue");
          continue;
        }
        if(i == 3) {
          System.out.println("continue outer");
          continue outer;
        }
        if(i == 5) {
          System.out.println("break");
          break;
        }
        if(i == 7) {
          System.out.println("break outer");
          break outer;
        }
      }
    }
  }
}
```

输出结果：

```
Outer while loop
i = 1
continue
i = 2
i = 3
continue outer
Outer while loop
i = 4
i = 5
break
Outer while loop
i = 6
i = 7
break outer
```

大家要记住的重点是：在 Java 里需要使用标签的唯一理由就是因为有循环嵌套存在，而且想从多层嵌套中 **break** 或 **continue**。

## switch

**switch** 适用于整型，它的格式如下：

```java
switch(integral-selector) {
	case integral-value1 : statement; break;
	case integral-value2 : statement;	break;
	case integral-value3 : statement;	break;
	case integral-value4 : statement;	break;
	case integral-value5 : statement;	break;
	// ...
	default: statement;
}
```

下面这个例子可随机生成字母，并判断它们是元音还是辅音字母：

```java
// control/VowelsAndConsonants.java

// switch 执行语句的演示
import java.util.*;

public class VowelsAndConsonants {
  public static void main(String[] args) {
    Random rand = new Random(47);
    for(int i = 0; i < 100; i++) {
      int c = rand.nextInt(26) + 'a';
      System.out.print((char)c + ", " + c + ": ");
      switch(c) {
        case 'a':
        case 'e':
        case 'i':
        case 'o':
        case 'u': System.out.println("vowel");
                  break;
        case 'y':
        case 'w': System.out.println("Sometimes vowel");
                  break;
        default:  System.out.println("consonant");
      }
    }
  }
}
```

输出结果：

```
y, 121: Sometimes vowel
n, 110: consonant
z, 122: consonant
b, 98: consonant
r, 114: consonant
n, 110: consonant
y, 121: Sometimes vowel
g, 103: consonant
c, 99: consonant
f, 102: consonant
o, 111: vowel
w, 119: Sometimes vowel
z, 122: consonant
  ...
```

由于 `Random.nextInt(26)` 会产生 0 到 25 之间的一个值，所以在其上加上一个偏移量 `a`，即可产生小写字母。在 **case** 语句中，使用单引号引起的字符也会产生用于比较的整数值。

请注意 **case** 语句能够堆叠在一起，为一段代码形成多重匹配，即只要符合多种条件中的一种，就执行那段特别的代码。这时也应该注意将 **break** 语句置于特定 **case** 的末尾，否则控制流程会继续往下执行，处理后面的 **case**。在下面的语句中：

```java
int c = rand.nextInt(26) + 'a';
```

此处 `Random.nextInt()` 将产生 0~25 之间的一个随机 **int** 值，它将被加到 `a` 上。这表示 `a` 将自动被转换为 **int** 以执行加法。为了把 `c` 当作字符打印，必须将其转型为 **char**；否则，将会输出整数。

## switch 字符串

Java 7 增加了在字符串上 **switch** 的用法。 以及新式方法：

```java
// control/StringSwitch.java

public class StringSwitch {
  public static void main(String[] args) {
    String color = "red";
    // 老的方式: 使用 if-then 判断
    if("red".equals(color)) {
      System.out.println("RED");
    } else if("green".equals(color)) {
      System.out.println("GREEN");
    } else if("blue".equals(color)) {
      System.out.println("BLUE");
    } else if("yellow".equals(color)) {
      System.out.println("YELLOW");
    } else {
      System.out.println("Unknown");
    }
    // 新的方法: 字符串搭配 switch
    switch(color) {
      case "red":
        System.out.println("RED");
        break;
      case "green":
        System.out.println("GREEN");
        break;
      case "blue":
        System.out.println("BLUE");
        break;
      case "yellow":
        System.out.println("YELLOW");
        break;
      default:
        System.out.println("Unknown");
        break;
    }
  }
}
```

输出结果：

```
RED
RED
```

**switch**其实就是一个逻辑扩展的语法糖。它使得结果更清晰，更易于理解和维护。

## 本章小结

本章总结了我们对大多数编程语言中出现的基本特性的探索：计算，运算符优先级，类型转换，选择和迭代。下一章的内容涵盖了 Java 编程中的重要问题：对象的[初始化和清理](./06-Housekeeping.md)。紧接着，还会介绍[封装](./07-Implementation-Hiding.md)（implementation hiding）的核心概念。

