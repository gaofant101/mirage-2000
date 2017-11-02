# @ issues

## @ `CSS3`动画性能

- `CSS3`动画的定位设置,会触发页面的重排`relayout` 重绘`repaint` 重组`recomposite`
- `paint` 是最花费性能的, `CSS3`中`transform`只会触发`composite`, 而通过定位实现的动画(`left right top bottom`)会额外的触发`layout` `paint`
- 使用`CSS3`动画时, 推荐使用`position: absolute;` 让元素脱离文档流
- 在移动端因为设备的原因,动画会出现卡顿和闪烁(`3D` 变形会消耗更多的内存和耗电)
- 启用硬件加速`transform: translate3d(0, 0, 0);`
- 解决动画闪烁的 `hack` `backface-visibility: hidden; perspective: 1000;`
- `box-shadows` 和 `gradients` 性能杀手,尽可能少用


## @ `1px border`的实现

在`Retina`屏幕中 `1px`会显示成2个物理像素,目前有2个解决方案;

```scss
// 伪类+transform
.border{
    position: relative;
    margin-bottom:20px;
}
.border:after{
    content: '';
    display:block;
    position: absolute;
    width: 100%;
    left: 0px;
    bottom: 0px;
    height: 1px;
    background-color: #ccc;
    -webkit-transform: scaleY(0.5);
    transform: scaleY(0.5);
}

// 线性渐变
.border2{
    background: -webkit-gradient(linear, left top, left bottom, color-stop(.5, transparent), color-stop(.5, #ccc), to(#ccc)) left bottom repeat-x;
    background-size: 100% 1px;
}
```

## @ 全局样式

- `-webkit-tap-highlight-color: rgba(0, 0, 0, 0); -webkit-tap-highlight-color: transparent;` 取消默认选中背景颜色
- `user-select: none;` 用户不能选择
- `-webkit-text-size-adjust: 100%` 禁止文字缩放
- `<input type="text" autocomplete="off" name="sometext" />` 禁止浏览器自动填充

## @ 你想要的

<a href="http://browserhacks.com/" target="_blank">各种hack -> BROWSERHACKS</a>
