---
title: 常用的Bash命令四-系统管理
top: false
cover: false
date: 2019-12-22 16:40:54
password:
summary: 常用的Bash命令-系统管理,有top netstat等
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

## 2、ps - 输出进程运行情况
`ps` 用于显示当前进程 (process) 的状态。
```yaml
# ps 的参数非常多, 在此仅列出几个常用的参数并大略介绍含义
-A 列出所有的行程信息
-u root //显示root进程用户信息
-ef //显示所有命令，连带命令行
PID: pid
-aux 显示所有包含其他使用者的行程(这里会比-ef 获得的资讯更详细)
```

## 3、lsof/netstat - 查看端口占用情况
`lsof`(list open files)是一个列出当前系统打开文件的工具。
`netstat` -tunlp 用于显示 tcp，udp 的端口和进程等相关情况。
```yaml
lsof

-i:port 查看服务器 port 端口的占用情况
abc.txt：显示开启文件abc.txt的进程
-c -p 1234：列出进程号为1234的进程所打开的文件
```
---

```yaml
netstat

-tunlp | grep 端口号
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

## 4、kill - 杀掉对应的进程
```yaml
# 查到端口占用的进程后，如果你要杀掉对应的进程可以使用 kill 命令
kill -9 PID
如:
kill -9 26993 强制杀死端口号为26993的进程
```