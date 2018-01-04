# @ 并发模型与事件循环

### 可视化描述

![](https://developer.mozilla.org/files/4617/default.svg)

`javascript` 运行时, 主线程产生堆(`heap`)、 栈(`stack`)、消息队列(`queue`);

- 堆(`heap`): 对象被分配在一个堆中, 即用以表示一个大部分非结构化的内存区域;
- 栈(`stack`): 函数调用形成一个栈帧;
- 队列(`queue`): 一个 `javascript` 运行时包含了一个待处理的消息队列. 每一个消息都与一个函数相关联. 当栈拥有足够内存时, 从队列中取出一个消息进行处理. 这个处理过程包含了调用与这个消息相关联的函数(以及因而创建了一个初始堆栈帧). 当栈再次为空的时候, 也就意味着消息处理结束;

### 体现

[`task-queue`](https://html.spec.whatwg.org/multipage/webappapis.html#task-queue)

`macrotasks`: `setTimeout` `setInterval` `setImmediate` `I/O` `UI rendering`   
`microtasks`: `Promise` `process.nextTick` `Object.observe` `MutationObserver`

## @ 例子

我们直接用例子来看看 `tasks` 运行流程, 默记一下打印的内容顺序是什么.
```javascript
console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
    console.log('promise1');
}).then(function() {
    console.log('promise2');
});

console.log('script end');
```

### 过程

1. 开始运行 `javascript`, 引擎主线程运行, 产生栈(`stack`);

2. 从上往下执行同步代码, `lgo(script start)` 压入执行栈(`stack`), `log` 是浏览器内方法不属于`webapi`, 立即出栈(`stack`)执行, 打印`script start`;

3. 往下执行, 遇到 `settimeout` 异步任务, 压入执行栈(`stack`), 查看任务类型, 分配到`tasks queue`;

4. 往下执行, 遇到 `promise` 异步任务, 压入执行栈(`stack`), 查看任务类型, 分配到 `microtasks queue`;

5. 往下执行, 同步骤 (2), 压入执行栈(`stack`), 立即出栈(`stack`)执行, 打印`script end`;

6. 这个时候`task queue`空闲, 开始轮询(`even loop`) 查看栈(`stack`)积情况, `task` 执行过 `log`; 开始提取 `microtasks queue`;

7. 执行` promise then`, `promise callback` 压入栈(`stack`), 出栈执行`promise callback` , 打印 `promise1`, `promise callback returns undefined`;

8. 继续执行 `promise then` 又一个异步任务, 添加到 `microtasks queue`;

9. 第一个`microtask` 执行完毕, 执行下一个 `microtask`;

10. 同步骤(7), `promise callback` 压入栈(`stack`), 出栈执行 `promise callback` , 打印 `promise2` ;

11. 到这里 `microtask` 全部执行完毕, 浏览器可能 `update rendering`;

12. 继续轮询`even loop`, 查看栈(`stack`)积情况, 调取`task`中 `setTimeout callback`出栈执行; 打印 `setTimeout`;

13. 到此所有的`task`执行完毕; 任务列队处于空闲状态;

> 结果以`chrome`为基准; 在`firefox`、`safari`、`windows browser`中可能不一样; 差异化可以查看[Jake Archibald blog](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

### `Even Loop`

![](https://cdn.int64ago.org/6p10znqn.png)

### 过程图解

### `Queue<1>`

![](/images/tasks-01-03-001.jpg)

### `Queue<2>`

![](/images/tasks-01-03-002.jpg)

### `Queue<3>`

![](/images/tasks-01-03-003.jpg)

### `Queue<4>`

![](/images/tasks-01-03-004.jpg)

### `Queue<5>`

![](/images/tasks-01-03-005.jpg)

### `Queue<6>`

![](/images/tasks-01-03-006.jpg)


### `Queue<7>`

![](/images/tasks-01-03-007.jpg)  

### `Queue<8>`

![](/images/tasks-01-03-008.jpg)  

### `Queue<9>`

![](/images/tasks-01-03-009.jpg)


### `Queue<10>`

![](/images/tasks-01-03-0010.jpg)

### `Queue<11>`

![](/images/tasks-01-03-0011.jpg)


### `Queue<12>`

![](/images/tasks-01-03-0012.jpg)

### `Queue<13>`

![](/images/tasks-01-03-0013.jpg)

### `Queue<14>`

![](/images/tasks-01-03-0014.jpg)

### `Queue<15>`
![](/images/tasks-01-03-0015.jpg)

### `Queue<16>`

![](/images/tasks-01-03-0016.jpg)

### `Queue<17>`

![](/images/tasks-01-03-0017.jpg)

### `Queue<18>`

![](/images/tasks-01-03-0018.jpg)

### `Queue<19>`

![](/images/tasks-01-03-0019.jpg)

### `Queue<20>`

![](/images/tasks-01-03-0020.jpg)


## @ 参考

[并发模型与事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)

[tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

[event-loop-timers-and-nexttick](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
