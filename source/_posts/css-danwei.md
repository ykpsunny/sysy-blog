---
layout: post
title: "CSS 中常见的单位"
date: 2020-4-20 21:20
tags: 

  + CSS

---
### 前言
  CSS 中有很多用于表式长度的单位, 比如常见的 px, em, rem等等。单位又分绝对单位与相对单位, 数字与单位之间不能出现空格。如果长度值为 0，则可以省略单位。对于一些 CSS 属性，长度可以是负数。

### 绝对单位
  绝对长度单位是一个固定值, 它反映的是一个真实的物理尺寸, 不会因为其他元素的尺寸变化而变化

**px**
  px是一个绝对单位，但是由于各种设备的 dpi(分辨率) 不同，每1个 CSS 像素所代表的物理像素是可以变化的，所以 px 也有其相对性(相对于分辨率)。

<!-- more -->

### 相对单位

**em**
  em 的值并不是固定的, 而是由其他的元素来决定的相对值, em 会继承父级元素的字体大小(font-size), 表示自身字体大小(font-size具有继承性, 如果自身没有设置 font-size 则继承自父级)。
计算规则: 1 ÷ 父元素的font-size × 需要转换的 px 值 = em 值

**rem**
  rem 是相对根节点 html 的字体大小(font-size)来计算, 一般用于移动端布局, 根据不同的窗口大小, 来计算根元素的字体大小, 以提高用户视觉体验。
计算规则: 1 ÷ 根元素(html)font-size × 需要转换的 px 值 = em 值

**vw**
  表示 Viewpoint Width (视口宽度) 百分比。1vw 代表视口宽度的 1%。例如，如果视口的宽度是 1920px, 10vh求得的值为 192px。

**vh**
  表示 Viewpoint Height (视口高度) 百分比。1vh 代表视口高度的 1%。例如，如果视口的高度是 800px, 1vh求得的值为 8px。 

**vmin**
  表示 vm 和 vh 中较小的那个。

**vmax**
  表示 vm 和 vh 中较大的那个。

![vw-vh](http://q7yhi2vue.bkt.clouddn.com/image/vw-vh.png)

vh 和vw 总是与视口的高度和宽度有关，与之不同的，vmin 和vmax 是与视口宽度和高度的最大值或最小值有关，取决于哪个更大和更小。例如，如果视口的宽为 1440px、高为 660px，10vmin 为 66px, 10vmax 为 144px。

**百分比(%)**
  CSS中的百分比参照的值不同, 最终计算出来的值也是不同的。

+ 正常情况下，百分比设置宽高一般参照的都是父级的宽高 (不包含 padding, margin, border)，比如父级宽度 `500px`, 高度 `300px`, `padding: 20px`, 子集(width: 50%, height: 50%)的宽为 `250px`, 高为 `150px`，如果父级没有显示的设置宽高, 则子集的高度为 **0**。
注: 参照宽度为 width || height，不区分盒模型

+ 定位情况下，如果子集 `position: absolute`, 则参照最近定位不是 `static` 的父级的宽高 (不包含 margin, border), 如果一直到根元素都没有定位的话, 则参照的是第一屏可视区的宽高, 如果子集 `position: fixed`，则参照的是第一屏可视区的高度与宽度
注: 参照宽度为 width + padding || height + padding，不区分盒模型


translateX()的百分比相对于自身的 `width + padding + border` 来计算
translateY()的百分比相对于自身的 `height + padding + border` 来计算

巧妙使用**百分比(%)**实现水平垂直居中

```html
  <style>
    .container {
      width: 500px;
      height: 300px;
      background-color: #f00;
      padding: 20px;
      position: relative;
      border: 10px solid black;
    }
    .inner {
      width: 50%;
      height: 50%;
      border: 10px solid black;
      padding: 10px;
      background-color: #0ff;
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
    }
  </style>
  <div class="container">
    <div class="inner"></div>
  </div>
```

![css-center](http://q7yhi2vue.bkt.clouddn.com/image/css-center.png)