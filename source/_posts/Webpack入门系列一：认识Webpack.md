---
title: Webpack入门系列一：认识Webpack
date: 2023-02-17 20:47:37
tags:
- Webpack
categories:
- [前端, 工程化, Webpack]
---

# 背景

一个前端项目由各种各样的资源构成，包括图片PNG、JPG、GIF等，代码资源JS、TS、CSS、LESS、VUE等等。

假设如下情景，一个网页依赖了50个JS文件，且彼此之间有依赖，需要严格按依赖顺序书写。同时由于浏览器只支持CSS与JS，难以直接接入TS、LESS等资源，或对JS新特性存在兼容性问题。这种情况对开发效率的影响非常大。

于是出现了诸如Webpack、Gulp、Vite等工程化工具，有效解决了上述问题，并有了”前端工程“的概念。

# Webpack介绍

Webpack本质上是一种前端资源**编译**、**打包**工具。包括：

- **模块化处理css、图片等资源文件**
- **支持Babel、Eslint、TS、CoffeScript、Less、Sass**
- **多份资源文件打包成一个Bundle**
- 支持HMR+开发服务器
- 支持持续监听、持续构建
- 支持代码分离
- 支持Tree-shaking
- 支持Sourcemap

# 基本使用

1. 安装

   ```
   npm i -D webpack webpack-cli
   ```

2. 编辑配置文件 webpack.config.js

   ```js
   module.exports = {
       entry: 'main.js',
       output: {
           filename: "[name].js",
           path: path.join(__dirname, "./dist"),
       },
       module: {
           rules:[{
               test: /\.less$/i,
               use: ['style-loader', 'css-loader', 'less-loader']
           }]
       }
   }
   ```

3. 执行编译命令

   ```
   npx webpack
   ```

# 核心流程

1. 入口处理

   从`entry`文件开始，启动编译流程。

2. 依赖解析

   根据`require`或`import`等语句找到依赖资源。

3. 资源解析

   根据`module`配置，调用资源转移器，将png、css等非标准JS资源转译为JS内容。

   递归调用2、3，直到所有资源处理完毕。

4. 资源合并打包

   将转译后的资源内容合并打包为可直接在浏览器运行的JS文件。

# 配置

关于Webpack的使用，基本都围绕**配置**展开，这些配置大致可划分为两类：

- 流程类：作用于流程中某个or若干个环节，直接影响打包效果的配置项
  - 输入：`entry`、`context`
  - 模块解析：`resolve`、`externals`
  - 模块转移：`module`
  - 后处理：`optimization`、`mode`、`target`

- 工具类：主流程之外，提供更多工程化能力的配置项
  - 开发效率类：`watch`、`devtool`、`devServer`
  - 性能优化类：`cache`、`performance`
  - 日志类：`stats`、`infrastructureLogging`





