---
title: CSS 两栏布局方法总结
date: 2017-12-02 11:59:56
tags: [CSS, 布局, Flexbox]
---
CSS 布局是前端开发的一个重要内容，在 CSS2.1 之前没有出现真正意义上的布局属性，直到 CSS3 才出现了如 Flex、Grid 等布局属性。本文总结了目前主流的两栏布局方法。

> 注意：本文中涉及 float 的布局均未对父元素进行清除浮动，在实际使用中需要在父容器清除浮动。（本文不讨论清除浮动的方法）

![两栏布局](/images/css-multi-column-layout/2-column-layout.png)

## 绝对定位法 absolute + margin

### HTML
```html
<div class="hd">Header</div>
<div class="bd">
  <div class="aside">侧边栏 固定宽度</div>
  <div class="main">主内容栏 自适应宽度</div>
</div>
<div class="ft">Footer</div>
```

### CSS
```css
.aside {
  position: absolute;
  top: 0;
  left: 0;
  width: 200px;
}
.main {
  margin-left: 210px; /* 10px 列间隙 */
}
```

`DEMO1`：[绝对定位法 absolute + margin 两栏布局](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/absolute-margin.html)

**特点：**1. 在不修改 `HTML` 的情况下，只需要修改 CSS 即可`支持任意调整列顺序`。2. `支持主内容优先显示`，只需将主内容的 `HTML` 放置在前即可。
**缺陷：**绝对定位 `absoulte` 是定位流，会脱离常规流，不影响上下文排版，无法撑开父元素高度。
**结论：**在内容量不可控的场景，不推荐使用该方式。

## 浮动法 float + margin

### HTML
```html
<div class="hd">Header</div>
<div class="bd clearfix">
  <div class="aside">侧边栏 固定宽度</div>
  <div class="main">主内容栏 自适应宽度</div>
</div>
<div class="ft">Footer</div>
```

### CSS
```css
.aside {
  float: left;
  width: 200px;
}
.main {
  margin-left: 210px; /* 10px 列间隙 */
}
```

`DEMO2`：[浮动法 float + margin 两栏布局](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/float-margin.html)

**特点：**1. 在不修改 `HTML` 的情况下，只需要修改 CSS 即可`支持任意调整列顺序`。
**缺陷：**1. `不支持主内容优先显示`。2. 不兼容IE6，`.main` 内部第一个元素存在清除浮动时，会发生 `.main` 掉下去的情况。3. `.main` 中存在清除浮动 `clear: both` 属性，样式会出错，`DEMO3`：[浮动法 float + margin 两栏布局 clear: both BUG](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/float-margin-clear-bug.html)。
**结论：**若主内容栏包含需要清除浮动之类的元素，不适合使用该方式。

## 浮动法改进版一 float + margin + wrap 双标签

### HTML
```html
<div class="hd">Header</div>
<div class="bd clearfix">
  <div class="aside">侧边栏 固定宽度</div>
  <div class="main-wrap">
    <div class="main">主内容栏 自适应宽度</div>
  </div>
</div>
<div class="ft">Footer</div>
```

### CSS
```css
.aside {
  float: left;
  width: 200px;
  position: relative; /* 提升侧边栏层级 */
}
.main-wrap {
  float: right;
  width: 100%;
  margin-left: -200px;
}
.main {
  margin-left: 210px; /* 10px 列间隙 */
}
```

`DEMO4`：[浮动法改进版一 float + margin + wrap 双标签 两栏布局](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/float-margin-wrap.html)

**特点：**1. 在不修改 `HTML` 的情况下，只需要修改 CSS 即可`支持任意调整列顺序`。2. `支持主内容优先显示`，只需将主内容的 `HTML` 放置在前即可。3.兼容性好，兼容所有浏览器。
**缺陷：**结构增加，样式复杂。
**结论：**推荐使用该方法，除了结构复杂没有其他明显缺陷。

## 浮动法改进版二 float + overflow

### HTML
```html
<div class="hd">Header</div>
<div class="bd clearfix">
  <div class="aside">侧边栏 固定宽度</div>
  <div class="main">主内容栏 自适应宽度</div>
</div>
<div class="ft">Footer</div>
```

### CSS
```css
.aside {
  float: left;
  width: 200px;
  margin-right: 10px; /* 10px 列间隙 */
}
.main {
  overflow: hidden; /* 触发BFC */
  *zoom: 1; /* IE6 使用 zoom: 1 触发 hasLayout */
}
```

`DEMO5`：[浮动法改进版二 float + overflow 两栏布局](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/float-overflow.html)

**特点：**1. 在不修改 `HTML` 的情况下，只需要修改 CSS 即可`支持任意调整列顺序`。2. 设置简单，基于 BFC。
**缺陷：**1. `不支持主内容优先显示`。2. `overflow: hidden` 会影响主内容栏滚动条。

## 弹性盒子法 Flexbox 布局

### HTML
```html
<div class="hd">Header</div>
<div class="bd">
  <div class="aside">侧边栏 固定宽度</div>
  <div class="main">主内容栏 自适应宽度</div>
</div>
<div class="ft">Footer</div>
```

### CSS
```css
.bd {
  display: flex;
}
.aside {
  width: 200px;
  margin-right: 10px; /* 10px 列间隙 */
}
.main {
  flex: 1; /* 展开适应宽度 */
}
```

`DEMO6`：[弹性盒子法 Flexbox 布局 两栏布局](https://ihapboy.github.io/front-end-lab/css-multi-column-layout/2-column-layout/flex.html)

**特点：**1. 在不修改 `HTML` 的情况下，只需要修改 CSS 即可`支持任意调整列顺序`（使用 `order` 属性）。2. `支持主内容优先显示`，只需将主内容的 `HTML` 放置在前即可。
**缺陷：**不兼容 IE < 11
**结论：**推荐使用该方法，除了对IE兼容性较差外没有其他明显缺陷。

## 总结

以上为目前主流的两栏布局方法，在不需要兼容 IE < 11 的情况下，推荐使用 `弹性盒子法 Flexbox 布局` 方法，其次推荐 `浮动法改进版一 float + margin + wrap 双标签` 方法。
