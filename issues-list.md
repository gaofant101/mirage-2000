# @ 各种坑

## @用`vue`写的web应用在微信中分享的小问题

前端自理路由,会出现页面切换微信分享失败的问题;   
发现`iOS`分享正常,`Android`部分机型正常部分机型不正常(相同机型不同系统也存在差异);   
经过大量真机测试,发现`iOS`主需要签名一次整个webapp都能正常分享,那么我们先把`iOS`正常的状态给保证好;   
`Android`中测试,每次跳转都重新去生成签名可以解决大部分机型分享的问题,会存在同一种机型但是不同系统出现分享不正常的问题,这个目前还没有想到好的解决办法;   
解决思路:   
##### 这段代码使用在2016年,不知道过了这么久微信或者`vue`有没有处理好这种问题;
```javascript
// vue2.1 vue-router2.1.1
// route.js
const router = new Router({
    mode: 'history',
    base: __dirname,
    ...
});

// 每次切换路由,都调用签名
// 定义一个开关
let wxhttpStatus = false
router.afterEach(route => {
    wxoauth(wxhttpStatus)
    if (!/Android/i.test(navigator.userAgent) && !wxhttpStatus) {
        wxhttpStatus = true
    }
})

// wxoauth.js
// 如果开关为true时 不去签名
export default function (wxhttpStatus) {
    if (wxhttpStatus) return
    Promise.resolve()
    .then(() => window.location.href)
    .then((res) => {
        return axios.get('link?url' + encodeURIComponent(res.split('#')[0]))
    }).then(function (response) {
        if (response.object !== null) {
            wx.config({
                ...
            })
        }
    })
}
```

## @ `iOS`中 `fixed`之坑

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>fixed</title>
    <style type="text/css">
        input{
            position: fixed;
            z-index: 999;
            bottom: 0;
            left: 0;
        }
    </style>
</head>

<body>
    <input type="text" name="" value="">
</body>

</html>

```

唤起虚拟键盘时`input`会错乱问题;   
![fixed之坑](../images/IMG_0754.JPG)   
(手滑,请忽略字丑)    
面对这个问题,目前是通过`css` || `js` 改变样式来解决

### `js` 的解决方案

``` JavaScript
// 改变定位状态
function fixedWatch(el) {
    if(document.activeElement.nodeName == 'INPUT'){
        el.css('position', 'static');
    } else {
        el.css('position', 'fixed');
    }
}
// iscroll.js 目前并不推荐这个, 性能太差劲
```

#### `css` 的解决方案
```css
// 1.用新属性
input{
    position: sticky;
    z-index: 999;
    bottom: 0;
    left: 0;
}

// 2.参考 twitter 的 ratchet库
// 原理是模拟一个body容器出来
#content{
    position: absolute;
    z-index: 1;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    width: 100%;
    height: 100%;
    max-width: 10rem;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
}
```


## @ `input`的 `compositionstart` 和 `compositionend`

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

## @ 参考

[twbs/ratchet](https://github.com/twbs/ratche)

[移动web问题小结](http://www.alloyteam.com/2015/06/yi-dong-web-wen-ti-xiao-jie/)
