---
title: 常用的Bash命令(三) 文本处理
top: false
cover: false
date: 2019-12-10 19:26:18
password:
summary:
tags: 
 - linux
categories: linux
---

## 1、sort - 排序文件
`sort` 命令用于将文本文件的行排序。默认情况下，sort命令是按照字符串的字母顺序排序。
```yaml
# 将文本内容按字母顺序排序
sort example.txt
-u: 移除所有重复行后排序 
-n: 按照数字按数值的大小排序
-r: 倒序方式排序
```
## 2、uniq - 文本去重
`uniq` 命令用于移除或发现文件中重复的条目。
```yaml
# 它将移除文件中重复的行并显示单一行
uniq example.txt

-c: 统计重复行出现的次数
-d: 只显示文件中有重复的行并只显示一次
-D: 显示文件中所有重复的行
-u: 只显示文件中不重复的行
```
## 2、uniq - 文本去重
`uniq` 命令用于移除或发现文件中重复的条目。
```yaml
# 它将移除文件中重复的行并显示单一行
uniq example.txt

-c: 统计重复行出现的次数
-d: 只显示文件中有重复的行并只显示一次
-D: 显示文件中所有重复的行
-u: 只显示文件中不重复的行
```

