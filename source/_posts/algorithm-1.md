---
layout: post
title: "两数之和与洗牌"
date: 2020-4-12 15:20
tags: 

  + 算法题

---

#### 1、两数之和

给定一个整数数组 `mums` 和一个目标值 `target` , 请你在该数组中找出和为目标值的那两个整数, 并返回他们的数组下标。

可以假设每种入只会对应一个答案。

**示例:**

给定 nums = [1, 3, 5, 6, 11], target = 7 

因为nums[0] + nums[3] = 1 + 6 = 7 
所以返回[0, 3]

<!-- more -->
``` javascript
function findSumIndex(arr, target) {
  for (let i = 0, len = arr.length; i < len; i++) {
    for (let j = 0; j < len; j++) {
      if (arr[i] + arr[j] === target) {
        return [i, j]
      }
    }
  }
}

findSumIndex([1, 3, 5, 6, 11], 7) // [0, 3]
```

#### 2、洗牌算法

+ 交换打乱
  其基本思想就是从原始数组中随机取一个之前没取过的数字与最后一个数字交换位置

``` javascript
  function shuffle(arr) {
    const newArr = arr.slice()
    for (let i = newArr.length - 1; i >= 0; i--) {
      const rmdIndex = Math.floor(Math.random() * i) // 随机取一个下标
      // 两数交换
      const temp = newArr[i]
      newArr[i] = newArr[rmdIndex]
      newArr[rmdIndex] = temp
    }
    return newArr
  }
  shuffle([1, 2, 3, 4, 5, 6, 7, 8]) // [7, 4, 2, 1, 8, 5, 6, 3]
```

+ 随机打乱
  利用数组 `sort` 方法的函数参数特性
  参数为函数时:

    1. 可以有参数（两个参数排序），也可以没有参数（乱序）
    2. 看返回值
  
		+ 当返回值为负数时，那么前面的数放在前面
		+ 当返回值为正数时，那么后面的数放在前面
		+ 为0时，不动

``` javascript
  const arr = [1, 2, 3, 4, 5, 6, 7, 8]
  arr.sort(() => Math.random() - 0.5) // [2, 7, 5, 1, 8, 3, 6, 4]
```

