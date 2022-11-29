---
title: node
date: 2022-08-18 20:02:49
tags:
---

# Express

## 安装

```powershell
npm init -y
npm i express
```

## 创建server.js

```javascript
// 1.引入express
const express = require('express')
// 2.创建应用对象
const app = express()

// 3.创建路由规则
// request是对请求报文的封装
// response是对响应报文的封装
app.get('/', (request, response) => {
  // 设置响应头
  response.setHeader('Access-Control-Allow-Origin', '*')
  // 设置响应体
  response.send("Hello Express")
})

// 4.监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动，8000端口监听中...");
})
```

## 运行

```powershell
node server.js
// nodemon server.js
```



# HTML部分

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    #result {
      width: 200px;
      height: 100px;
      border: 5px solid pink;
    }
  </style>
</head>
<body>
  <button>点击发送请求</button>
  <div id="result"></div>
</body>
</html>
```

# 发送GET请求

```javascript
const btn = document.querySelector('button')
const result = document.querySelector('#result')
btn.onclick = function() {
  // 1.创建对象
  const xhr = new XMLHttpRequest()
  // 2.初始化 设置请求方法和url
  xhr.open('GET', 'http://127.0.0.1:8000/')
  // 3.发送
  xhr.send();
  // 4.处理服务端返回的结果
  // readystate是xhr对象中的属性，表示状态0 1 2 3 4
  xhr.onreadystatechange = function() {
    if(xhr.readyState === 4) {
      // 判断响应状态码
      if(xhr.status >= 200 && xhr.status < 300) {
        // 响应体
        result.innerHTML = xhr.response
      } else {
        console.log('error ' + xhr.status);
      }
    }
  }
}
```

## 设置请求参数

```javascript
xhr.open('GET', 'http://127.0.0.1:8000/server?a=100&b=200&c=300')
xhr.send()
```

# 发送POST请求

## 前端部分

```js
xhr.open('POST', 'http://127.0.0.1:8000/server')
```

## 后端部分

```javascript
app.post('/server', (request, response) => {
  // 设置响应头
  response.setHeader('Access-Control-Allow-Origin', '*')
  // 设置响应体
  response.send("Hello Ajax Post")
})
```

## 设置请求参数

```javascript
xhr.open('POST', 'http://127.0.0.1:8000/server')
xhr.send('a=100&b=200&c=300')
```

## 设置请求头

### 预定义请求头

```javascript
xhr.open('POST', 'http://127.0.0.1:8000/server')
// Content-Type设置请求体内容的类型
shr.setRequestHeader('Content-Type', 'application/x-www-form-urlencode')
xhr.send('a=100&b=200&c=300')
```

### 自定义请求头

```javascript
xhr.open('POST', 'http://127.0.0.1:8000/server')
// Content-Type设置请求体内容的类型
shr.setRequestHeader('Content-Type', 'application/x-www-form-urlencode')
shr.setRequestHeader('name', 'admin')
xhr.send('a=100&b=200&c=300')
```

此时前端发送的请求变为option

后端部分监听改为all，同时增加对请求头的设置

```javascript
app.all('/server', (request, response) => {
  // 设置响应头
  response.setHeader('Access-Control-Allow-Origin', '*')
  response.setHeader('Access-Control-Allow-Headers', '*')
  // 设置响应体
  response.send("Hello Ajax Post")
})
```



# 服务端响应json数据

## 后端部分

```javascript
app.get('/json-server', (request, response) => {
  response.setHeader('Access-Control-Allow-Origin', '*')
  const data = {
    name: 'admin'
  }
  // 要把数据转化成字符串
  let str = JSON.stringify(data)
  response.send(str)
})
```

## 前端部分

### 手动转化

```javascript
if(xhr.status >= 200 && xhr.status < 300) {
  // 把字符串转化为数据对象
  let data = JSON.parse(xhr.response)
  result.innerHTML = data.name
}
```

### 自动转化

```javascript
// 1.创建对象
const xhr = new XMLHttpRequest()
// 设置响应体数据的类型
xhr.responseType = 'json'
```

# 解决IE缓存问题

在IE老版本中，发起重复的请求会使用缓存，影响时效性较强的数据的获取。

通过给请求增加时间戳，使缓存不被使用，从而重新发起请求。

```javascript
xhr.open('GET', 'http://127.0.0.1:8000/ie?t=' + Date.now())
```

# 请求超时与网络异常处理

## 超时设置

```javascript
// 1.创建对象
const xhr = new XMLHttpRequest()
// 设置超时2s后取消请求
xhr.timeout = 2000
// 2.初始化 设置请求方法和url
xhr.open('GET', 'http://127.0.0.1:8000/')
```

## 超时回调

```javascript
// 1.创建对象
const xhr = new XMLHttpRequest()
// 设置超时2s后取消请求
xhr.timeout = 2000
// 超时回调
xhr.ontimeout = function() {
    alert("网络异常，请稍后重试！！")
}
// 2.初始化 设置请求方法和url
xhr.open('GET', 'http://127.0.0.1:8000/')
```

## 网络异常回调

```javascript
// 1.创建对象
const xhr = new XMLHttpRequest()
// 设置超时2s后取消请求
xhr.timeout = 2000
// 超时回调
xhr.ontimeout = function() {
    alert("网络异常，请稍后重试！")
}
// 网络异常回调
xhr.onerror = function() {
    alert("你的网络似乎出现了一些问题！")
}
// 2.初始化 设置请求方法和url
xhr.open('GET', 'http://127.0.0.1:8000/')
```

# 取消请求

```javascript
xhr.abort()
```

# Axios

## 引入

```javascript
const axios = require('axios')
```

## 发送GET请求

```javascript
axios.get('http://127.0.0.1:8000/axios-server', {
  // url参数
  params: {
    id: 100,
    vip: 7
  },
  // 请求头信息
  headers: {
    name: 'admin',
    age: 20
  }
})
```

## 配置 baseURL

```javascript
axios.defaults.baseURL = 'http://127.0.0.1:8000'
axios.get('/axios-server', {
  // ...
})
```

## 获取返回结果

axios基于promise

```javascript
axios.get('/axios-server', {
  // ...
}).then(response => {
  console.log(response);
  // 响应体
  console.log(response.data);
})
```

## 发送POST请求

```javascript
axios.post('/axios-server', {
    // 请求体
    username: 'admin',
    password: 'admin'
  }, {
    // url参数
    params: {
      id: 100,
      vip: 7
    },
    // 请求头信息
    headers: {
      name: 'admin',
      age: 20
  }
})
```

## axios函数发送ajax请求

```javascript
axios({
  // 请求方式
  method: 'POST',
  // url
  url: '/axios-server',
  // url参数
  params: {
    id: 100,
    vip: 7
  },
  // 请求头信息
  headers: {
    name: 'admin',
    age: 20
  },
  data: {
    // 请求体
    username: 'admin',
    password: 'admin'
  }
})
```

# 跨域

## 同源策略

协议、域名、端口号，必须完全相同

## JSONP解决跨域

利用script标签的跨域能力来发送请求

## CORS解决跨域

CORS（Cross-Origin Resource Sharing, 跨域资源共享）由一系列**HTTP响应头**组成，这些HTTP响应头决定浏览器是否阻止前端JS代码跨域获取资源。

浏览器的**同源安全策略**默认会组织网页的“跨域”获取资源。

cors是Express的一个第三方中间件

使用步骤如下

1. 安装中间件

```powershell
npm install cors
```

2. 导入中间件

```powershell
const cors = require('cors')
```

3. 在路由之前配置中间件

```powershell
app.use(cors())
```

# 在express中使用Session

## 安装express-session中间件

```powershell
npm i express-session
```

## 配置

# 在express中使用jwt

## 安装

```
npm i jsonwebtoken express-jwt
```

## 配置

```javascript
// 用于生成JWT字符串的包
const jwt = require('jsonwebtoken')
// 用于将客户端发送过来的JWT字符串，解析还原成JSON对象的包
const expreeJWT = require('express-jwt')
```

