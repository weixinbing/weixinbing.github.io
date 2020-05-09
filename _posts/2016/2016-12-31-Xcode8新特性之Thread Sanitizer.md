---
title: Xcode8 新特性之 Thread Sanitizer
date: 2016-12-31
tags: Xcode
categories: Xcode
---

多线程问题调试工具

<!-- more -->

### Thread Sanitizer 的使用

打开 Xcode 8 新增的多线程问题调试工具 Thread Sanitizer
![Xcode Thread Sanitizer](http://oihqdel9t.bkt.clouddn.com/2012/12/Xcode%20Thread%20Sanitizer.png)

运行下图中代码，检测 data race
![count data race](http://oihqdel9t.bkt.clouddn.com/2012/12/count%20data%20race.png)
很直观，Xcode 直接提示你发生了 data race 的变量及其代码位置，同时还清晰的展示了函数当前的各线程调用栈，十分清晰，接下来你要做的就是增加同步操作，比如加锁，从而消除 data race，再运行测试是否生效。

> 最后计算的结果有很大概率小于 20000，原因是 count ++为非原子操作。这也是 data race 的场景，这种 race 没有 crash 也没有 memory corruption，因此有些人把这种 race 称作 benign race(良性的 race)。不过上面提到的 WWDC 视频中，苹果的工程师说到：
> `There is No Such Thing as a “Benign” Race`
> 意思是，只要发生 data race，就没有良性一说了，因为虽然程序没有 crash，但 count 最后的值还是出错了，这种 错误必然会导致逻辑上的错误，如果这个 count 值代表的是你银行卡余额，你应该会更加同意苹果工程师的观点。

data race 定义：

- 当至少有两个线程同时访问同一个变量，而且至少其中有一个是写操作时，就发生了 data race

### Thread Sanitizer 的工作原理

> 在 WWDC 的视频中也介绍过了，大家可以仔细看下视频，大致原理是记录每个线程访问变量的信息来做分析，值得一提的是，现阶段的 Thread Sanitizer 最多只同时记录 4 个线程的访问信息，在复杂的场景下，可能出现偶尔检测不出 data race 的场景，所以需要长时间经常性的运行来尽可能多的发现 data race，这也是为什么苹果建议默认开启 Thread Sanitizer，而且 Thread Sanitizer 造成的额外性能损耗非常之小。
