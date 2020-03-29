---
layout: post
title: "CSS 变量(variable) -- 实战篇"
date: 2020-3-20 21:31
tags:

  + css

---

#### 一、 网站换肤

1、实现网站换肤功能，一般最先想到的是用全局 class 控制样式切换
缺点：全局控制 CSS，在项目不大，换肤样式不多的情况下，还能勉强够用。但是换肤样式很多的话，代码会非常臃肿，不利于维护。
2、通过 js 来切换引入 CSS 样式的 href 属性值，来达到样式切换的目的。
缺点：使用 js 改变 href 属性会带来加载延迟(样式文件越大，加载越久)。
3、使用 js 修改 css 变量值来达到换肤效果。

今天我们要说的就是第三种, 使用 js 修改 css 变量值来达到换肤效果

<!--more-->
![配图](http://q7yhi2vue.bkt.clouddn.com/image/wood.jpg)

``` html
<style>
    .box {
        color: var(--color);
        background-color: var(--bg-color);
    }

    .box:hover {
        color: var(--active-color);
    }
</style>
<div class="box">box</div>
<button id="btn1">风格一</button>
<button id="btn2">风格二</button>
```

``` javascript
// 先在根元素上定义背景颜色、字体颜色、active 颜色等变量
const style1 = {
    "--bg-color": "#f5f5f5",
    "--color": "#222",
    "--active-color": "#c90000"
};
const style2 = {
    "--bg-color": "#fff",
    "--color": "#333",
    "--active-color": "#ff6700"
};
const root = document.documentElement,
    btn1 = document.getElementById("btn1"),
    btn2 = document.getElementById("btn2");
btn1.addEventListener("click", () => styleHandle(root, style1));
btn2.addEventListener("click", () => styleHandle(root, style2));

function styleHandle(elem, style) {
    for (let prop in style) {
        if (style.hasOwnProperty(prop)) {
            elem.style.setProperty(prop, style[prop]);
        }
    }
}
```

#### 二、 条形加载 Loading

![loading bar](http://q7yhi2vue.bkt.clouddn.com/image/loading-bar.gif)

一个条形加载 Loading 通常由几条线组成，并且每条线存在不同的时延，通过时间差运行相同的动画，从而产生 loading 效果，估计大部分的同学可能会把 css 写成一下这样。

``` html
<style>
    #loading {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color: #fff;
    }

    #loading .item {
        margin: 0 10px;
        height: 30px;
        width: 20px;
        background-color: #333;
        border-radius: 5px;
        animation-iteration-count: infinite;
        animation-timing-function: linear;
        animation-duration: 1s;
        animation-name: loading;
    }

    #loading .item:nth-child(2) {
        animation-delay: 100ms;
    }

    #loading .item:nth-child(3) {
        animation-delay: 200ms;
    }

    #loading .item:nth-child(4) {
        animation-delay: 300ms;
    }

    #loading .item:nth-child(5) {
        animation-delay: 400ms;
    }

    @keyframes loading {
        0% {
            height: 30px;
        }

        50% {
            height: 120px;
        }
    }
</style>
<div id="loading"></div>
<script>
    let loading = document.getElementById("loading"),
        fragment = document.createDocumentFragment();
    for (let i = 0; i < 5; i++) {
        let div = document.createElement("div");
        div.className = `item` ;
        fragment.appendChild(div);
    }
    loading.appendChild(fragment);
</script>
```

分析代码发现，每个 item 只是存在 animation-delay 不同，而其余代码则完全相同，如果有 100 个 item，那岂不是要写 100 个 :nth-child。
显然这种方式不灵活，我们可以使用 css 变量来代替。

对于 HTML 部分的修改，让每个 item 添加一个 css 变量，对于 css 部分的修改，对规律变化的部分使用 css 变量来代替即可。

``` html
<style>
    #loading {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        background-color: #fff;
    }

    #loading .item {
        margin: 0 10px;
        height: 30px;
        width: 20px;
        background-color: #333;
        border-radius: 5px;
        animation: loading 1s linear var(--time) infinite;
    }

    @keyframes loading {
        0% {
            height: 30px;
        }

        50% {
            height: 120px;
        }
    }
</style>
<div id="loading"></div>
<script>
    let loading = document.getElementById("loading"),
        fragment = document.createDocumentFragment();
    for (let i = 0; i < 5; i++) {
        let div = document.createElement("div");
        div.className = `item` ;
        div.style.setProperty("--time", `${i * 100}ms` );
        fragment.appendChild(div);
    }
    loading.appendChild(fragment);
</script>
```

通过巧妙的使用 css 变量，代码大大的减少了，而且灵活性也变强了(某天说加载的时间差效果不明显，直接将 100 改成 150 即可，无需对每个 :nth-child(n) 进行修改)。

#### 三、 悬浮跟踪效果

其实思路也比较简单，先对按钮进行布局和着色，然后使用伪元素标记鼠标的位置，定义 `--x` 和 `--y` 表示伪元素在按钮里的坐标位置，通过 js 获取鼠标在按钮上的 offsetLeft 和 offsetTop 分别赋值给 `--x` 和 `--y` ，再对伪元素添加径向渐变的背景颜色。

![hover-button](http://q7yhi2vue.bkt.clouddn.com/image/hover-button.gif)

第一步: 获取鼠标的位置，计算相对按钮的偏移位置，将坐标存到变量中

``` javascript
const btn = document.getElementById('btn')
btn.addEventListener('mousemove', (e) => {
    const {
        offsetLeft,
        offsetTop,
        style
    } = e.target
    const x = e.pageX - offsetLeft,
        y = e.pageY - offsetTop
    style.setProperty('--x', `${x}px` )
    style.setProperty('--y', `${y}px` )
})
```

第二步：定义 `span` 元素的层级，渐变显示在文字上方，将伪元素 `after` 的 `width` 和 `height` 都默认设置为 0 (不显示)，当鼠标经过时改为 `300px` ， 并设置动画，在background 属性上应用径向渐变 `radial-gradient` ，使用。closest-side覆盖整个面。

``` html
  <style>
      #btn {
          width: 146px;
          height: 46px;
          line-height: 46px;
          text-align: center;
          border-radius: 10px;
          font-size: 18px;
          color: #fff;
          background-color: #788cff;
          font-weight: bold;
          display: inline-block;
          cursor: pointer;
          user-select: none;
          position: relative;
          overflow: hidden;
      }

      #btn span {
          position: relative;
          z-index: 999;
      }

      #btn::after {
          content: '';
          position: absolute;
          left: var(--x);
          top: var(--y);
          width: var(--size);
          height: var(--size);
          background: radial-gradient(#c47dde, transparent);
          transform: translate(-50%, -50%);
          /* 让伪元素中心跟随着鼠标点 */
          transition: width .1s ease-in-out,
                      height .1s ease-in-out;
      }

      #btn:hover::after {
          --size: 300px;
      }
  </style>
  <div id="btn">
      <span>click me</span>
  </div>
```


就这样，一个炫酷的按钮就大功告成了。

