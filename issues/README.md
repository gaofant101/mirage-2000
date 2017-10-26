## @各种坑

### `input`的 `compositionstart` 和 `compositionend`

想象一个场景,用户输入内容,我们要对用户输入的内容进行检查限制;   
理想状态是用户输入完内容过后才对输入的内容进行判断,但是`iOS`中会出现用户触摸虚拟键盘的每一个按钮都会触发检查机制,这显然不是我们想要的;   
这个时候就可以通过`compositionstart` 和 `compositionend`来做开关了

```javascript
var power = false;
function do(inputElement) {
    var regex = /[^1-9a-zA-Z]/g;
    inputElement.value = inputElement.value.replace(regex, '');
}

inputElement.addEventListener('compositionstart', function() {
    power = true;
});


inputElement.addEventListener('compositionend', function(event) {
    power = false;
    do(event.target);
})


inputElement.addEventListener('input', function(event) {
    if (!power) {
        do(event.target);
        event.returnValue = false;
    }
});

```
