---
title: Webpack入门系列二：常见配置
date: 2023-02-17 21:20:29
tags:
- Webpack
catecories:
- [前端, 工程化, Webpack]
---

# 流程类配置

## entry、output

文件结构

```
.
|-- src
	|-- index.js
|-- webpack.config.js
```

**entry**入口文件

```java
module.exports = {
    entry: 'main.js',
}
```

**output**产物出口

```javascript
module.exports = {
    entry: 'main.js',
    output: {
        filename: "[name].js",
        path: path.join(__dirname, "./dist"),
    },
}
```

## 处理CSS

文件结构

```
.
|-- src
	|-- index.js
	|-- index.css
|-- webpack.config.js
```

`index.js`

```javascript
const styles = require('./index.css')
// or
import styles from './index.css'
```

js本身只支持导入js文件，不支持其它格式的资源。因此需要使用相关Loader来处理。

1. 安装Loader

   ```
   npm add iD css-loader style-loader
   ```

2. 添加`module`处理css文件，test为匹配规则，use为链式调用的Loader（从后往前调用）

```javascript
const path = require('path')

module.exports = {
    entry: 'main.js',
    output: {
        filename: "[name].js",
        path: path.join(__dirname, "./dist"),
    },
    module: {
        // css处理器
        rules:[{
            test: /\.css/i,
            use: [
                'style-loader',
                'css-loader'
            ]
        }]
    }
}
```

## Babel

`index.js`

```javascript
class Person {
    constructor() {
        this.name = 'Tecvan'
    }
}

console.log((new Person()).name)

const say = () => {}
```

Babel可以将es6语法转化为兼容低版本浏览器的js代码

1. 安装依赖

   ```
   npm i -D @babel.core @babel/preset-env babel-loader
   ```

2. 声明产物出口`output`

   ```javascript
   const path = require('path')
   module.exports = {
       entry: 'main.js',
       output: {
           filename: "[name].js",
           path: path.join(__dirname, "./dist"),
       },
       module: {
           rules:[{
               test: /\.js?$/,
               use: [{
                   loader: 'babel-loader',
                   options: {
                       persets: [
                           [
                               '@babel/preset-env'
                           ]
                       ]
                   }
               }, ]
           }]
       }
   }
   ```

3. 执行`npx webpack`

## 生成HTML

文件结构

```
.
|-- src
	|-- index.js
|-- webpack.config.js
```

1. 安装依赖

   ```
   npm i -D html-webpack-plugin
   ```

2. 声明产物出口`output`

   ```javascript
   const path = require('path')
   module.exports = {
       entry: 'main.js',
       output: {
           filename: "[name].js",
           path: path.join(__dirname, "./dist"),
       },
       plugin: [new HtmlWebpackPlugin()]
   }
   ```

3. 执行`npx webpack`

文件结构

```
.
|-- dist
	|-- index.html
	|-- main.js
|-- src
	|-- index.js
|-- webpack.config.js
```

## context

## mode

## target

## module

## plugins

# 工具类配置

## HMR

Hot Module Replacement，模块热替换

1. 开启HMR

   ```javascript
   module.exports = {
       // ...
       devServer: {
           hot: true
       }
   }
   ```

2. 启动Webpack

   ```
   npx webpack serve
   ```

## Tree-Shaking

用于删除Dead Code，如没有被用到、不可到达的代码，执行结果不会被用到的代码……

```javascript
module.exports = {
    entry: "./src/index",
    mode: "production",
    devtool: false,
    optimization: {
        usedExports: true,
    }
}
```

## devtool

## devServer

## watch

