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

![](/images/tasks-01-03-001.jpg)

### 任务队列<2>

![](/images/tasks-01-03-002.jpg)

### 任务队列<3>

```
setTimeout callbacks are queued as tasks
```

### 任务队列<4>

![](/images/tasks-01-03-003.jpg)

### 任务队列<5>

Promise callbacks are queued as microtasks

### 任务队列<6>


![](/images/tasks-01-03-004.jpg)

### 任务队列<7>


![](/images/tasks-01-03-005.jpg)

### 任务队列<8>


![](/images/tasks-01-03-006.jpg)

### 任务队列<9>

```
At the end of a task, we process microtasks
```

### 任务队列<10>

![](/images/tasks-01-03-007.jpg)  

### 任务队列<11>


![](/images/tasks-01-03-008.jpg)  

### 任务队列<12>


![](/images/tasks-01-03-009.jpg)

### 任务队列<13>

```
This promise callback returns 'undefined', which queues the next promise callback as a microtask
```

### 任务队列<14>


![](/images/tasks-01-03-0010.jpg)

### 任务队列<15>

```
This microtask is done so we move onto the next one in the queue
```


![](/images/tasks-01-03-0011.jpg)


### 任务队列<16>

![](/images/tasks-01-03-0012.jpg)

### 任务队列<17>


![](/images/tasks-01-03-0013.jpg)

### 任务队列<18>


![](/images/tasks-01-03-0014.jpg)

### 任务队列<19>


![](/images/tasks-01-03-0015.jpg)

### 任务队列<20>

```
And that's this task done! The browser may update rendering
```

### 任务队列<21>


![](/images/tasks-01-03-0016.jpg)

### 任务队列<22>


![](/images/tasks-01-03-0017.jpg)

### 任务队列<23>


![](/images/tasks-01-03-0018.jpg)

### 任务队列<24>


![](/images/tasks-01-03-0019.jpg)

### 任务队列<25>


![](/images/tasks-01-03-0020.jpg)


## @ 参考

[tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
