---
layout: post
title: "盘点 CSS 中的方法"
date: 2020-4-25 20:20
tags: 

  + CSS

---

### 前言

随着 web3. 0 时代的到来，CSS 语言越来越强大，能实现的功能也比我们想象的要多，以前需要借助 JavaScript 才能完成的事情，现在使用纯 CSS 就能完成，在这篇文章中盘点 CSS 中的函数。

### 一、var

  var 函数使得可以在 CSS 使用变量

使用:

``` css
  :root {
    /* 定义变量 */
    --bg-color: #f40;
  }

  .box {
    /* 使用变量 */
    background: var(--bg-color);
  }
```

[点击查看 var 的详细使用](/2020/03/19/css-variable-1)

<!-- more -->

### 一、calc

  在 CSS 中，calc()可以说是一个计算函数，我们可以在里面运行我们的表达式。

语法:

``` css
  .box {
    width: calc(expression);
  }
```

使用:

``` css
  .box {
    /* 加 */
    height: calc(20px + 2em);
    /* 减 */
    width: calc(100% - 200px);
    /* 乘 */
    padding: calc(20 * 20em);
    /* 除 */
    margin: calc(40em / 20);
  }
```

规则:

  + 可以使用px、em、rem、百分比(%)等单位
  + 可以使用 `+` 、 `-` 、 `*` 和 `/` 进行四则运算
  + 如果为 `+` 或 `-` 时，两边必须要有空格，否则无效， `*` 或 `/` 可以省略

### 二、filter

  在css中，filter 其实是一个属性，他的值是函数，用来做各种滤镜效果的。

  清明节那天，为了纪念因为疫情牺牲的医护，公安等工作人员，很多网站都做了灰色效果，在 filter 属性出现之前，为了达到这种灰色效果，需要花费大量的人力来做这件事，在出现 filter 属性之后，一行代码就能解决这个效果。

语法:

``` css
  html {
    filter: grayscale(1);
  }
```

使用:

``` css
  html {
    /* 灰色效果 */
    filter: grayscale(1);
    /* 模糊效果 */
    filter: blur(20px);
    /* 透明效果, 跟 opacity 属性类似 */
    filter: opacity(0.5);
  }
```

### 三、attr

  attr 可以获取 html 结构上的属性值

语法:

``` css
  .box::after {
    content: attr(attribute);
  }
```

使用:

``` html
  <style>
    .box::after {
      content: attr(data-content);
      /* content: attr(class); */
    }
  </style>
  <div class='box' data-content='content'></div>
```

### 四、linear-gradient 与 radial-gradient

  可以让你在两个或多个指定的颜色之间显示平稳的过渡的颜色。
语法:

``` css
  .box {
    background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
    background-image: radial-gradient(shape, start-color, ..., last-color);
  }
```

使用:

``` html
  <style>
    .box {
      background-image: linear-gradient(to right, #f40, #ff0, #0ff);
      background-image: radial-gradient(circle, #f40, #ff0, #0ff);
      min-height: 200px;
      min-width: 200px;
    }
  </style>
  <div class='box'></div>
```

### 五、repeating-linear-gradient 与 repeating-radial-gradient

  用于重复线性渐变
  
语法:

``` css
  .box {
    background-image: repeating-linear-gradient(color1, color2 5%, color3 10%);
    background-image: repeating-radial-gradient(color1, color2 5%, color3 10%);
  }
```

使用:

``` html
<style>
  .box {
    background-image: repeating-linear-gradient(#f40, #ff0 5%, #0ff 10%);
    background-image: repeating-radial-gradient(#f40, #ff0 5%, #0ff 10%);
    height: 400px;
    width: 400px;
  }
</style>
<div class="box" data-content="content"></div>
```

### 六、cubic-bezier

  贝塞尔曲线函数，一般用于动画节奏感设置

语法:

``` css
  .box {
    transition: all 0.3s cubic-bezier(x1, y1, x2, y2);
  }
```
