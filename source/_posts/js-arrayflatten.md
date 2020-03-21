---
layout: post
title: 'JS 数组扁平化 (flatten)'
date: 2020-3-21 18:15
tags: 
  - javascript
  - js-array
---

#### 前言

​	数组是 JS 中使用评率仅次于对象的数据结构，官方提供了诸多 API (比如：push，pop，shift，unshift ....)，今天我们来谈谈如何扁平化数组。



> 顾名思义，扁平化数组就是将一些嵌套的多维数组转成一维数组。



我们以一个例子贯穿全文:

```javascript
const array = [1, 2, [3, 4, [5, 6], [7, 8], [9]], 10]
```



#### 一、ES10 中提供的数组扁平化方法 --- flat

> **flat** 方法会按照一个可指定的深度进行递归遍历数组，并将所有遍历到的元素合并为一个新的数组返回。

##### 语法

```javascript
const newArray = arr.flat([depth])
```

注意：dpeth 参数是可选的, 指定要提取嵌套数组的结构深度，默认值为 1

```javascript
const result = array.flat(Infinity) // 当参数为 Infinity 时代表，递归任意深度的嵌套数组

console.log(result) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### 二、for...of 实现

```javascript
const flatten = (function () {
    const result = []
    return (arr) => {
        for (const item of arr) {
            Array.isArray(item) ? flatten(item) : result.push(item)
        }
        return result
    }
}())
console.log(flatten(array)) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### 三、生成器(Generator)实现

```javascript
function *flatten (arr) {
    for (const item of arr) {
        Array.isArray(item) ? yield *flatten(item) : yield item
    }
}
console.log([...flatten(array)]) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### 四、reduce 实现

```javascript
function flatten (arr) {
    return arr.reduce((result, item) => {
        return result.concat(Array.isArray(item) ? flatten(item) : item)
    }, [])
}
console.log(flatten(array)) // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

















