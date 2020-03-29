---
layout: post
title: "32个js代码小技巧"
date: 2020-3-15 15:12
tags: 
  - javascript
  - js技巧
---


![配图](http://q7yhi2vue.bkt.clouddn.com/image/landscape.jpg)

1、**生成长度为 10 的随机 ID**

   ```javascript
   let id = Math.random().toString(36).slice(2) // "z29mwr6neu"
   ```
<!--more-->
2、**生成随机 16 进制颜色码**

   ```javascript
   let color = `#${Math.random().toString(16).slice(2, 8)}` // "#3386d8"
   ```
3、**小数取整**

   ```javascript
   let number = ~~2.22    // 2
   let number = 2.11 | 0  // 2
   let number = 2.00 >> 0 // 2
   ```
4、**两数交换**

   ```javascript
   let a = 20, b = 10
   
   1. a = a + b; b = a - b; a = a - b
   
   2. [a, b] = [b, a]  // ES6解构
   ```
5、**深拷贝**

   ```javascript
   let clone = JSON.parse(JSON.stringify(obj))  // 只能用于简单数据类型，数组，对象
   ```
6、**递归求阶乘**

   ```javascript
   function factorial(n) {
       return (n > 1) ? n * factorial(n - 1) : n
   }
   
   factorial(5) // 120
   ```
7、**求 1 - n 之和**

   ```javascript
   let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
   function sumHander(arr) {
       let len = arr.length
       return len * (len - 1) / 2  // len * (arr[0] + arr[len - 1]) / 2
   } 
   
   sumHander(arr) // 45
   ```
8、**银行计数法**

   + `正则法`

     ```javascript
     let str = '10000000'
     let reg = /(?=(\B)(\d{3})+$)/g
     
     str.replace(reg, ',') // "10,000,000"
     ```

   + `内置函数法`

     ```javascript
     let str = '100000000'
     function format(str) {
     	let arr = str.split('').reverse()
     	return arr.reduce((prev, next, index) => (index % 3 ? next : next + ',' ) + prev)
     }
     
     format(str) // "100,000,000"
     ```
9、**判断数组中的元素是否都相等**

   ```javascript
   const allEqual = arr => arr.every(item => item === arr[0])
   
   allEqual([1, 1, 1, 1]) // true
   allEqual([0, 1, 2, 3]) // false
   ```
10、**求平均数**

    ```javascript
    const average = (...rest) => (rest.reduce((prev, next) => prev + next) / rest.length)
    
    average(1, 2, 3, 4, 5) // 3
    ```
11、**求字符串的字节长度**

    ```javascript
    const byteSize = str => new Blob([str]).size
    
    byteSize('Hello world') // 11 
    byteSize('你好，世界') // 15 中文汉字在utf-8中到底占3个字节, 在 gbk 编码中占据2个字节
    ```
12、**将首字母转换成大写**

    ```javascript
    const toFirstUpperCase = ([first, ...rest]) => first.toUpperCase() + rest.join('')
    
    toFirstUpperCase('hello world') // "Hello world"
    ```
13、**将首字母转换成小写**

    ```javascript
    const toFirstLowerCase = ([first, ...rest]) => first.toLowerCase() + rest.join('')
    
    toFirstLowerCase('Hello world') // "hello world"
    ```  
14、**将一个句子中的每个单词首字母转换成大写**

    ```javascript
    const capitalize = str => str.replace(/\b\w/g, char => char.toUpperCase())
    
    capitalize('hello world') // "Hello World"
    ```
15、**将连字符隔开的字符串转化成小驼峰**

    ```javascript
    const toSmallHump = str => str.replace(/-(\w)/g, ($, $1) => $1.toUpperCase())
    
    toSmallHump("the-first-name") // "theFirstName"
    ```
16、**移除数组中值为 `false` 的内容**

    ```javascript
    const compact = arr => arr.filter(Boolean)
    
    compact([0, 1, 2, 3, NaN, false, '', undefined]) // [1, 2, 3]
    ```
17、**统计数组中某个值出现的次数**

    ```javascript
    const occurrences = (arr, val) => arr.reduce((count, item) => item === val ? count + 1 : count , 0)
    
    occurrences([1, 2, 1, 2, 3, 4, 5, 1], 1) // 3
    ```
18、**返回当前天是当前年的第几天**

    ```javascript
    const getCurrentDay = date => Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24)
    
    getCurrentDay(new Date()) // 57
    ```
19、**获取当前几点钟**

    ```javascript
    const getNowTime = date => date.toTimeString().slice(0, 8)
    
    getNowTime(new Date()) // "10:03:23"
    ```
20、**获取两个日期之间相差多少天**

    ```javascript
    const getDaysDiffBetweenDates = (dateStart, dateEnd) => (dateEnd - dateStart) / (1000 * 60 * 60 * 24)
    
    getDaysDiffBetweenDates(new Date('2020-01-01'), new Date('2020-02-27')) // 57
    ```
21、**获取元素对应样式**

    ```javascript
    function getStyle(elem, prop) {
    	if(window.getComputedStyle){
    		return window.getComputedStyle(elem,null)[prop];
    	}
    	else{
    		return elem.currenStyle[prop];
    	}
    }
    
    getStyle(document.body, 'padding') // "0px"
    ```
22、**获取数据类型**

    ```javascript
    const getType = val => val === undefined ? 'undefined' : val === null ? 'null' : val.constructor.name.toLowerCase()
    
    getType([1, 2, 3]) // "array"
    getType(1) // "number"
    ```
23、**在某个元素后插入元素**

    ```javascript
    const insertAfter = (targetElem, elem) => {
        const parentElement = targetElem.parentElement
        parentElement && parentElement.insertBefore(elem, targetElem.nextSibling)
    }
    
    insertAfter(document.querySelector('div'), document.createElement('p'))
    ```
24、**返回两个数组元素的交集**

    ```javascript
    const intersection = (a, b) => a.filter(i => b.includes(i))
    
    intersection([1, 2, 3], [2, 3, 4]) // [2, 3]
    ```
25、**数组扁平化**

    ```javascript
    const deepFlatten = arr => [].concat(...arr.map(v => Array.isArray(v) ? deepFlatten(v) : v))
    
    deepFlatten([1, 2, [3, [4, 5]], 6]) // [1, 2, 3, 4, 5, 6]
    ```
26、**数组去重**

    ``` javascript
    const unique = arr => [...new Set(arr)]
    
    unique([1, 1, 2, 2, 3, 4]) // [1, 2, 3, 4]
    ```
27、**将度数转成弧度**

    ```javascript
    const degToRad = deg => (deg * Math.PI) / 180
    
    degToRad(90) // 1.5707963267948966
    ```
28、**将数字拆分成数组**

    ```javascript
    const digitize = num => [...`${num}`].map(i => parseInt(i))
    
    digitize(123) // [1, 2, 3]
    ```
29、**计算两点之间的距离**

    ```javascript
    const distance = (x, y, x1, y1) => Math.hypot(x1- x, y1 - y)
    
    distance(1, 1, 2, 2) // 1.4142135623730951
    ```
30、**按照给定的函数条件筛选数组，将最后一个满足条件的元素进行删除** 

    ```javascript
    const findLast = (arr, fn) => arr.filter(fn).pop()
    
    findLast([1, 2, 3, 4, 5], n => n % 2 === 0) // 4
    ```
31、**判断一个对象是否为空**

    ```javascript
    const isObjectEmpty = obj => Object.keys(obj).length === 0
    
    isObjectEmpty({}) // true
    ```

32、**错误处理代码(有错自动搜索，手动滑稽)**

    ```javascript
    try{
        // your code
    }
    catch (error) {
        window.open(`https://stackoverflow.com/search?q=${String(error)}`)
    }
    ```
