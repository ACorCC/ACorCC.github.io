---
title: Webpack入门系列四：理解插件
date: 2023-02-18 00:03:00
tags:
- Webpack
catecories:
- [前端, 工程化, Webpack]
---

# Why 插件

很多知名工具，如：

- VS Code、Web Storm、Chrome、Firefox
- Babel、Webpack、Rollup、Eslint
- Vue、Redux、Quill、Axios

等等，都设计了所谓”插件“架构，为什么？

对于一个庞大的开源项目，需要多人参与开发，若所有功能都集成在项目里，那么：

- 新人需要了解整个流程细节，上手成本高
- 功能迭代成本高，牵一发动全身
- 功能僵化，作为开源项目而言缺乏成长性
- ...

这会降低一个开源项目的生命力

而使用插件的设计，能让开发者在自己的仓库里修改代码，而不用修改主项目的代码。

插件架构的精髓：对扩展开房，对修改封闭。

# 插件开发

插件开发围绕钩子展开，需要对Webpack复杂的内部过程有细致了解。

# 常用Plugin

- html-webpack-plugin
- clean-webpack-plugin
- define-plugin
- webpack-bundle-analyzer
- mini-css-extract-plugin
- imagemin-webpack-plugin
- web-pack-dashboard