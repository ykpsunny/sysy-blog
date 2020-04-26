---
layout: post
title: "CSS 中常见的布局"
date: 2020-4-24 21:20
tags: 

  + CSS

---

### 一、单列布局

header 与 footer 等宽沾满全屏, body 略窄的单列布局

``` html
  <style>
    .header,
    .footer {
      height: 60px;
    }

    .body {
      width: 80%;
      margin: auto;
      min-height: calc(100vh - 120px);
    }
  </style>
  <div class="container">
    <header class="header"></header>
    <div class="body"></div>
    <footer class="footer"></footer>
  </div>
```

<!-- more -->

### 二、两列自适应布局

两列自适应布局是指一列宽度固定，另一列撑满剩余宽度的布局方式

``` html
<style>
  .body {
    display: flex;
    min-height: 100vh;
  }

  .aside {
    width: 200px;
  }

  .main {
    width: calc(100vw - 200px);
  }
</style>
<div class="container">
  <div class="body">
    <aside class="aside"></aside>
    <main class="main"></main>
  </div>
</div>
```

### 三、三栏布局

中间列自适应宽度，旁边两侧固定宽度

``` html
<style>
  .body {
    display: flex;
    min-height: 100vh;
  }

  .col-left,
  .col-right {
    width: 200px;
  }

  .col-center {
    width: calc(100% - 400px);
  }
</style>
<div class="container">
  <div class="body">
    <div class="col-left"></div>
    <div class="col-center"></div>
    <div class="col-right"></div>
  </div>
</div>
```

### 圣杯布局

比较特殊的三栏布局，同样也是两边固定宽度，中间自适应，唯一区别结构必须是先写中间列部分，这样实现中间列可以优先加载。

实现方式: 

  + 三个部分都设定为左浮动，然后设置 col-center 的宽度为 100%(实现中间列内容自适应)，此时，col-left 和 col-right 部分会被挤到下一行
  + 通过设置 margin-left 为负值让 col-left 和 col-right 部分回到与 col-center 部分同一行
  + 然后通过设置父容器的 padding-left 和 padding-right，让左右两边留出间隙。
  + 最后通过设置相对定位，让 left 和 right 部分移动到两侧。

``` html
<style>
  .body {
    padding: 0 200px;
  }

  .col-left,
  .col-right,
  .col-center {
    float: left;
    min-height: 500px;
  }

  .col-left,
  .col-right {
    width: 200px;
    position: relative;
  }

  .col-center {
    width: 100%;
  }

  .col-left {
    left: -200px;
    /* margin 负数值比较大的话会一直移动到上一行 */
    margin-left: -100%;
  }

  .col-right {
    margin-right: -200px;
  }
</style>
<div class="container">
  <div class="body">
    <div class="col-center"></div>
    <div class="col-left"></div>
    <div class="col-right"></div>
  </div>
</div>
```

### 双飞翼布局

为了解决圣杯布局宽度较小时，左右两列挤下去，以及中间部分不显示的问题。
设置父容器的最小宽度

``` html
<style>
  .body {
    padding: 0 200px;
    /* 设置父容器的最小宽度，显示中间部分 */
    min-width: 400px;
  }

  .col-left,
  .col-right,
  .col-center {
    float: left;
    position: relative;
    min-height: 500px;
  }

  .col-left,
  .col-right {
    width: 200px;
  }

  .col-center {
    width: 100%;
    background-color: #f00;
  }

  .col-left {
    left: -200px;
    /* margin 负数值比较大的话会一直移动到上一行 */
    margin-left: -100%;
    background-color: #000;
  }

  .col-right {
    margin-right: -200px;
    background: #0ff;
  }
</style>
<div class="container">
  <div class="body">
    <div class="col-center">
    </div>
    <div class="col-left"></div>
    <div class="col-right"></div>
  </div>
</div>
```

