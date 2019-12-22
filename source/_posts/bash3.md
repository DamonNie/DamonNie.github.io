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
## 3、grep - 查找字符串
`grep` 搜索文本或指定的文件中与指定的字符串或模式相匹配的行。
```yaml
# `grep`命令查找文件/etc/passwd 中帐号 damon 的信息
grep damon /etc/passwd

-i: 忽略搜索关键字的大小写
-r: 递归搜索指定目录下的所有文件
-w: 只匹配包含指定单词的行
-c: 报告文件或文本中模式被匹配的次数
-n: 显示每一个匹配的行的行号
-v: 输出除匹配指定模式的行以外的其他所有行
--color: 输出将匹配的字符串以彩色的形式标出
```
## 4、diff - 查找字符串
`diff` 命令用于比较两个文件，并找出它们之间的不同。
```yaml
# 比较两个文件
diff nsswitch.conf nsswitch.conf.org

-w: 忽略空格
-y: 以并排的格式输出两个文件的比较结果（这个比较实用些）
-c: 上下对比的格式输出两个文件的比较结果
```
## 5、cat - 显示文件内容
`cat` 查看文件的内容、连接文件、创建一个或多个文件和重定向输出到终端或文件。
```yaml
# 使用 cat 命令查看文件 /etc/group 的内容
cat /etc/group

-n: 显示行号
-b: 只显示非空白行的行号
-e: 在每一行的结尾显示“$”字符
```

## 6、less、more - 分屏显示文件
`more` 用于一次翻阅一整屏文件的过滤器。
`less` 与'more'命令类似，但是支持向前和向后翻页。

```yaml
# 查看一个文件，自动清空屏幕并显示文件开头部分
more /etc/inittab

-num: 指定一次显示num行

> less常用命令参数如下：
-f  强迫打开特殊文件，例如外围设备代号、目录和二进制文件
-i  忽略搜索时的大小写
-N  显示每行的行号
-Q  不使用警告音
-s  显示连续空行为一行
/字符串：向下搜索“字符串”的功能
?字符串：向上搜索“字符串”的功能
n： 重复前一个搜索（与 / 或 ? 有关）
u  向前滚动半页
y  向前滚动一行
d  向后翻半页
b  向后翻一页
h  显示帮助界面
Q  退出less 命令
空格键 滚动一行
回车键 滚动一页
```
## 7、tail、head 显示文件头部、尾部
```yaml
# 指定打印文件的前/后5行
-n 5 <file>
-5 <file>
#  打印文件的前/后N个字节的数据
-c N
# 及时跟新打印的文件数据(适用于tail)
-f
-f --retry 稍后才会创建
```

## 8、wc - 查看文件统计信息
`wc`命令用于查看文件的行数、单词数和字符数等信息。
```yaml
# 其中X表示行数，Y表示单词数，Z表示字节数，filename表示文件名。
wc filename
X Y Z /etc/inittab

-l 只统计文件的行数信息(行数)
-w 只统计文件的单词数信息(单词个数)
-c 只统计文件的字节数信息(字节byte数)
-L 只统计文件中最长的行的长度
```

## 9、find - 查找文件或目录
```yaml
以下列出常用的几种场景命令：
# 在当前目录下查找名称为 inittab 的文件
find . -name inittab

# 找出 /tmp 目录下，所有者是 root 的文件和目录
find /tmp/ -user root

# 找出你账号的主目录下，30 天以前修改的所有文件
find ~ -type f -mtime +30

# 找出你账号的主目录下，3 天以内修改的所有文件
find ~ -type f -mtime -3

# 找出你账号主目录下，大小是50MB的所有文件
find ~ -type f -size 50MB

# 找出你账号主目录下，大于50MB小于100MB的所有文件
find ~ -type f -size +50MB -size -100MB
```

## 10、awk - 查找文件
```yaml
用法一:
awk '{[pattern] action}' {filenames}   # 行匹配语句 awk '' 只能用单引号
# 每行按空格或TAB分割，输出文本中的1、4项
awk '{print $1,$4}' log.txt

用法二:
awk -F  #-F相当于内置变量FS, 指定分割字符

用法三:
awk -v  # 设置变量
实例：

用法四:
awk -f {awk脚本} {文件名}
实例：awk -f cal.awk log.txt
```
### 进阶篇：
查询一个日志文件中访问次数最多前10个IP？

第一步：按照IP进行将记录排序。

第二步：按照IP去重，并且显示重复次数

第三步：按照次数升序排列

第四步：显示前10行

awk '{print $1}' log.txt | sort | uniq -c | sort -n -r | head -n 10



参考：[常用Bash命令整理之文本处理](https://blinkfox.github.io/2018/10/11/ruan-jian-gong-ju/linux/chang-yong-bash-ming-ling-zheng-li-zhi-wen-ben-chu-li/)[常用Bash命令整理之查看文件和目录](https://blinkfox.github.io/2018/10/09/ruan-jian-gong-ju/linux/chang-yong-bash-ming-ling-zheng-li-zhi-cha-kan-wen-jian-he-mu-lu/)
