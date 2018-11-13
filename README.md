# GCD使用总结
包含了对gcd中队列，函数以及使用场景的理解。
## 为什么要使用GCD
项目中常常会需要处理到一些耗时操作（比如：绘图，网络请求，大量的图片处理，大量的数据存储），这样会造成主线程堵塞，影响到UI的刷新速度，给用户造成卡顿的视觉体验。所以这时我们需要将这些耗时操作统统放到子线程中去做。
## 队列介绍
*   串行队列
```
dispatch_queue_t queue = dispatch_queue_create("queue.com", DISPATCH_QUEUE_SERIAL);
```
*   并发队列
```
dispatch_queue_t queue = dispatch_queue_create("queue.com", DISPATCH_QUEUE_CONCURRENT);
```
*   全局并发队列
```
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
```
*   主队列
```
dispatch_queue_t queue = dispatch_get_main_queue();
```
## 队列差异
*   串行队列
```
多个任务abc按顺序执行，任务a执行完毕后才会执行任务b，遵循FIFO原则。
```
*   并发队列
```
多个任务abc按顺序执行(先执行a，在执行b)，但是不会考虑任务a是否执行完毕，而是a执行后直接执行任务b。
```
## 队列使用场景
一般情况下使用gcd用的最多的就是异步并发操作，
