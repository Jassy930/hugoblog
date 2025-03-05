---
title: C补遗：C语言里不那么面熟的关键字
comment: true
toc: true
tags:
  - C
abbrlink: f3b58568
---

C用了很久了，但是也没有说很系统平铺的整体看过，一直是用什么会什么，不用什么就过去了。既然C已经被我当做是一门已经掌握的语言，所以就重新扫一遍C里面不那么面熟的东西或者理解有偏差的东西，当做是个记录以备复习使用，也可以对C打个结了。

## auto && register && static

怎么也没想到会把这三个放在一起，一个是C++中常用的声明类型的关键字，一个没见过的，和一个还算常用的，这里主要理解的偏差还是在 `auto` 上。

在C++中，auto作为一种自动判断变量类型的定义存在，比如当我使用 `auto c = 10;` 去定义 `c` 这个变量的时候，是和 `int c = 10;` 的作用是一样的，这样可以节省很多去考虑这个变量具体是什么类型以及大量的敲字母的时间，还可以避免一些打字错误，比如  `vector<map<int,string>>::iterator a =` 这种东西可以直接 `auto a =` 还是挺爽的。顺便这里提一嘴，C++中的 `auto` 后面必须赋值，否则编译器不知道到底要初始化成什么类型的。
<!-- more -->
在C中，`auto` 关键字是用来指定变量的存储位置的。这也是为什么要和 `static` 和 `register` 一起讲。 `static` 说过很多遍也用过很多遍了，是存储在静态存储区的，而这个不太常见的 `register` 就是指的平时我们使用中的变量，是直接存储在堆或者栈中的，所以 `auto` 关键字就只是用来指定是用 `static` 修饰的还是用 `register` 修饰的，也就是新建变量的存储位置，举例：

``` c
int a = 10; //平时这样定义就相当于 auto in a = 10; auto相当于省略
register int b = 10;
static int c = 10;
```

注意的是，平时我们在 `int a = 10;` 这样定义的时候就相当于省略了 `auto` 关键字，上面这段在定义的时候就是 a 和 b 定义是在堆或者栈中的，c定义在静态存储区中。而且在使用了 `auto` `register` `static` 修饰后，仍然需要增加变量类型，因为他们并不是定义变量类型的，而是变量的存储区域。

C中的 `auto` 并不是复杂，而是主要要注意一下与C++中的 `auto` 的区别。

## C99 新增关键字

后面这两个部分我定义为《很少见看上去很新但有可能我以后再也不会用到的关键字》，是新定义的关键字。随着时代发展社会进步，所以 C 的标准也在一直跟着更新，这两个部分就是在新的标准下引入的新的关键字。

### _Bool

这个挺明显的，就是补齐C中没有 `bool` 类型的缺憾了，不同程序员自己对 `bool` 类型的定义都不一样，所以可能会产生分歧，这个可以统一一下。定义的true为1，false为0，类型的具体大小由编译器去决定，标准只要求了大小要能够存放0和1这两个值，gcc是使用char实现的。需要引入 `<stdbool.h>`

另外，对bool类型赋0和1以外的值，gcc会强制赋值成1，所以 `bool` 类型的值只存在0和1两种可能

另另外，C++中是 `bool` 类型，C中是 `_Bool` 类型。

### _Complex && _Imaginary

复数！复数！复数你敢想？就在C里面，复数！

需要引入 `<complex.h>` , `_Imaginary` 作为虚数，但是并没有强制要求编译器去实现虚数部，所以很多都是只实现了 `complex`。

`complex` 分三种类型:

``` c
float _Complex fc = 1.0f + 1.0if;
double _Complex fc = 1.0 + 1.0i;
long double _Complex fc = 1.0 + 1.0i;
```

### inline

内联函数，与C++中的 `inline` 作用相同用来提高程序运行效率，减少函数调用开支。但是只是建议编译器这样去做，编译器可以选择性忽略（可配置）。

``` c
inline int f(int a)
{
    return a+1;
}
```

内联函数中不能定义可改变的 `static` 变量，不能引用具有内部链接的变量。

建议内联函数只用在函数体比较简单的情况下，否则代码展开太过庞大。

### restrict

这是一个只对于指针的修饰符，被 `restrict` 修饰的指针所指向的内容的所有操作都是基于这个指针进行的，不会从其他地方进行修改，这样可以帮助编译器进行更好的代码优化提高代码效率。如：

``` c
// 某 restrict 指针 a
void f(int *restrict a)
{
    *a++;
    *a++;
}
```

这种情况下，编译器知道在f处理过程中不可能从别的地方对 `*a` 进行修改，所以可以直接将两句优化为 `*a+=2;`

并且两个不同的 `restrict` 指针必定是两个分别指向不同区域的指针，不会是相同的指针，可以因此对一些函数输入参数进行限定。如：

``` c
void * memcpy(void * restrict s1, const void * restrict s2, size_t n);
void * memove(void * s1, const void * s2, size_t n);
```

对于 `memcpy` 来讲，两个指针区域的重叠是存在危险的行为，所以需要使用 `restrict` 进行修饰。

另外，这个关键字C++中并没有引入。

另另外，在linux中 `restrict` 需要替换为 `_restrict`

## C11 新增关键字

### _Alignas && _Alignof

加个 cppreference 的链接辅助：

[_Alignas](https://zh.cppreference.com/w/c/language/_Alignas)

[_Alignof](https://zh.cppreference.com/w/c/language/_Alignof)

修改被声明对象的对齐说明，是的，这个对齐说明就是 `struct` 的那个对齐说明，后面可以放置数字或者一个类型，如：

``` c
#include <stdalign.h>
#include <stdio.h>
//  每一个 struct sse_t 类型的对象会在 16 字节边界对齐
// （注意：需要支持 DR 444 ）
struct sse_t
{
  alignas(16) float sse_data[4];
};

// 这种 struct data 的每一个对象都会在 128 字节边界对齐
struct data {
  char x;
  alignas(128) char cacheline[128]; // 过对齐的 char 数组对象，
                                    // 不是过对齐的 char 对象的数组
};

int main(void)
{
    printf("sizeof(data) = %zu (1 byte + 127 bytes padding + 128-byte array)\n",
           sizeof(struct data));
    printf("alignment of sse_t is %zu\n", alignof(struct sse_t));
    alignas(2048) struct data d; // 此 struct data 的实例会更严格地对齐
}
```

输出:

``` output
sizeof(data) = 256 (1 byte + 127 bytes padding + 128-byte array)
alignment of sse_t is 16
```

需要注意的是如果 `alignas` 后面的表达式为0会被忽略，如果定义了多个会使用最严格的，这个最严格的描述包括无任何 `alignas` 描述的原始对齐方式。

`_Alignof` 就是获取这个类型的对齐要求。如下：

``` c
#include <stdio.h>
#include <stddef.h>
#include <stdalign.h>

int main(void)
{
    printf("Alignment of char = %zu\n", alignof(char));
    printf("Alignment of max_align_t = %zu\n", alignof(max_align_t));
    printf("alignof(float[10]) = %zu\n", alignof(float[10]));
    printf("alignof(struct{char c; int n;}) = %zu\n",
            alignof(struct {char c; int n;}));
}
```

输出：

``` output
Alignment of char = 1
Alignment of max_align_t = 16
alignof(float[10]) = 4
alignof(struct{char c; int n;}) = 4
```

### _Atomic

是用于多线程的玩意，用这个修饰符限定的变量的操作只能是**原子性**的，类似于互锁，同一时间只能有一个线程访问这个参数，类似于线程锁。定义方式：

``` c
_Atomic int c = 10;
```



### _Generic

泛型。

``` c
#include<stdio.h>
#define GENERAL_ABS(x) _Generic((x),int:abs,float:fabsf,double:fabs)(x)

int main(void)
{
    printf("intabs:%d\n",GENERAL_ABS(-12));
    printf("floatabs:%f\n",GENERAL_ABS(-12.04f));
    printf("doubleabs:%f\n",GENERAL_ABS(-13.09876));

    int a=10;
    int b=0,c=0;
    _Generic(a+0.1f,int:b,float:c,default:b)++;
    printf("b=%d,c=%d\n",b,c);
    _Generic(a+=1.1f,int:b,float:c,default:b)++;
    printf("a=%d,b=%d,c=%d\n",a,b,c);
```

对不同的输入参数的类型执行不同的函数，x为函数的输入参数，后面是对应不同输入参数类型的操作，最后可以加一个 `defalut` 用于其他的输出结果。

### _Noreturn

[cppreference](https://en.cppreference.com/w/c/language/_Noreturn)

这是一个描述函数的关键词，用于表示该函数执行完毕后不会返回，请注意这里是**不会返回**，而不是像 `void` 一样是返回空，而是根本不会返回，编译器也会因此对代码进行优化。例如:

``` c
noreturn void stop_now(int i) // or _Noreturn void stop_now(int i)
{
    if (i > 0) exit(i);
}
```

但是这个说明符并没有什么**实际**的作用，这个实际体现在，他是程序员对编译器的一种承诺，我不会让这个函数返回的，你也别让他返回，只是一个提示、通知的作用，但是你要在函数里非要返回，编译器也不会在你返回的时候强行停止掉。that's it.

### _Static_assert

[cppreference](https://en.cppreference.com/w/c/error/static_assert)

静态断言，如果第一个参数的值为false（可以输入一个表达式或者string），那么编译器会报出第二个string的错误，同时编译失败，如果第一个表达式的值为true的话没有任何影响。

``` c
#include <assert.h>
int main(void)
{
    static_assert(2 + 2 == 4, "2+2 isn't 4");      // well-formed
    static_assert(sizeof(int) < sizeof(char),
                 "this program requires that int is less than char"); // compile-time error
}
```

### _Thread_local

> 便捷宏，可用于指定对象具有线程本地存储持续时间。

大家都是这么说的，但是没找到 C 的很确切的定义和说明。但是它是一个描述变量的，是用来描述变量的生命周期为线程存储期，可以和 `static` 和 `extern` 结合来指定内部或者外部链接，具体的声明周期描述如下：

> ***线程（thread）***存储期。对象的存储在线程开始时分配，而在线程结束时解分配。每个线程拥有其自身的对象实例。唯有声明为 `thread_local` 的对象拥有此存储期。 `thread_local` 能与 `static` 或 `extern` 一同出现，以调整连接。关于具有此存储期的对象的初始化的细节。

## 总结

其实这样顺下来，不清楚不了解的东西还很多。主要原因还是因为在做项目的过程中很多东西没有很高的要求和需求，那么也不需要用到，这样的话没有动力不会去了解去理解，一直只是在嵌入式中使用的C，所以很多东西比如线程复数等等根本没有想到会出现，也更加没有用过了，这边只是一个对C语言知识的补遗，我不能通过一个补遗就了解这些知识，但是可以通过这个补遗知道有这样一回事，提前有准备，不会突然被一棒子打到。嗯。
