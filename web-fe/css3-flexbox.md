---
title: CSS3 Flexbox 知识点整理
date: '2017-09-12T15:45:04.000Z'
tags: null
---

# css3-flexbox

这段时间经常用到 Flexbox 弹性盒模型，十分简洁好用，不需要 float, position 等属性即可完成复杂的页面布局。若无需兼容 IE 或只在移动端开发，推荐使用 Flex 布局。

## Flexbox 布局

Flexbox 是一种布置元素的布局模式，适应不同的屏幕尺寸，弹性盒子模型是对块模型的改进，flex容器的边缘也不会与其内容的边缘折叠。

Flex 容器默认存在两条轴，分别是水平主轴\(main axis\) 和 垂直的交叉轴\(或侧轴\)\(cross axis\)，可以通过修改 `flex-direction` 来改变主轴和交叉轴。

Flex 容器\(Flex container\): 包含着弹性项目的父元素。通过设置 `display: flex | inline-flex` 定义弹性容器。

```css
.container {
    display: flex | inline-flex;
}
```

## Flex 容器的 6 个属性

1. `flex-direction` 确立主轴。

   ```css
   .container {
       flex-direction: row | row-reverse | column | column-reverse; // 默认值 row
   }
   ```

   row/row-reverse: 水平方向从左到右/从右到左 column/column-reverse: 垂直方向从上到下/从下到上

2. `flex-wrap` 控制侧轴的方向和新行排列的方向，即弹性项目单行、多行排布。

   ```css
   .container {
       flex-wrap: nowrap | wrap | wrap-reverse; // 默认值 nowrap
   }
   ```

   nowrap\(默认值\): 不换行 wrap/wrap-reverse: 项目主轴总尺寸超出容器时换行，第一行在上方/换行，第一行在下方

3. `flex-flow` 是 `flex-direction` 和 `flex-wrap` 的简写，决定弹性项目如何排布。

   ```css
   .container {
       flex-flow: <flex-direction> || <flex-wrap>; // 默认值 row nowrap
   }
   ```

4. `justify-content` 定义弹性项目沿主轴如何排布。

   ```css
   .container {
       justify-content: flex-start | flex-end | center | space-between | space-around; // 默认值 flex-start
   }
   ```

   flex-start\(默认值\): 向主轴起始位置靠齐; flex-end: 向主轴结束位置靠齐; center: 向主轴中间位置靠齐; space-between: 两端对齐，项目之间等距; space-around: 每个项目两侧的间隔相等，项目之间的间隔比项目与边缘的间隔大一倍.

5. `align-items` 定义弹性项目沿侧轴如何排布。

   ```css
   .container {
       align-items: flex-start | flex-end | center | baseline | stretch; // 默认值 stretch
   }
   ```

   stretch\(默认值\): 项目未设置高\(宽\)度或者设为 auto，将在侧轴拉伸填充整个容器; flex-start: 向侧轴起始位置靠齐; flex-end: 向侧轴结束位置靠齐; center: 向侧轴中间位置靠齐; baseline: 根据项目的基线对齐.

6. `align-content` 定义多个侧轴的空间分配方式，只有一根侧轴不起作用。

   ```css
   .container {
       align-content: flex-start | flex-end | center | space-between | space-around | stretch; // 默认值 stretch
   }
   ```

   stretch\(默认值\): 项目未设置高\(宽\)度或者设为 auto，将在侧轴拉伸填充整个容器; flex-start: 向侧轴起始位置靠齐; flex-end: 向侧轴结束位置靠齐; center: 向侧轴中间位置靠齐; space-between: 两端对齐，项目之间等距; space-around: 每个项目两侧的间隔相等，项目之间的间隔比项目与边缘的间隔大一倍.

## Flex 项目的 6 个属性

1. `order` 规定了弹性容器中的可伸缩项目在布局时的顺序，数值越小，排列越靠前，默认值为 0。

   ```css
   .item {
       order: <integer>; // 默认值 0
   }
   ```

2. `flex-basis` 伸缩基础，在进行计算剩余空间或超出空间前，给伸缩项目重新设置一个宽度，然后再计算。

   ```css
   .item {
       flex-basis: <length> | auto; // 默认值 auto
   }
   ```

3. `flex-grow` 当伸缩项目在主轴方向的总宽度 &lt; 伸缩容器，伸缩项目根据**扩展因素**分配伸缩容器的剩余空间。

   ```css
   .item {
       flex-grow: <number>; // 默认值 0，即存在剩余空间不放大
   }
   ```

4. `flex-shrink` 当伸缩项目在主轴方向的总宽度 &gt; 伸缩容器，伸缩项目根据**缩小因素**分配总宽度超出伸缩容器的空间。

   ```css
   .item {
       flex-shrink: <number>; // 默认值 1，即如果空间不足，该项目将缩小，负值对该属性无效。
   }
   ```

5. `flex` flex-grow, flex-shrink 和 flex-basis 的简写

   ```css
   .item {
       flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
   }
   ```

   initial\(默认值\): `flex: 0 1 auto` 即 flex-grow: 0; flex-shrink: 1; flex-basis: auto; none: `flex: 0 0 auto` 元素会根据自身宽高来设置尺寸。它是完全非弹性的：既不会缩短，也不会伸长来适应 flex 容器; auto: `flex: 1 1 auto`; `<number>`: `flex: number 1 0%`; `<width>`: `flex: 1 1 width` 例如 `flex: 0% => flex: 1 1 0%`, `flex: 10px => flex: 1 1 10px`.

   **拉伸后弹性项目宽度计算**

   > 弹性项目扩展宽度 = \(容器宽度 - 项目宽度或项目设置的 flex-basis 总和\) \* 对应的 flex-grow 比例 拉伸后弹性项目宽度 = 原弹性项目宽度 + 扩展宽度

   **压缩后弹性项目宽度计算**

   > 弹性项目缩小宽度 = \(项目宽度或项目设置的 flex-basis 总和 - 容器宽度\)  _\*\(对应的 flex-shrink 项目宽度或项目设置的 flex-basis 加权比例\)_ 压缩后弹性项目宽度 = 原弹性项目宽度 - 缩小宽度

   **注意：使用缩写属性会留下一些陷阱，导致表现的结果不尽如人意。**

   > 例如 flex: 1 与 flex-grow: 1 flex: 1 =&gt; flex: 1 1 0% flex-grow: 1 =&gt; flex: 1 1 auto 两者的区别在于 flex-basis 的值不同。flex: 1 为项目宽度重新设置了宽度为0，所以可分配空间为整个容器。

6. `align-self` 定义单个弹性项目在侧轴上应当如何对齐，这个定义会覆盖由 align-items 所确立的默认值。

   ```css
   .item {
        align-self: auto | flex-start | flex-end | center | baseline | stretch;
   }
   ```

## 需要注意的 Flexbox 特性 / Bug

1. 无效属性
   * `float` 和 `clear` 在弹性项目无效
   * `vertical-align` 在弹性项目无效
   * `column-*` 在弹性容器无效
   * `::first-line` 和 `::first-letter` 在弹性容器无效
2. 弹性容器中的 非空字符文本节点 也是弹性项目
3. `margin` 折叠
   * 弹性容器和弹性项目的 `margin` 不会折叠
   * 弹性项目间的 `margin` 不会折叠
4. 旧版 Flexbox 的 BUG

   弹性项目为行内元素要加 `display: block` 或 `display: flex` 属性

## 参考文献

[CSS 弹性盒子布局 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout) [30 分钟学会 Flex 布局 - 林东洲](https://zhuanlan.zhihu.com/p/25303493) [Flexbox布局的正确使用姿势 - Leechikit](https://segmentfault.com/a/1190000009932882)

