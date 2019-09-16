# GCD使用总结
包含了对gcd中任务，队列，函数以及使用场景。
## 为什么要使用GCD
项目中常常会需要处理到一些耗时操作（比如：绘图，网络请求，大量的图片处理，数据库操作），这样会造成主线程堵塞，影响到UI的刷新速度，给用户造成卡顿的视觉体验。所以这时我们需要将这些耗时操作统统放到子线程中去做。
## 任务介绍
* 同步任务
```
同步任务是指在主线程做的操作。
```
* 异步任务
```
异步任务是指在子线程做的操作。
```
## 队列介绍
*   串行队列
```
dispatch_queue_t queue = dispatch_queue_create("queue.com", DISPATCH_QUEUE_SERIAL);
```
*   并发队列
```
dispatch_queue_t queue = dispatch_queue_create("queue.com", DISPATCH_QUEUE_CONCURRENT);
```
*   全局并发队列（实际上和并发队列没啥区别）
```
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```
*   主队列
```
dispatch_queue_t queue = dispatch_get_main_queue();
```
*   队列组
```
// 创建队列组
dispatch_group_t group = dispatch_group_create();
// 获取全局并发队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

// 异步并发执行，不会堵塞主线程
dispatch_group_async(group, queue, ^{

    // 进入队列组
    dispatch_group_enter(group);

    // 延时操作，拿到数据后离开队列组
    dispatch_group_leave(group);
});

// 拦截通知,当队列组中所有的任务都执行完毕的时候回进入到下面的方法
dispatch_group_notify(group, queue, ^{
    dispatch_async(dispatch_get_main_queue(), ^{
        // do something
    });
});
```
## 队列差异
*   串行队列
```
多个任务abc按顺序执行(先执行a，再执行b)，当任务a执行完毕后才会执行任务b，遵循FIFO原则。
```
```
串行队列开辟一条新线程
```
*   并发队列
```
多个任务abc按顺序执行(先执行a，在执行b)，不会考虑任务a是否执行完毕，而是a执行后直接执行任务b。
```
```
并发队列开辟多条新线程
```
*   队列组
```
队列管理在同一队列的执行顺序是串行的，不同队列是并行的。
```
## 队列使用场景
以下是gcd对一些耗时操作的使用场景：<br>
* 数据库操作时，但是我们需要获取最终的数据库（存储，查询，删除，添加）结果再去执行下一步操作，需要用到异步串行队列。<br>
* 处理图片或者上传图片时，应该在做其他的操作的同时，继续加载或者上传图片，需要用到异步并发队列组。<br>
* 网络请求，在数据没有回调之前，客户端可以响应其他事件，需要用到异步并发队列。<br>

## 函数介绍
* dispatch_barrier_async
```
栅栏函数；队列里面的 前面执行完 才会执行后面的。
```
* dispatch_suspend(queue）
```
函数挂起指定的Dispatch Queue
```
* dispatch_resume(queue）
```
函数恢复指定的dispatch Queue
```
* dispatch_after
```
延迟函数，延时多少秒将任务加入队列中。
```
* dispatch_group
```
多个处理全部请求结束后想要执行结果处理
```
* dispatch_once
```
函数是保证在应用程序执行中只执行一次指定处理的API
```
* Dispatch Semaphore
```
信号量，可以设置线程数量的函数。
```
* dispatch_apply
```
该函数是dispatch_sync函数和Dispatch Group的关联API。按照指定的次数将指定的Block追加到指定的Dispatch Queue中，并等待全部处理执行结束。
```
