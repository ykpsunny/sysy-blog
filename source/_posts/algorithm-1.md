---
layout: post
title: "两数之和、洗牌、数组中的第 K 个最大元素、爬楼梯"
date: 2020-4-12 15:20
tags: 

  + 算法题

---

#### 1、两数之和

给定一个整数数组 `nums` 和一个目标值 `target` , 请你在该数组中找出和为目标值的那两个整数, 并返回他们的数组下标。

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

* 交换打乱

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

* 随机打乱

  利用数组 `sort` 方法的函数参数特性
  参数为函数时:

    1. 可以有参数（两个参数排序），也可以没有参数（乱序）
    2. 看返回值

  

    - 当返回值为负数时，那么前面的数放在前面
    - 当返回值为正数时，那么后面的数放在前面
    - 为0时，不动

``` javascript
  const arr = [1, 2, 3, 4, 5, 6, 7, 8]
  arr.sort(() => Math.random() - 0.5) // [2, 7, 5, 1, 8, 3, 6, 4]
```

#### 3、数组中的第K个最大元素

  在未排序的数组中找到第 `K` 个最大的元素。
  注意: 需要找到是数组排序后的第 `K` 个最大的元素，而不是第 K 个不同的元素。

  示例 1:

    输入: [2, 3, 1, 4, 5] 和 k = 3
    输出: 3

  示例 2:

    输入: [3, 2, 3, 4, 1, 4, 5, 6, 7, 6] 和 k = 3
    输出: 6

``` javascript
  const arr = [3, 2, 3, 4, 1, 4, 5, 6, 7, 6],
    k = 3
  // 方法一
  function quickSort(arr) {
    if (arr.length <= 1) {
      return arr
    }
    const baseItem = arr.splice(Math.floor(arr.length / 2), 1)[0]
    const left = [],
      right = []
    for (let i = 0, len = arr.length; i < len; i++) {
      baseItem > arr[i] ? left.push(arr[i]) : right.push(arr[i])
    }
    return quickSort(right).concat(baseItem, quickSort(left))
  }
  quickSort()[k - 1] // 6
  // 方法二
  arr.sort((a, b) => b - a)[k - 1] // 6
```

#### 4、爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1:

  输入： 2
  输出： 2
  解释： 有两种方法可以爬到楼顶。

  1.  1 + 1
  2.  2

示例 2:

  输入： 4
  输出： 5
  解释： 有三种方法可以爬到楼顶。

  1. 1 + 1 + 1 + 1
  2. 1 + 1 + 2
  3. 1 + 2 + 1
  4. 2 + 1 + 1
  5. 2 + 2

``` javascript
  function climbStairs(n) {
    let arr = [0, 1, 2, 3]
    for (let i = 4; i <= n; i++) {
      arr[i] = arr[i - 1] + arr[i - 2]
    }
    return arr[n]
  }
  // 递归法
  function climbStairs (n) {
    if (n <= 2) {
      return n
    }
    return climbStairs(n - 1) + climbStairs(n - 2)
  }
  climbStairs(4) // 5
```

