---
title: 如何写好JavaScript
date: 2023-02-04 15:00:16
tags:
- JavaScript
categories:
- [前端]
---

# HTML/CSS/JS 各司其责

- 避免不必要的由JS直接操作样式
- 可以用class来表示状态
- 纯展示类交互寻求零JS方案

# 组件封装

组件设计的原则：封装线、正确性、扩展性、复用性

实现组件的步骤：结构设计、展现效果、行为设计

- 解耦/插件化

  - 将控制元素抽取成插件

  - 插件与组件之间通过**依赖注入**方式建立联系

- 模板化

  - 将HTML模板化，更易于扩展

- 抽象化

  - 将组件通用模型抽象出来

# 过程抽象

高阶函数HOF

- 以函数作为参数
- 以函数作为返回值
- 常用于作为函数装饰器

```js
function HOF(fn) {
    return function(...args) {
        return fn.apply(this, args)
    }
}
```

常用高阶函数

- once

```js
function once(fn) {
    return function(...args) {
        if(fn) {
            const ret = fn.apply(this, args)
            fn = null
            return ret
        }
    }
}
```



- Throttle 节流

```js
function throttle(fn, time = 500) {
    let timer
    return function(...args) {
        if(timer == null) {
            fn.apply(this, args)
            timer = setTimeout(() => {
                timer = null
            }, time)
        }
    }
}
```



- Debounce 防抖

```js
function debounce(fn, dur) {
    dur = dur || 100
    let timer
    return function() {
        clearTimeout(timer)
        timer = setTimeout(() => {
            fn.apply(this, arguments)
        }, dur)
    }
}
```



- Consumer/2 使多个相同的同步过程变成异步

```js
function consumer(fn, time) {
    let tasks = [], timer
    return function(...args) {
        tasks.push(fn.bind(this, ...args))
        if(timer == null) {
            timer = setInterval(() => {
                tasks.shift().call(this)
                if(tasks.length <= 0) {
                    clearInterval(timer)
                    timer = null
                }
            }, time)
        }
    }
}
```



- Iterative 迭代调用fn函数

```js
function iterative(fn) {
    return function(subject, ...rest) {
        if(isIterable(subject)) {
            const ret = []
            for(let obj of subject) {
                ret.push(fn.apply(this, [obj, ...rest]))
            }
            return ret
        }
        return fn.apply(this, [subject, ...rest])
    }
}
```



