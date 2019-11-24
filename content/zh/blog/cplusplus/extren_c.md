---
title: "关键字 extern 在 C 语言的使用"
authors: [kiki]
tags: [c++]
categories: [blog]
draft: false
---

- [前言](#%e5%89%8d%e8%a8%80)
- [变量或函数的声明和定义](#%e5%8f%98%e9%87%8f%e6%88%96%e5%87%bd%e6%95%b0%e7%9a%84%e5%a3%b0%e6%98%8e%e5%92%8c%e5%ae%9a%e4%b9%89)
- [声明和定义全局变量的最好方式](#%e5%a3%b0%e6%98%8e%e5%92%8c%e5%ae%9a%e4%b9%89%e5%85%a8%e5%b1%80%e5%8f%98%e9%87%8f%e7%9a%84%e6%9c%80%e5%a5%bd%e6%96%b9%e5%bc%8f)
- [关键点 1：可以声明多次但初始化一次](#%e5%85%b3%e9%94%ae%e7%82%b9-1%e5%8f%af%e4%bb%a5%e5%a3%b0%e6%98%8e%e5%a4%9a%e6%ac%a1%e4%bd%86%e5%88%9d%e5%a7%8b%e5%8c%96%e4%b8%80%e6%ac%a1)
- [关键点 2：默认存储类是 extern](#%e5%85%b3%e9%94%ae%e7%82%b9-2%e9%bb%98%e8%ae%a4%e5%ad%98%e5%82%a8%e7%b1%bb%e6%98%af-extern)
- [关键点 3：extern 变量或程序对整个程序可见](#%e5%85%b3%e9%94%ae%e7%82%b9-3extern-%e5%8f%98%e9%87%8f%e6%88%96%e7%a8%8b%e5%ba%8f%e5%af%b9%e6%95%b4%e4%b8%aa%e7%a8%8b%e5%ba%8f%e5%8f%af%e8%a7%81)
- [关键点 4：extern 作用于变量](#%e5%85%b3%e9%94%ae%e7%82%b9-4extern-%e4%bd%9c%e7%94%a8%e4%ba%8e%e5%8f%98%e9%87%8f)
  - [只用于声明变量](#%e5%8f%aa%e7%94%a8%e4%ba%8e%e5%a3%b0%e6%98%8e%e5%8f%98%e9%87%8f)
  - [全局变量自动初始化](#%e5%85%a8%e5%b1%80%e5%8f%98%e9%87%8f%e8%87%aa%e5%8a%a8%e5%88%9d%e5%a7%8b%e5%8c%96)
  - [不能局部地初始化 extern 变量](#%e4%b8%8d%e8%83%bd%e5%b1%80%e9%83%a8%e5%9c%b0%e5%88%9d%e5%a7%8b%e5%8c%96-extern-%e5%8f%98%e9%87%8f)
  - [不能写全局的赋值语句](#%e4%b8%8d%e8%83%bd%e5%86%99%e5%85%a8%e5%b1%80%e7%9a%84%e8%b5%8b%e5%80%bc%e8%af%ad%e5%8f%a5)
  - [对类成员无效](#%e5%af%b9%e7%b1%bb%e6%88%90%e5%91%98%e6%97%a0%e6%95%88)
- [常见错误](#%e5%b8%b8%e8%a7%81%e9%94%99%e8%af%af)
  - [未定义的行为](#%e6%9c%aa%e5%ae%9a%e4%b9%89%e7%9a%84%e8%a1%8c%e4%b8%ba)
  - [外部定义](#%e5%a4%96%e9%83%a8%e5%ae%9a%e4%b9%89)
  - [多重外部定义](#%e5%a4%9a%e9%87%8d%e5%a4%96%e9%83%a8%e5%ae%9a%e4%b9%89)
  - [头文件中变量的声明](#%e5%a4%b4%e6%96%87%e4%bb%b6%e4%b8%ad%e5%8f%98%e9%87%8f%e7%9a%84%e5%a3%b0%e6%98%8e)
- [参考](#%e5%8f%82%e8%80%83)

## 前言

- `extern` 用于声明 C 语言中的外部变量和函数。这个修饰符用于所有数据类型，比如 int，float，double，array，pointer，structure，function 等
- 范围(scope)：不绑定到任何函数。作用域整个程序，是全局的
- 默认值(default value)：全局变量的默认初始化值是 0(或 null)
- 生命周期(lifetime)：直到整个程序执行结束
- `extern` 告诉编译器变量或函数(非静态的)都可以在链接时找到。适用于在模块之间共享某些全局变量，但是不想把它们放在一个头文件，或者在一个头文件中定义它们
  - 大部分编译器编译器会优化程序确保它们不会为 `extern` 对象保留内存，因为编译器知道定义它们的模块会保留内存

## 变量或函数的声明和定义

- 声明(declaration)：声明变量或函数存在程序的某个地方，但是不为它们分配内存。确定了变量或函数的类型
  - 声明一个变量时，程序知道这个变量的类型；声明一个函数时，程序知道函数的参数、数据类型、参数顺序和函数返回类型
  - 声明是编译器需要的用于接受对标识符的引用
- 定义(definition)：既包含声明的作用，也为变量或函数分配内存。可以认为定义是声明的一个超集
- 因此一个函数或变量可以声明多次，但是只能定义一次(即同一个函数或变量不能存在两个位置)
  - 定义是对标识符的实例化/实现
  - 定义时链接器需要的用于链接对这些实体的引用
  - 单一定义原则(One Definition Rule)：编译单元不应该对任意变量、函数、类类型、枚举类型或模板有多余一个的定义

## 声明和定义全局变量的最好方式

- 声明和定义全局变量的清晰、可靠的方式是使用一个头文件，该头文件包含变量的 `extern` 声明
  - 定义这些变量的源文件和引用这些变量的源文件包含此头文件
  - 对于每一个程序，有且只有一个源文件定义这些变量
  - 对于每一个程序，有且只有一个头文件声明这些变量
  - 这个头文件时重要的，它使能在独立的翻译单元(TU，translation units，源文件)之间交叉检查，同时确保一致性
- 完整的程序可能还需要全局函数。C99 和 C11 要求函数在使用之前必须是已经声明或定义过的。使用一个头文件包含全局函数的 `extern` 声明。也可以不加 `extern`
- 尽量避免使用全局函数——可以使用全局变量

## 关键点 1：可以声明多次但初始化一次

- 一个特殊的 `extern` 变量或函数可以声明多次，但是只初始化一次。但是不可以声明一次，初始化多次

```c
#include <stdio.h>
extern int i;//declaring variable i
int i=25;//initializing variable i
extern int i;//again declaring variable i
int main() {
  extern int i;//again declaring variable i
  printf("%d\n", i);
  return 0;
}
// 输出：25
```

```c
#include <stdio.h>
extern void sum(int,int);//by default it is extern function
int main() {
  extern void sum(int,int);//by default it is extern function
  int a=5,b=10;
  sum(a,b);
  return 0;
}
void sum(int a, int b) {
  printf("%d\n", a+b);
}
```

```c
#include <stdio.h>
extern int i;//declaring variable i
int i=25;//initializing variable i
int main() {
  printf("%d\n", i);
  return 0;
}
int i=20;//again initializing variable i
// 输出：编译错误(error: redefinition of ‘i’)
```

## 关键点 2：默认存储类是 extern

- `extern` 是所有全局变量和函数的默认存储类(storage class)，即全局变量和函数默认对整个程序可见，不需要声明或定义 `extern` 函数。使用 `extern` 关键字是多余的
- 编译器会在全局函数的声明和定义前面自动加上 `extern`
- 在下面两个测试代码中，变量 `i` 都是 `extern` 变量

```c
// test1.c
#include <stdio.h>
int i;//definition of i: by default it is extern variable
int main() {
  printf("%d\n", i);
  return 0;
}
// 输出：0
```

```c
// test2.c
#include <stdio.h>
extern int i;//extern variable
int main() {
  printf("%d\n", i);
  return 0;
}
// 输出：编译错误，未定义的引用(undefined reference to i)
```

```c
// test3.c
#include <stdio.h>
void sum(int,int);//by default it is extern function
int main() {
  int a=5,b=10;
  sum(a,b);
  return 0;
}
void sum(int a, int b) {
  printf("%d\n", a+b);
}
// 输出：15
```

## 关键点 3：extern 变量或程序对整个程序可见

- `extern` 关键字用于扩展变量或函数的可见性。如果全局声明一个 `extern` 变量或函数，那么它的可见性是整个程序，这个程序可能包含一个或多个文件。比如一个 C 程序，包含两个文件 `one.c` 和 `two.c`
- 下面程序的输出是 30

```c
//one.c
#include <conio.h>
int i=25;//by default extern variable
int j=5;//by default extern variable
//above two lines is initialization of variable i and j
void main() {
  clrscr();
  sum();
  getch();
}
```

```c
//two.c
#include <stdio.h>
extern int i;//declaration of variable i
extern int j;//declaration of variable j
//above tow lines will search the initialization statement of variable i and j either in two.c(if initialized variable id static and static)
// or one.c(if initialized variable is extern)
void sum() {//by default extern function
  int s;
  s = i + j;
  printf("%d\n", s);
}
```

- 一个 `extern` 变量或函数有外部(external)链接，一个外部链接的变量或函数对所有文件可见
  - `extern` 作用于函数只是告诉编译链接是外部的；作用于变量只声明变量而不会定义(初始化或实例化)变量

## 关键点 4：extern 作用于变量

### 只用于声明变量

- 当对变量使用 `extern` 修饰符时，它只用于声明(比如不会为这些变量分配内存)。因此在上述 `test2.c` 中，编译器报错`undefined symbol`。如果要定义变量(比如为 `extern` 变量分配内存)，必须初始化变量
- 初始化 `extern` 变量即是定义 `extern` 变量

```c
#include <stdio.h>
extern int i=10;//extern variable
int main() {
  printf("%d\n", i);
  return 0;
}
// 输出：10
// warning: ‘i’ initialized and declared ‘extern’
// 如果声明时也提供了初始化，那么会为变量分配内存，该变量认为是被定义过的
```

- 编译警告参考[warning in extern declaration](https://stackoverflow.com/questions/4268589/warning-in-extern-declaration)

```c
#include <stdio.h>
extern int i;//extern variable
int main() {
  return 0;
}
// 编译成功。只声明变量 i 但是未使用，不会报错
```

- 如果我们声明一个变量是 `extern`，那么编译器会搜索这个变量是否已经初始化。如果已经初始化为 `extern` 或 `static` 则成功。否则编译器会报错
- **修正：初始化为 static 仍然报错????**

```c
#include <stdio.h>
int main() {
  extern int i;//it will search the initialization of i
  printf("%d\n", i);
  return 0;
}
int i=20;//initialization of extern variable i
// 输出：20
```

```c
#include <stdio.h>
int main() {
  extern int i;//it will search the initialization of i
  printf("%d\n", i);
  return 0;
}
static int i=20;//initialization of static variable i
// 输出：编译错误(error: static declaration of ‘i’ follows non-static declaration)
```

### 全局变量自动初始化

- 如果全局变量不适用 `extern` 关键字，编译器会使用默认值自动初始化 `extern` 变量
- `extern` 整数类型变量的默认初始化值是 0 或者 null

```c
#include <stdio.h>
char c;
int i;
float f;
char *str;
int main() {
  printf("%d %d %f %s\n", c, i, f, str);
  return 0;
}
// 输出：0 0 0.000000 (null)
```

### 不能局部地初始化 extern 变量

- 不能在任何代码块内部局部地初始化 `extern` 变量，不论是声明时初始化还是初始化和声明分开。我们只能全局地初始化 `extern` 变量

```c
#include <stdio.h>
int main() {
  extern int i=10;
  printf("%d\n", i);
  return 0;
}
// 输出：编译错误(error: ‘i’ has both ‘extern’ and initializer)
```

```c
#include <stdio.h>
int main() {
  extern int i;//declaration of extern variable i
  int i=10;//try to locally initialize extern variable i
  printf("%d\n", i);
  return 0;
}
// 输出：编译错误(error: declaration of ‘i’ with no linkage follows extern declaration)
```

```c
#include <stdio.h>
extern int i;//declaration of extern variable i
int main() {
  int i=10;//declare and define a local variable
  printf("%d\n", i);//the i is local
  return 0;
}
// 输出：10
```

```c
#include <stdio.h>
int main() {
  extern int i;//declaration of extern variable i, its memory is not allocated
  i=10;//try to change the value of variable i to 10, but it doesn't exist
  printf("%d\n", i);
  return 0;
}
// 输出：编译错误(两处错误：undefined reference to i)
```

### 不能写全局的赋值语句

- 在声明变量时给变量赋值叫做初始化(initialization)
- 不在变量声明时给变量赋值叫做赋值(assignment)

```c
#include <stdio.h>
extern int i;//declaring variable i
int i=25;//initializing variable i
i=20;
int main() {
  printf("%d\n", i);
  return 0;
}
// 输出：编译错误(error: redefinition of ‘i’)
```

```c
#include <stdio.h>
extern int i;//declaring variable i
int main() {
  i=20;//assignment
  printf("%d\n", i);
  return 0;
}
int i=25;//initialization
// 输出：20
```

### 对类成员无效

- `extern "C"` 被类成员忽略

## 常见错误

### 未定义的行为

- [Undefined behavior](http://port70.net/~nsz/c/c11/n1570.html#6.9)：使用了一个带外部链接的标识符，但是程序中不存在该标识符的外部定义，或者没有使用此标识符但是有多处定义此标识符

### 外部定义

- [External definitions](http://port70.net/~nsz/c/c11/n1570.html#6.9p5)：外部定义指一个外部声明，同事也是函数(除了内联函数)或对象的定义。如果一个表达式中使用了一个有外部链接的标识符(除了作为 `sizeof`或`_Alignof`运算符的操作数的一部分，这些运算符的结果是一个证书常数)，程序的其它地方应该有且仅有一个对此标识符的外部定义
- 因此，如果一个声明为外部链接的标识符未在表达式中被使用，不应该有它的外部定义

### 多重外部定义

- [Multiple external definitions](http://port70.net/~nsz/c/c11/n1570.html#J.5.11)：对于一个对象的标识符可能有多于一处的外部定义，这些定义可能有也可能没有显式使用 `extern` 关键字；如果这些定义不一致，或者多于一处有初始化，就会导致 undefined behavior

### 头文件中变量的声明

- 声明 `int some_var;`：如果一个头文件不使用 `extern` 定义一个变量，那么每个包含此头文件的文件都会尝试创建一个此变量的一个定义。但是 C 标准不确保这个一定会正常工作
- 声明 `int some_var = 13;`：如果头文件定义并初始化一个变量，那么在给定的程序中只有一个源文件可以使用这个头文件。因为头文件主要是用来共享信息的，创建一个只能使用一次的头文件不是好的做法
- 声明 `static int some_var =3;`：如果头文件定义一个静态变量(不论是否初始化)，那么每个源文件都会有此“全局”变量的一份私有拷贝。而且，如果这个变量是一个复杂的数组，那么会导致大量代码的拷贝

## 参考

- [extern keyword in c](https://www.cquestions.com/2011/02/extern-keyword-in-c.html)
- [Understanding “extern” keyword in C](https://www.geeksforgeeks.org/understanding-extern-keyword-in-c/)
- [How do I use extern to share variables between source files](https://stackoverflow.com/questions/1433204/how-do-i-use-extern-to-share-variables-between-source-files)
- [DECLARATIONS V.S. DEFINITIONS](https://www.dreamincode.net/forums/topic/171468-declarations-vs-definitions/)
