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
```java
// step1

Tasks: [run script]   
Microtasks:    
JSStack: [script]   
Log:   
```
```java
// step2

Tasks: [run script]   
Microtasks:    
JSStack: [script]   
Log: [script start]
```
```java
// step3   

setTimeout callbacks are queued as tasks
```
```java
// step4

Tasks: [run script], [setTimeout callback]
Microtasks:    
JSStack: [script]   
Log: [script start]
```
```java
// step5

Promise callbacks are queued as microtasks
```
```java
// step6

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [script]   
Log: [script start]
```
```java
// step7

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [script]   
Log: [script start], [script end]
```
```java
// step8

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack:    
Log: [script start], [script end]  
```
```java
// step9

At the end of a task, we process microtasks
```
```java
// step10

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then] // 准备执行
JSStack:    
Log: [script start], [script end]  
```
```java
// step11

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [Promise callback]
Log: [script start], [script end]  
```
```java
// step12

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [Promise callback]
Log: [script start], [script end], [promise1]
```
```java
// step13

This promise callback returns 'undefined', which queues the next promise callback as a microtask
```
```java
// step14

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then], [Promise then]
JSStack: [Promise callback]
Log: [script start], [script end], [promise1]
```
```java
// step15

This microtask is done so we move onto the next one in the queue

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then], [Promise then]
JSStack:
Log: [script start], [script end], [promise1]
```
```java
// step16

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack:
Log: [script start], [script end], [promise1]
```
```java
// step17

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [Promise callback]
Log: [script start], [script end], [promise1]
```
```java
// step18

Tasks: [run script], [setTimeout callback]
Microtasks: [Promise then]
JSStack: [Promise callback]
Log: [script start], [script end], [promise1], [promise2]
```
```java
// step19

Tasks: [run script], [setTimeout callback]
Microtasks:
JSStack:
Log: [script start], [script end], [promise1], [promise2]
```
```java
// step20

And that's this task done! The browser may update rendering
```
```java
// step21

Tasks: [setTimeout callback]
Microtasks:
JSStack:
Log: [script start], [script end], [promise1], [promise2]
```
```java
// step22

Tasks: [setTimeout callback]
Microtasks:
JSStack: [setTimeout callback]
Log: [script start], [script end], [promise1], [promise2]
```
```java
// step23

Tasks: [setTimeout callback]
Microtasks:
JSStack: [setTimeout callback]
Log: [script start], [script end], [promise1], [promise2],[setTimeout]
```
```java
// step24

Tasks: [setTimeout callback]
Microtasks:
JSStack:
Log: [script start], [script end], [promise1], [promise2],[setTimeout]
```
```java
// step25

Tasks:
Microtasks:
JSStack:
Log: [script start], [script end], [promise1], [promise2],[setTimeout]
```
```java
// step26

Finsh

Log: [script start], [script end], [promise1], [promise2],[setTimeout]
```

## @ 参考

[tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver)
