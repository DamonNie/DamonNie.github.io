---
title: 常用的Bash命令四-系统管理
top: false
cover: false
date: 2019-12-22 16:40:54
password:
summary:
tags: 
 - linux
categories: linux
---

## 1、top - 显示 process 的动态
`top` 显示进程信息。
```yaml
# 显示进程信息
top

-c 显示完整命令
-b 批处理模式显示
-S 累计模式显示程序信息
-n 2 设置信息更新次数 //表示更新两次后退出
-d 3 设置信息更新时间 //表示更新周期为3秒
-p 139 显示指定的进程信息 //显示进程号为139的进程信息，CPU、内存占用率等

```