---
title: Webpack入门系列三：理解Loader
date: 2023-02-17 22:18:55
tags:
- Webpack
categories:
- [前端, 工程化, Webpack]
---

# Why Loader

Webpack只认识JS，为了处理非标准JS资源，设计出资源翻译模块——Loader，用于将资源翻译为标准JS。

# Usage

1. npm安装Loader模块
2. 在配置文件中添加具体`module`规则

# Loader特性

- 链式调用

- 支持异步执行
- 分normal、pitch两种模式

# 常用Loader

掌握这些常用Loader的功能、配置方法

- JavaScript
  - **babel-loader**
  - **eslint-loader**
  - ts-loader
  - buble-loader
  - **vue-loader**
  - angular2-template-loader
- CSS
  - **css-loader**
  - **style-laoder**
  - **less-loader**
  - **sass-loader**
  - **stylus-loader**
  - **postcss-loader**
- HTML
  - html-loader
  - pug-loader
  - posthtml-loader
- Assets
  - **file-loader**
  - val-loader
  - **url-loader**
  - json5-loader



