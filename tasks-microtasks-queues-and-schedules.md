# @ `tasks-microtasks-queues-and-schedules`

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

### 任务队列<1>

开始运行 `javascript`, `js`程添加 中添加 `script`;

![](/images/tasks-01-03-001.jpg)

### 任务队列<2>

打印第一行 `log`;

![](/images/tasks-01-03-002.jpg)

### 任务队列<3>

运行到第三行,遇到 `settimeout` 异步任务;

```
setTimeout callbacks are queued as tasks
```

### 任务队列<4>

`tasks`中添加 `settimeout callback`;

![](/images/tasks-01-03-003.jpg)

### 任务队列<5>

运行到第七行,遇到 `promise` ;

```
Promise callbacks are queued as microtasks
```

### 任务队列<6>

`microtasks` 添加 `promise then`

![](/images/tasks-01-03-004.jpg)

### 任务队列<7>

运行到最后一行, 打印 `log`;

![](/images/tasks-01-03-005.jpg)

### 任务队列<8>

到这里`js`线程的任务执行完毕;

![](/images/tasks-01-03-006.jpg)

### 任务队列<9>

```
At the end of a task, we process microtasks
```

### 任务队列<10>

当`js`线程空闲的时候,轮询 `microtasks`;

![](/images/tasks-01-03-007.jpg)  

### 任务队列<11>

`js`线程插入 `promise callback`;

![](/images/tasks-01-03-008.jpg)  

### 任务队列<12>


打印 `promise1`;

![](/images/tasks-01-03-009.jpg)

### 任务队列<13>

`promise`任务流继续往下执行, `then`;

```
This promise callback returns 'undefined', which queues the next promise callback as a microtask
```

### 任务队列<14>

`microtasks` 添加下一个 `promise then`;

![](/images/tasks-01-03-0010.jpg)

### 任务队列<15>

```
This microtask is done so we move onto the next one in the queue
```

上一个 `promise callback` 执行完毕, `js`清除任务, 状态为空闲; `microtasks` 去除头队列;

![](/images/tasks-01-03-0011.jpg)


### 任务队列<16>

主线程继续轮询, 看到 `microtasks`中有任务等待, 准备执行 `microtasks`中头队列任务;

![](/images/tasks-01-03-0012.jpg)

### 任务队列<17>

`js`线程空闲状态下,继续添加 `microtasks`;

![](/images/tasks-01-03-0013.jpg)

### 任务队列<18>

执行`promise callback`; 打印 `promise2`;

![](/images/tasks-01-03-0014.jpg)

### 任务队列<19>

`promise callb`执行完毕,`js`线程清除任务,`microtasks` 中 `promise then` 任务清除;

![](/images/tasks-01-03-0015.jpg)

### 任务队列<20>

```
And that's this task done! The browser may update rendering
```

### 任务队列<21>

`js`线程空闲, 继续轮询, `microtasks` 为空, 轮询 `tasks`; 有任务等待;

![](/images/tasks-01-03-0016.jpg)

### 任务队列<22>

`js`线程中添加 `settimeout callback`;

![](/images/tasks-01-03-0017.jpg)

### 任务队列<23>

打印 `settimeout`;

![](/images/tasks-01-03-0018.jpg)

### 任务队列<24>

任务执行完毕, `js`线程清除任务;

![](/images/tasks-01-03-0019.jpg)

### 任务队列<25>

清除 `tasks`, 继续轮询, `js`线程没有任务, 轮询 `microtasks`, 没有任务, 轮询 `tasks`, 没有任务, `js`线程空闲等待...

![](/images/tasks-01-03-0020.jpg)


## @ 参考

[tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
