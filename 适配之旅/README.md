# @ `kill`

13年中旬刚入移动端坑的时候以为天下太平,从此兼容是路人;   
没想到啊,没想到,从一个坑跳到了另一个坑;

## @ 入门 (百分比`%`)

当初移动端网页还被称为`wap`;   
入门的第一个`ui`库就是`jquery-mobile`,因为`jQuery`的影响力`jquery-mobile`也被大家关注;   
上手快是理所当然,在很短的时间内就搭出来一个小型的网站;   
刚开始理解的适配就是百分比`%`来设置宽高,然后用媒体查询器`@media`进行像素适配;   

## @ 新的单位`rem`

### `px`

通常我们使用`px`作为文本单文,可以比较准确的设置大小和定位;

### `em`

如果浏览器放大或者缩小时`px`就不尽人意了;`em`就映入眼帘,`em`是通过父级元素字体大小来进行换算的,`1 ÷ 父元素的font-size × 需要转换的像素值 = em值`;但是他的问题就出现在`em`受所有父级元素字体大小的影响;层级深了,对样式表没有规划好很容易造成不是预计的大小;

### `rem`

`css3`带来了新的单位 `font size of the root element` (略显得是`em`的升级版),`rem`是相对于`html`根元素的字体大小,在一个相对值换算下就能保证预算值的正确性;

## @ 设置`html`的`font-size`

```javascript
// 早期设置html font-size 值的js
// iPhone 4 风靡的时代 640px是一个阙值
!(function(win, doc){
    var docEl = doc.documentElement,
    recalc = function () {
        var clientWidth = docEl.clientWidth;
        if (!clientWidth) return;
        var rem = Math.max(10,Math.min(20,(20 * (clientWidth / 640))));
        docEl.style.fontSize = rem + 'px';
    };
    var timer = null;
    recalc();
    win.addEventListener('DOMContentLoaded', function () {
        clearTimeout(timer);
        timer = setTimeout(recalc, 0);
    }, false);
    win.addEventListener('resize', function () {
        clearTimeout(timer);
        timer = setTimeout(recalc, 0);
    }, false);
}(window, document));
```
有了根元素的设置,那剩下的就是以`640px`为基准的设计稿,用计算器换算成`rem`;   
一切都显得那么容易,那么理所当然;   
什么?需求变了?设计稿不满意?要调整?WTF!   
好吧,在小心翼翼的调整吧;

```scss
// 就一个难题还能急死个人
// 还好有gulp 有sass

@mixin remCalc($property, $values...) {
    $max: length($values);//返回$values列表的长度值
    $pxValues: '';
    $remValues: '';
    @for $i from 1 through $max {
        $value: strip-units(nth($values, $i));//返回$values列表中的第$i个值，并将单位值去掉
        $browser-default-font-size: strip-units($browser-default-font-size);
        $pxValues: #{$pxValues + $value * $browser-default-font-size}px;
        @if $i < $max {
            $pxValues: #{$pxValues + " "};
        }
    }
    @for $i from 1 through $max {
        $value: strip-units(nth($values, $i));
        $remValues: #{$remValues + $value}rem;
        @if $i < $max {
            $remValues: #{$remValues + " "};
        }
    }
    #{$property}: $pxValues;
    #{$property}: $remValues;
}
```

## @ 另一种更粗狂的适配

无论多宽都已`640px`为基准缩放显示;(这个是到处看M站点,在网易M站抄到的)
```html
<meta content="target-densitydpi=device-dpi,width=640,user-scalable=no" name="viewport">
```

## @ `flexible`方案

手淘出品,经受了双十一的考验
```html
// 引入插件
<script src="http://g.tbcdn.cn/mtb/lib-flexible/{{version}}/??flexible.js"></script>
```
```css
// 配合使用 postcss px2rem autoprefixer

.test{
    font-size: 28px;  /*px*/ 转换成像素
    border: 1px solid #ccc; /*no*/ 使插件忽略
}
```
```css
// 处理过后的代码

.test {
    border: 1px solid #ddd;
}
[data-dpr="1"] .test {
    font-size: 14px;
}
[data-dpr="2"] .test {
    font-size: 28px;
}
[data-dpr="3"] .test {
    font-size: 42px;
}

```

配置好工作流,全自动,完全担心换算错误;

## @ `vw` 和 `vh`

- `vw`: 是 `Viewport's width` 的简写, `1vw` 等于 `window.innerWidth` 的 `1%``
- `vh`: 和 `vw` 类似,是`Viewport's height` 的简写, `1vh` 等于 `window.innerHeihgt` 的 `1%`
- `vmin`: `vmin` 的值是当前 `vw` 和 `vh` 中较小的值
- `vmax`: `vmax` 的值是当前 `vw` 和 `vh` 中较大的值

这个单位还比较新只在某些样式中附带使用,膜拜大厂尝鲜中;

## @参考

<a href="https://github.com/jquery/jquery-mobile" target="_blank">jquery-mobile</a>

<a href="https://www.w3cplus.com/preprocessor/sass-px-to-rem-with-mixin-and-function.html" target="_blank">Sass基础——Rem与Px的转换</a>


<a href="http://c.m.163.com/CreditMarket/default.html" target="_blank">网易新闻客户端金币商城</a>

<a href="https://github.com/amfe/article/issues/17" target="_blank">使用Flexible实现手淘H5页面的终端适配 #17</a>

<a href="https://www.w3cplus.com/css/vw-for-layout.html" target="_blank">再聊移动端页面的适配</a>
