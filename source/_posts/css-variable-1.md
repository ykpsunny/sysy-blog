---
layout: post
title: "CSS 变量(variable) -- 基础篇"
date: 2020-3-19 21:09
tags: 
  - css
---

#### 前言

​	css 变量可以让你不使用任何预处理器 (less, sass, stylus)，不需要通过任何工具预处理，可以原生在浏览器中使用变量

#### 一、变量的声明

​	声明变量的时候，变量名前要加两根连字符 **(- -)**。
​```css
:root{
    --bg-color: #f40;
}
```

​	上面代码中，声明了一个css变量，你可能会问，为什么官方要选择两根连字符表示变量？因为 $bg-color 被 sass 用掉了，@bg-color 被 less 用掉了，所以官方为了不产生冲突，css 声明变量改用两根连字符了。

<!--more-->
#### 二、变量的使用

​	css 中使用 **var** 函数用于读取变量 (没错，就是 js 中声明变量的那个 var，不过，此 var (css) 非比 var (js) )。
```css
:root{
    --bg-color: #f40;
}
div{
    background-color: var(--bg-color);
}
```

**var** 函数具有两个参数，第一个参数表示使用的变量，第二个参数表示默认值，那么就存在以下两种使用情况:

1. 两个参数都传递，如果变量存在，则正确引用，如果使用的变量不存在，则会使用默认值。
2. 只传递第一个参数，且变量存在，则正确使用变量， 如果变量不存在，则默认使用失败。

```css
div{
    color: var(--color, #ff0); // 变量不存在，则使用默认值
}
```

**注意 **: 变量只能用作属性值，不能用作属性名。

```css
div{
    --font: font-size;
    var(--font): 18px; // 无效
}
```

#### 三、变量值的类型

1. 可以是字符串

   ```css
   div{
       --content: 'hello world !!!'
   }
   div::after{
       content: var(--content); // 允许
   }
   ```

2. 可以是数字，但是不能与单位直接连用，需要借助 **calc** 计算函数

   ```css
   div{
       --num: 10;
       margin: var(--num)px; // 错误
       margin: calc(var(--num) * 1px); // 正确
   }
   ```

#### 四、作用域

变量只在当前元素及其子元素中有效

```html
<style>
    .parent{
        --color: #f00;
    }
    .children{
        color: var(--color);
    }
    .box{
        color: var(--color); // 无效
    }
</style>
<div class='parent'>
    <div class='children'>hello world !!!</div>
</div>
<div class='box'>box</div>
```

此时只有 hello world !!! 变成了红色字体，而 box 中的元素还是黑色字体，由于这个原因一般把声明的变量放在根元素 **:root** 里面，以确保任何元素都可以使用它们。

#### 五、兼容性

目前现代浏览器都支持(IE 除外，毕竟微软都抛弃了 [手动狗头])

![css变量兼容性](/assets/blogImg/css-variable.png)