---
title: iOS中的关键字const／static／extern
date: 2016-12-26 22:36:36
tags: 关键字
categories: iOS
---

#### const:
const只修饰其右边的变量，被修饰的变量是只读的；
const常量不能被修改，不能用来定义数组的长度，也不能放在case关键字后面。
```objectivec
const int *p
int const *p	
// *p只读, p变量(这2种没有区别)

int * const p	
// *p变量, p只读

const int * const p	  
// *p只读, p只读

int const * const p   
// *p只读, p只读
```
<!-- more -->
>const常量误区：
   `const int n = 5; int a[n];`
   上述代码中变量n被修饰为只读变量，可惜再怎么修饰也不是“常量”，而数组定义时长度必须是“常量”，因此报错。

 >const int MAX_LENGTH = 100; //这不是“常量”，而是一个只读变量。

#### static
##### 1. 修饰局部变量
保证局部变量永远只初始化一次，在程序的运行过程中永远只有一份内存， 生命周期类似全局变量了，但是作用域不变。

##### 2. 修饰全局变量
使全局变量的作用域仅限于当前文件内部，不能通过extern来引用

##### 3. 修饰函数
被修饰的函数被称为静态函数，使得外部文件无法访问这个函数，OC语言中很少使用。


#### extern
它的作用是声明外部全局变量

苹果推荐extern声明全局变量（不建议使用define），优势是保持常量绝对不会被修改，并且还带有初始化的类型信息。
通常在.h中声明，在.m中实现
```objectivec
// .h声明
extern NSString * const NAME;

// .m实现
NSString * const NAME = @"XXX";
```

>const是用来定义一个常量。而static在C语言中（OC中延用）表明此变量只在该变量的输出文件中可用(.m文件)，如果你不加“static”符号，那么编译器就会对该变量创建一个“外部符号”，后果是什么呢？
>
你可以尝试在不同文件中加入以下代码：
`NSString  * const kUserName = @"StrongX";`
可能尽管文件之间并没有相互引用，不存在属性名重复的问题（因为这并不是一个属性，这是一个外部符号）,但是编译器还是报错了:
`duplicate symbol XXX in: ....
clang: error: linker command failed with exit code 1 (use -v to see invocation)`
>
它会告诉你在两个目标文件(.o文件是.m文件编译后的输出文件)有一个重复的符号。(OC中没有类似C++中的名字空间的概念)
所以当你在你自己的.m文件中需要声明一个只有你自己可见的局部变量(k开头)的变量的时候
`一定要同时使用“static”和“const”两个关键字。`




