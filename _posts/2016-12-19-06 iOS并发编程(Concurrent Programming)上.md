---
title: iOS并发编程(Concurrent Programming)
tags: [多线程, 并发编程]
categories: iOS
---

多线程和GCD
<!-- more -->
## 术语
进程(process):
- 正在运行中的程序(可执行文件)称为进程
- 拥有独立的虚拟内存空间和系统资源，包括端口权限等
- 当一个进程的主线程退出时，这个进程就结束了

线程(thread): 
- 线程是进程中一个独立的代码执行路径(控制单元)
- 一个进程中至少包含一条线程，即主线程（又叫UI线程）
- 可以将耗时的执行路径(如：网络请求)放在其他线程中执行

>在 iOS 中，线程的底层实现是基于 `POSIX threads API` 的，也就是我们常说的 pthreads


任务(task):
- 需要执行的工作，是一个抽象的概念
- 通俗的说，就是一段代码

队列(queue):
- 串行队列，队列中的任务只会顺序执行
- 并行队列，队列中的任务通常会并发执行

>iOS 系统就是使用队列来进行任务调度的，它会根据调度任务的需要和系统当前的负载情况动态地创建和销毁线程，而不需要我们手动地管理。

同步和异步:
- 串行与并行针对的是队列，而同步与异步，针对的则是线程。
- 最大的区别在于，同步操作要阻塞当前线程，必须要等待同步操作中的任务执行完，返回以后，才能继续执行下一任务；而异步操作则是不用等待

并发(concurrent)：
- 同时运行多个任务。这些任务可能是以在单核 CPU 上分时（时间共享）的形式同时运行，也可能是在多核 CPU 上以真正的并行方式来运行。

>调度原理
- 多线程可以在单核 CPU 上同时（或者至少看作同时）运行。操作系统将短暂的时间片(Timeslice)分配给每一个线程轮流使用CPU
- 由于CPU对每个时间片的处理速度非常快, 因此，用户看来好像这些任务在同时执行的
- 如果 CPU 是多核的，那么线程就可以真正的以并发方式被执行，从而减少了完成某项操作所需要的总时间。


## 多线程优势、弊端和误区
优势:
- 充分发挥多核处理器优势，将不同线程任务分配给不同的处理器，真正进入“并行运算”状态
- 将耗时的任务分配到其他线程执行，由主线程负责统一更新界面会使应用程序更加流畅，用户体验更好
- 当硬件处理器的数量增加，程序会运行更快，而程序无需做任何调整

弊端:
- 新建线程会消耗内存空间和CPU时间，线程太多会降低系统的运行性能

误区:
- 多线程技术是为了并发执行多项任务，不会提高单个算法本身的执行效率

## iOS的三种多线程技术
### NSThread
NSThread 是 Objective-C 对 pthread 的一个封装。
- 使用NSThread对象建立一个线程非常方便
- 但是！要使用NSThread管理多个线程非常困难，不推荐使用
- 技巧！使用[NSThread currentThread]跟踪任务所在线程(包括GCD和Operation Queues)

### GCD
为了让开发者更加容易的使用设备上的多核CPU，苹果在 OS X 10.6 和 iOS 4 中引入了 Grand Central Dispatch（GCD）。
- 是基于C语言的底层API
- 用Block定义任务，使用起来非常灵活便捷
- 提供了更多的控制能力以及操作队列中所不能使用的底层函数

### Operation Queues
操作队列（operation queue）是由 GCD 提供的一个队列模型的 Cocoa 抽象。
- 是使用GCD实现的一套Objective-C的API
- 是面向对象的多线程技术
- 提供了一些在GCD中不容易实现的特性，如：限制最大并发数量、操作之间的依赖关系

NSOperationQueue 有两种不同类型的队列：主队列和自定义队列。
```objectivec
[NSOperationQueue mainQueue] //获取主队列
NSOperationQueue *queue = [[NSOperationQueue alloc] init]; //自定义队列
```
主队列运行在主线程之上，而自定义队列在后台执行。在两种类型中，这些队列所处理的任务都使用 NSOperation 的子类来表述。

## Grand Central Dispatch（GCD）
GCD的基本思想是就将操作放在队列中去执行
>操作使用Blocks定义
>队列负责调度操作执行所在的线程以及具体的执行时间
>队列的特点是先进先出(FIFO)的，新添加至队列的操作都会排在队尾

提示:
- GCD的函数都是以dispatch开头的

操作:
- dispatch_async 异步操作
- dispatch_sync 同步操作

注：
- 队列不是线程？也不表示对应的CPU
- 队列就是负责调度的! 谁空闲，就把任务给谁！
- 多线程技术的目的，就是为了在一个CPU上实现快速切换！

### GCD队列(dispatch_queue_t)
GCD 公开有 5 个不同的队列：运行在主线程中的 main queue，3 个不同优先级的后台队列，以及一个优先级更低的后台队列（用于 I/O）。 另外，开发者可以创建自定义队列：串行或者并行队列。自定义队列非常强大，在自定义队列中被调度的所有 block 最终都将被放入到系统的全局队列中和线程池中。

![GCD队列示意图](http://oihqdel9t.bkt.clouddn.com/2016/12/Blog/gcd-queues.png)
>无论什么队列和什么任务，线程的创建和回收不需要程序员参与。 
线程的创建回收工作是由队列负责的
“并发”编程，为了让程序员从负责的线程控制中解脱出来！只需要面对队列和任务！

#### GCD串行队列

```objectivec
dispatch_queue_t q = dispatch_queue_create("com.name.s", DISPATCH_QUEUE_SERIAL);
//同步操作不会新建线程, 任务顺序执行
for (int i = 0; i < 10; ++i) {
dispatch_sync(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
//异步操作会新建线程, 任务顺序执行(非常有用)
for (int i = 0; i < 10; ++i) {
dispatch_async(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
```

#### GCD并行队列

```objectivec
dispatch_queue_t q = dispatch_queue_create("com.name.c", DISPATCH_QUEUE_CONCURRENT);
//同步操作不会新建线程, 任务顺序执行
for (int i = 0; i < 10; ++i) {
dispatch_sync(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
//异步操作会新建多个线程, 任务无序执行
for (int i = 0; i < 10; ++i) {
dispatch_async(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
```
#### GCD全局队列
全局队列与并行队列的区别:
- 不需要创建，直接GET就能用
- 两个队列的执行效果相同
- 全局队列没有名称，调试时，无法确认准确队列

>`强烈建议，在绝大多数情况下使用默认的优先级队列就可以了。`
>如果执行的任务需要访问一些共享的资源，那么在不同优先级的队列中调度这些任务很快就会造成不可预期的行为。这样可能会引起程序的完全挂起，造成`优先级反转`，因为低优先级的任务阻塞了高优先级任务，使它不能被执行。

```objectivec
dispatch_queue_t q =dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
//同步操作不会新建线程, 任务顺序执行
for (int i = 0; i < 10; ++i) {
dispatch_sync(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
//异步操作会新建多个线程, 任务无序执行
for (int i = 0; i < 10; ++i) {
dispatch_async(q, ^{
NSLog(@"%@ %d", [NSThread currentThread], i);
});
}
```

#### GCD主队列
在iOS开发中，所有UI的更新工作，都必须在主线程上执行！
```objectivec
dispatch_queue_t q = dispatch_get_main_queue();
//同步操作会造成死锁,永远不会执行
dispatch_sync(q, ^{
NSLog(@"come here baby!");
});
// 异步操作，在主线程上运行，同时是保持队形的
for (int i = 0; i < 10; ++i) {
dispatch_async(q, ^{
NSLog(@"%@ - %d", [NSThread currentThread], i);
});
}
```

### GCD不同队列种嵌套dispatch_sync的结果
```objectivec
// 全局队列，都在主线程上执行，不会死锁
dispatch_queue_t q = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
// 并行队列，都在主线程上执行，不会死锁
dispatch_queue_t q = dispatch_queue_create("com.name.c", DISPATCH_QUEUE_CONCURRENT);
// 串行队列，会死锁，但是会执行嵌套同步操作之前的代码
dispatch_queue_t q = dispatch_queue_create("com.name.s", DISPATCH_QUEUE_SERIAL);
// 直接死锁
dispatch_queue_t q = dispatch_get_main_queue();

dispatch_sync(q, ^{
NSLog(@"同步任务 %@", [NSThread currentThread]);
dispatch_sync(q, ^{
NSLog(@"同步任务 %@", [NSThread currentThread]);
});
});
```
dispatch_sync的应用场景:
- 阻塞并行队列的执行，要求某一操作执行后再进行后续操作，如用户登录
- 确保块代码之外的局部变量确实被修改

### GCD死锁
这里有篇博客已经介绍得非常详细了,图文并茂, 浅显易懂
[某妹纸博客](http://www.saitjr.com/ios/ios-gcd-deadlock.html)

### GCD队列组（Group queue）
- Group queue 可以通过调用dispatch_group_create()来获取，通过dispatch_group_notify,可以直接监听组里所有线程完成情况。
- 当遇到需要执行多个线程并发执行，然后等多个线程都结束之后，再汇总执行结果时可以用group queue

```objectivec
dispatch_queue_t q = dispatch_get_global_queue(0, 0);
dispatch_queue_t mainQueue = dispatch_get_main_queue();
dispatch_group_t groupQueue = dispatch_group_create();
NSLog(@"current task");
dispatch_group_async(groupQueue, q, ^{
NSLog(@"并行任务1");
});
dispatch_group_async(groupQueue, q, ^{
NSLog(@"并行任务2");
});
dispatch_group_notify(groupQueue, mainQueue, ^{
NSLog(@"groupQueue中的任务 都执行完成,回到主线程更新UI");
});
NSLog(@"next task");
```

### GCD常用系统方法
#### dispatch_once
- 保证在app运行期间，block中的代码只执行一次
- 经典使用场景－－－单例

```objectivec
+ (instancetype)manager {
static id instance = nil;
// 使用GCD
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
// 保证代码执行一次
instance = [[[self class] alloc]init];
});
return instance;
}
```

#### dispatch_after
- 延时添加到队列

```objectivec
dispatch_time_t delayTime3 = dispatch_time(DISPATCH_TIME_NOW, 3*NSEC_PER_SEC);
dispatch_time_t delayTime2 = dispatch_time(DISPATCH_TIME_NOW, 2*NSEC_PER_SEC);
dispatch_queue_t mainQueue = dispatch_get_main_queue();
NSLog(@"current task");
dispatch_after(delayTime3, mainQueue, ^{
NSLog(@"3秒之后添加到队列");
});
dispatch_after(delayTime2, mainQueue, ^{
NSLog(@"2秒之后添加到队列");
});
NSLog(@"next task");
```

#### dispatch_apply
- 在给定的队列上多次执行某一任务
- 是同步执行的函数, 在主线程直接调用会阻塞主线程去执行block中的任务
- 一般把dispatch_apply放在异步队列中调用，然后执行完成后通知主线程

```objectivec
dispatch_queue_t globalQueue = dispatch_get_global_queue(0, 0);
NSLog(@"current task");
dispatch_async(globalQueue, ^{
dispatch_queue_t applyQueue = dispatch_get_global_queue(0, 0);
// 3表示重复次数
dispatch_apply(3, applyQueue, ^(size_t index) {
NSLog(@"current index %@",@(index));
sleep(1);
});
NSLog(@"dispatch_apply 执行完成");
dispatch_queue_t mainQueue = dispatch_get_main_queue();
dispatch_async(mainQueue, ^{
NSLog(@"回到主线程更新UI");
});
});
NSLog(@"next task");
```

#### dispatch_barrier_async
- 栅栏的作用
- 功能：在并行队列中，等待在dispatch_barrier_async之前加入队列的任务全部执行完成之后，再执行dispatch_barrier_async中的任务，再去执行在dispatch_barrier_async之后加入到队列中的任务。

```objectivec
dispatch_queue_t q = dispatch_queue_create("com.name.c", DISPATCH_QUEUE_CONCURRENT);
dispatch_async(q, ^{
NSLog(@"dispatch 1");
});
dispatch_barrier_async(q, ^{
NSLog(@"dispatch barrier");
});
dispatch_async(q, ^{
NSLog(@"dispatch 2");
});

```
