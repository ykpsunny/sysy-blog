---
layout: post
title: "JS 字典"
date: 2020-5-16 19:20
tags:

  + js

---

最近朋友问我一道题目, 所以打算记录下, 题目如下:
  给定一个 "扁平化" 字典对象, 其键以点分隔。例如: {'A': 1, 'B. A': 2, 'B. B': 3, 'C. D. E': 4, 'C. D. F': 5}。实现将其转换为 **嵌套** 字典对象的功能。在上述的描述情况下，嵌套版本如下:
``` JS
{
  A: 1, 
  B: {

    A: 2,
    B: 3

  }, 
  C: {

    D: {
      E: 4,
      F: 5
    }

  }
}

```

``` javascript

  let obj = {
    "A": 1,
    "B.A": 2,
    "B.B": 3,
    "C.D.E": 4,
    "C.D.F": 5
  };

  function dictionariesObj(origin, target = {}) {

    function assembleObj(origin, prop, value) {
      let index = prop.indexOf(".");
      if (index === -1) {
        origin[prop] = value;
        return origin;
      }
      let key = prop.slice(0, index);
      origin[key] = {
        ...origin[key],
      };
      return assembleObj(origin[key], prop.slice(index + 1), value);
    }

    for (let prop in obj) {
      if (obj.hasOwnProperty(prop)) {
        if (prop.indexOf(".") > -1) {
          assembleObj(target, prop, obj[prop]);
        } else {
          target[prop] = obj[prop];
        }
      }
    }

    return target;
  }

  dictionariesObj(obj)

```

上面是转成嵌套版本的，那从 **嵌套** 版本转成 **扁平** 版本呢 ? 实现方式如下:


