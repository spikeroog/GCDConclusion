# GCD使用总结
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
