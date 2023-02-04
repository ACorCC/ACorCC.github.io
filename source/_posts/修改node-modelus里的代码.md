---
title: 修改node_modelus里的代码
date: 2023-02-04 23:05:50
tags:
- 工程化
categories:
- [前端]
---

在开发中，如果有必要修改node_modules里的代码，有哪几种方法呢？哪种方法最好呢？

# 第一种：直接改
这种很容易理解，就是直接进node_modules中，找到相关包的代码，并修改相应位置代码，然后重启项目即可。
但是这样做存在如下弊端：
1. 只能是你自己本地用你修改的代码，其他人用不了；
2. 下次npm install 之后之前修改的代码都会恢复原状；

# 第二种：独立维护一个包
假如我使用了包A，它限制了上传文件的格式，但是我的业务要求是放开所有限制，此时我可以这样：在原有包的基础上copy一个包B，，修改相关代码后把包B推送到npm上，此时我的项目中不需要原来的包了，用我刚维护的包B就可以了，这样就可以达到效果。以前我也是这么做的。这样做的缺点就是会增加维护的成本，当然个人认为这种成本可以忽略不计，因为我改完后很长时间从来没有再次改动过。

# 第三种: patch-package
这是一个专门用来修改node_modules中包的代码的工具，使用方式也很简单：
1. 安装patch-package

```
npm i patch-package
```

2. 修改node_modules

比如我想修改包A，那么我直接在node_modules中修改，然后执行

```
npx patch-package A
```

这时候你的根目录下就会出现patches这个目录，里面会出现一个包A的补丁文件，这个文件大有用处！

注意：记得要把 patches 这个目录提交到git或者svn

这个时候你本地已经使用到了你修改后的代码了。那现在的问题是怎么让其他人也同步到你修改后的代码。

3. “postinstall”:“patch-package”

在package.json的script中增加：

```
"postinstall": "patch-package"
```

这个命令的作用就是：当执行npm install的时候，会自动执行npm run postinstall这个命令，也就是执行patch-package，这时候就会去读取上面说的 patches目录，并将那些补丁打到对应的包里，达到同步修改代码的效果！！

原文链接：https://blog.csdn.net/leal_ch/article/details/126855790