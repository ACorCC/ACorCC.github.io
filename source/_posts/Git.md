---
title: Git
date: 2023-01-07 13:43:05
tags:
categories:
- [前端]
---
# Proxy
## 设置全局代理

```
// Clash
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890

git config --global http.proxy 127.0.0.1:10809
git config --global https.proxy 127.0.0.1:10809

git config --global http.proxy 127.0.0.1:4919
git config --global https.proxy 127.0.0.1:4919
```

## 清除代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

