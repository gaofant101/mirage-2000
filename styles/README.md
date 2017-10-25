## @issues

### `CSS3`动画性能

- `CSS3`动画的定位设置,会触发页面的重排`relayout` 重绘`repaint` 重组`recomposite`
- `paint` 是最花费性能的, `CSS3`中`transform`只会触发`composite`, 而通过定位实现的动画(`left right top bottom`)会额外的触发`layout` `paint`
- 使用`CSS3`动画时, 推荐使用`position: absolute;` 让元素脱离文档流
- 在移动端因为设备的原因,动画会出现卡顿和闪烁(`3D` 变形会消耗更多的内存和耗电)
- 启用硬件加速`transform: translate3d(0, 0, 0);`
- 解决动画闪烁的 `hack` `backface-visibility: hidden; perspective: 1000;`
- `box-shadows` 和 `gradients` 性能杀手,尽可能少用
-




## 你想要的

<a href="http://browserhacks.com/" target="_blank">各种hack -> BROWSERHACKS</a>
