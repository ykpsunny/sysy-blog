---
layout: post
title: "immer的使用"
date: 2020-4-6 14:21
tags: 
  + immer
---

JS 里面的变量类型可以分为 `基本类型` 和 `引用类型` ，在 JS 中使用引用类型数据时需要格外的注意引用问题，不然，一不小心就踩坑了！！！

``` javascript
  let a = 10
  let b = a
  b = 5
  console.log(a === b) // ?
```

上面的 `**a**` 跟 `**b**` 会相对吗，因为是基本型数据，所以肯定是 **false**

那我们改成引用类型呢？

``` javascript
  let obj = {
    a: 10
  }
  let obj1 = obj
  obj1.a = 5
  console.log(obj.a) // ?
```

此时应该打印多少？10 还是 5，答案是 5，why？我刚刚明明修改的是 obj1，为什么会影响到 obj 呢，因为对象赋值本质上是把地址赋值给 obj1，并不是把 obj 中的内容赋值给 obj1，这样做是为了节约内存，但是在实际的生产过程当中这样的设计会引发无穷无尽的问题，所以我们要解决这种相互引用的问题。

为了解决互相引用的问题ES6提供了 Object.assign，展开运算符 `...` 

<!-- more -->
**Object.assign 与 `...` 运算符**

``` javascript
  let obj = {
    a: {
      b: 'b'
    }
  }
  let obj1 = {
    ...obj
  }
  // let obj1 = Object.assign({}, obj)
  obj1.a.b = 10
  console.log(obj.a.b) // ？
```

此处打印的是 10，因为 `...` 或 Object.assign 只做了浅层拷贝，对于深层的内容本质上还是相同的引用，所以修改 obj1.a.b 还是会影响到 obj，所以只能深层拷贝，彻底解决引用问题

**深拷贝 --- deepClone**

  深拷贝，顾名思义就是，在遍历赋值的过程中，如果遇到了引用类型的值，就递归创建一个新的数据。

``` javascript
  const deepClone = (origin) => {
    let target = Array.isArray(origin) ? [] : {}
    for (let prop in origin) {
      if (origin.hasOwnProperty(prop)) {
        if (origin[prop] !== null && typeof origin[prop] === 'object') {
          target[prop] = deepClone(origin[prop])
        } else {
          target[prop] = origin[prop]
        }
      }
    }
    return target
  }
  let obj = {
    a: {
      b: 'b'
    }
  }
  let obj1 = deepClone(obj)
  obj1.a.b = 10
  console.log(obj.a.b) // 'b'
```

上面的这个 deepClone 可以满足简单的需求，但是真正在生产工作中，我们需要考虑非常多的因素。

比如：对象之间的循环引用问题，值为 Symbol 类型的数据时，原型链上的内容如何处理，等一系列问题。

因为有太多不确定因素，所以在真正的生产环境中，还是推荐大家使用开源工具库 **loadsh** 中的 `cloneDeep` 。

深拷贝的问题在于给每一个数据都做了拷贝，会浪费内存，所以还是要借助 immer 或 immutable-js 来做性能优化。

**immer**

immer 是一个以 ES6 中 Proxy 为核心的 immutable 库，几乎以最小的成本 (压缩之后仅 3KB) 实现了 js 的不可变数据结构，简单易用 (API 不多，而且使用原生的数据类型)，满足了我们对JS不可变数据结构的需求。

GitHub 地址: https://immerjs.github.io/immer

安装:
  yarn: yarn add immer --save
  npm: npm i immer --save

使用:
 + 用法一:
  

``` javascript
  import produce from 'immer'
  let obj = {
    a: {
      b: 'b'
    }
    c: {
      text: 'c'
    }
  }

  let obj1 = produce(obj, (draftState) => {
    draftState.a.b = 10
  })

  console.log(obj.a.b) // 'b'
  console.log(obj1.a.b) // 10
  console.log(obj.c === obj1.c) // true
```

  通过上面的用法我们能发现，所有具有副作用的逻辑都可以放进 produce 的第二个参数的函数内部进行处理。在这个函数内部对数据进行任何操作，都不会对原对象产生任何影响。
  在不影响的同时，未修改的部分，还保持着相应的关联。
  

  + 用法二:

``` javascript
  import produce from 'immer'
  let obj = {
    a: {
      b: 'b'
    }
  }

  const producer = produce(draftState => {
    draftState.a.b = 10;
  })

  let obj1 = producer(obj)

  console.log(obj.a.b) // 'b'
  console.log(obj1.a.b) // 10
  console.log(obj.c === obj1.c) // true
```

  先传入一个要操作对象的函数，利用高阶函数的特点，返回一个生产者，然后传入需要修改的对象即可。

**在 React 中使用**

有如下数据:

``` javascript
  this.state = {
    userList: [{
        id: 0,
        name: "zs",
        age: 18
      },
      {
        id: 1,
        name: "ls",
        age: 20
      }
    ]
  }
```

需求：给 userList 的第最后一个用户年龄加一岁

不使用 immer 实现:

``` javascript
  let {
    userList
  } = this.state
  let users = userList.slice(),
    lastIndex = users.length - 1
  users[lastIndex].age++
  this.setState({
    userList: users
  })
```

使用 immer 之后:

``` javascript
  this.setState(produce(state => {
    let lastIndex = state.userList.length - 1
    state.userList[lastIndex].age++
  }))
```

是不是感觉代码量瞬间少了很多，阅读起来舒服了很多，而且更易于阅读了。

**React Hooks 中使用**

``` javascript
  const [data, setData] = useState({
    a: 'a',
    b: 'b',
    c: {
      d: 'd',
      f: 'f'
    }
  })
```

需求: 把对象 c 里的 d 的值改为 10

不使用 immer 实现:

``` javascript
  setData(state => {
    return {
      ...state,
      c: {
        ...state.c,
        d: 10
      }
    };
  })
```

使用 immer 之后:

``` javascript
  setData(
    produce(state => {
      state.c.d = 10;
      return state;
    })
  )
```

immer + React Hooks 使用起来真的很顺手，而且更易于阅读，目前我就一直在用。

**React reducer 中使用**

不使用 immer:

``` javascript
  const reducer = (state, action) => {
    switch (action.type) {
      case 'ADD':
        return {
          ...state,
          count:
            state.count++
        }
        break;
      case 'SUB':
        return {
          ...state,
          count:
            state.count--
        }
        default:
          break;
    }
  }
```

使用 immer 之后:

``` javascript
  const reducer = produce((state, action) => {
    switch (action.type) {
      case 'ADD':
        state.count++
        break;
      case 'SUB':
        state.count--
      default:
        break;
    }
  })
```

**immer 原理解析**

immer 源码中，使用了一个 ES6 的新特性 Proxy 对象。Proxy 对象拦截某些操作并实现自定义行为。

**immer 中的proxy**

immer 的做法就是在内部维护一份 state，劫持所有操作，内部来判断是否有变化从而最终决定如何返回。下面这个例子就是一个构造函数，如果将它的实例传入 Proxy 对象作为第一个参数，就能够后面的处理对象中使用其中的方法:

``` javascript
class Store {
  constructor(state) {
    this.modified = false
    this.source = state
    this.copy = null
  }
  get(key) {
    return !this.modified ?
      return this.source[key]: this.copy[key]
  }
  set(key, value) {
    if (!this.modified) {
      this.modifing()
    }
    return this.copy[key] = value
  }
  modifing() {
    if (this.modified) return
    this.modified = true
    // 使用解构实现一层 immutable
    this.copy = Array.isArray(this.source) ? [...this.source] : {
      ...this.source
    }
  }
}
```

上面这个 Store 构造函数相比源代码省略了很多判断的部分。实例上面有 modified，source，copy 三个属性，有 get，set，modifing 三个方法。modified 作为内置的 flag，判断如何进行设置和返回。

里面最关键的就应该是 modifing 这个函数，如果触发了 setter 并且之前没有改动过的话，就会手动将 modified 这个 flag 设置为 true，并且使用解构实现一层 immutable。

对于 Proxy 的第二个参数，在简版的实现中，我们只是简单做一层转发，任何对元素的读取和写入都转发到 store 实例内部方法去处理。

``` javascript
  const PROXY_FLAG = 'SYMBOL_PROXY_FLAG'
  const handler = {
    get(target, key) {
      // 如果遇到了这个 flag 我们直接返回我们操作的 target
      return key === PROXY_FLAG ? target : target.get(key)
    },
    set(target, key, value) {
      return target.set(key, value)
    }
  }
```

这里在 getter 里面加一个 flag 的目的就在于将来从 proxy 对象中获取 store 实例更加方便。

最终我们能够完成这个 produce 函数，创建 store 实例后创建 proxy 实例。然后将创建的 proxy 实例传入第二个函数中去。这样无论在内部做怎样有副作用的事情，最终都会在 store 实例内部将它解决。最终得到了修改之后的 proxy 对象，而 proxy 对象内部已经维护了两份 state ，通过判断 modified 的值来确定究竟返回哪一份。

```javascript
  function produce(state, producer) {
  const store = new Store(state)
  const proxy = new Proxy(store, handler)
  
  // 执行我们传入的 producer 函数，我们实际操作的都是 proxy 实例，所有有副作用的操作都会在 proxy 内部进行判断，是否最终要对 store 进行改动。
  producer(proxy)

  // 处理完成之后，通过 flag 拿到 store 实例
  const newState = proxy[PROXY_FLAG]
  return newState.modified ? newState.copy : newState.source
}

```

这样，一个分割成 Store 构造函数，handler 处理对象和 produce 处理 state 这三个模块的最简版就完成了，将它们组合起来就是一个最最最 tiny 版的 immer ，里面去除了很多不必要的校验和冗余的变量。但真正的 immer 内部也有其他的功能，例如上面提到的深层嵌套对象的结构化共享等等。

当然，Proxy 作为一个新的 API，并不是所有环境都支持，Proxy 也无法 polyfill，所以 immer 在不支持 Proxy 的环境中，使用 Object.defineProperty 来进行一个兼容。

参考资料: 
  + [immer.js: 也许更适合你的immutable js库](https://juejin.im/post/5e83e532f265da47e02a6d5a)
  + [官方文档](https://github.com/immerjs/immer/tree/master/docs)

