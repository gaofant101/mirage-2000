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

### 队列<1>

`Tasks`: [`run script`]   
`Microtasks`:    
`JSStack`: [`script`]   
`Log`:   


### 队列<2>

`Tasks`: [`run script`]   
`Microtasks`:    
`JSStack`: [`script`]   
`Log`: [`script start`]

### 队列<3>

```
setTimeout callbacks are queued as tasks
```

### 队列<4>

`Tasks`: [`run script`], [`setTimeout callback`]    
`Microtasks`:    
`JSStack`: [`script`]   
`Log`: [`script start`]

### 队列<5>

Promise callbacks are queued as microtasks

### 队列<6>

`Tasks`: [`run script`], [`setTimeout callback`]    
`Microtasks`: [`Promise then`]    
`JSStack`: [`script`]     
`Log`: [`script start`]    

### 队列<7>

`Tasks`: [`run script`], [`setTimeout callback`]    
`Microtasks`: [`Promise then`]   
`JSStack`: [`script`]    
`Log`: [`script start`], [`script end`]   

### 队列<8>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`:    
`Log`: [`script start`], [`script end`]  

### 队列<9>

```
At the end of a task, we process microtasks
```

### 队列<10>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`] // 准备执行   
`JSStack`:      
`Log`: [`script start`], [`script end`]     

### 队列<11>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`: [`Promise callback`]   
`Log`: [`script start`], [`script end`]     

### 队列<12>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`: [`Promise callback`]   
`Log`: [`script start`], [`script end`], [`promise1`]

### 队列<13>

```
This promise callback returns 'undefined', which queues the next promise callback as a microtask
```

### 队列<14>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`], [`Promise then`]   
`JSStack`: [`Promise callback`]   
`Log`: [`script start`], [`script end`], [`promise1`]

### 队列<15>

```
This microtask is done so we move onto the next one in the queue
```

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`], [`Promise then`]   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`]   


### 队列<16>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`]

### 队列<17>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`: [`Promise callback`]   
`Log`: [`script start`], [`script end`], [`promise1`]

### 队列<18>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`: [`Promise then`]   
`JSStack`: [`Promise callback`]   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`]   

### 队列<19>

`Tasks`: [`run script`], [`setTimeout callback`]   
`Microtasks`:   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`]   

### 队列<20>

```
And that's this task done! The browser may update rendering
```

### 队列<21>

`Tasks`: [`setTimeout callback`]   
`Microtasks`:   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`]   

### 队列<22>

`Tasks`: [`setTimeout callback`]   
`Microtasks`:   
`JSStack`: [`setTimeout callback`]   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`]   

### 队列<23>

`Tasks`: [`setTimeout callback`]   
`Microtasks`:   
`JSStack`: [`setTimeout callback`]   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`],[`setTimeout`]

### 队列<24>

`Tasks`: [`setTimeout callback`]   
`Microtasks`:   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`],[`setTimeout`]

### 队列<25>

`Tasks`:   
`Microtasks`:   
`JSStack`:   
`Log`: [`script start`], [`script end`], [`promise1`], [`promise2`],[`setTimeout`]

### 队列<26>

`Finsh`

```
Log: [`script start`], [`script end`], [`promise1`], [`promise2`],[`setTimeout`]
```

## @ 参考

[tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
