---
title: 常用的Bash命令(一)
top: false
cover: false
date: 2019-12-08 22:37:02
password:
summary:
tags: 
 - linux
categories: linux
---

## 1、hostname - 查看主机名
`hostname` 命令用于查看系统的主机名，亦或修改系统的主机名
`hostname` 的常用命令如下:
```yaml
# 显示系统的当前主机名
hostname

#修改系统的主机名
hostname damon-system
```

## 2、uptime - 查看系统运行时间

```yaml
uptime
```

## 3、who - 列出登录的用户
```yaml
# 显示当前登录的所有用户信息
who

# 显示系统的启动时间
who -b

# 显示系统登录进程
who -l

# 显示当前登录用户关联的用户信息
who -m

# 显示所有登录用户的用户名和用户数
who -q
```

## 4、uname - 查看系统信息
     
`uname`命令用于打印内核名称和版本、主机名等系统信息。命令的语法如下所示：
 ```yaml
 # 只打印内核的名称
 uname
 
 # 使用 -n 选项，只打印系统的主机名
 uname -n
 
 # 使用 -r 选项，打印内核版本信息
 uname -r
 
 # 使用 -m 选项，打印系统的硬件名称
 uname -m
 
 # 使用 -p 选项，打印系统的处理器类型信息
 uname -p
 
 # 使用 -i 选项，打印系统的硬件平台信息
 uname -i
 
 # 使用 -a 选项，打印上述所有示例中的信息
 uname -a
 ```
 ## 5、date - 显示系统时间
 ```yaml
# 以默认格式显示系统的当前日期时间
date

# 格式化当前日期
date +"%Y-%m-%d"
```
## 6、 id - 显示用户属性
```yaml
# 输出当前用户的uid、用户名、gid、组名及用户属于的群组信息
id

# 使用 -u 选项，输出用户的 uid
id -u

#-u 选项和 -n 选项结合使用，输出账户的用户名
id -un
```
引用：[常用Bash命令整理之其他常用命令](https://blinkfox.github.io/2018/10/13/ruan-jian-gong-ju/linux/chang-yong-bash-ming-ling-zheng-li-zhi-qi-ta-chang-yong-ming-ling/)