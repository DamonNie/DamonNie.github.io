---
title: 常用的Bash命令(二) 文件管理
top: false
cover: false
date: 2019-12-08 22:37:02
password:
summary:
tags: 
 - linux
categories: linux
---

## 1、touch - 创建文件
`touch` 命令就可用于创建、变更和修改文件的时间戳。
```yaml
-a: 只改变访问时间 
-c: 不创建任何文件
-m: 只改变修改时间
-r: 使用指定文件的时间替代当前时间
-t: 使用 [[CC]YY]MMDDhhmm[.ss] 替代当前时间
```

## 2、mkdir - 创建目录
`mkdir` 创建文件目录。
```yaml
# 在当前目录下创建一个给定的目录名
mkdir <dirname>

# 使用 -p 选项，会自动创建所有还不存在的父目录
mkdir -p backup/old
```

## 3、cp - 复制文件或目录
`cp` 命令用于将文件从一个地方复制到另一个地方。
示例如下:
```yaml
# 在当前目录下，创建一个文件 file.txt 的副本，取名为 newfile.txt
cp file.txt newfile.txt

# 复制当前目录下的 file.txt 文件到 /tmp 目录下
cp file.txt /tmp

# 使用 -R 或 -r 选项，递归地复制一个目录
# 即将一个目录及其下的所有文件和子目录都复制到另一个目录
cp -R * /home/blinkfox/backup
```

## 4、scp - 远程复制文件或目录     
`scp`远程复制某服务器的一个文件到另一个服务器。命令的语法如下所示：
 ```yaml
-r: 递归地复制一个目录
-P: 设置命令的端口号

 # 从本地复制到远程(第二个指定了文件名)
 scp local_file remote_username@remote_ip:remote_folder 
 或者 
 scp local_file remote_username@remote_ip:remote_file 
 
 # 从远程复制到本地(...)
 scp remote_username@remote_ip:remote_folder local_folder
 或者 
 scp remote_username@remote_ip:remote_file remote_file
 
# 远程服务器防火墙有为scp命令设置了指定的端口
#scp 命令使用端口号 4588
 scp -P 4588 remote@www.runoob.com:/usr/local/sin.sh /home/administrator
 ```
 ## 5、mv - 移动文件或目录
 `mv`命令用于将文件和目录从一个位置移到另外一个位置。命令的语法如下所示：
 ```yaml
# 将当前目录下的文件 source.txt 移到目录 /tmp 下
mv source.txt /tmp

# 将目录 dir1、dir2 移到目录 dir_dist 下
mv dir1 dir2 dir_dist

# 将当前目录下的 old.txt 文件更名为 new.txt
mv old.txt new.txt
```
## 6、 ln - 链接文件或目录
`ln`命令用于创建软链接或硬链接。使用 -s 选项，可以创建一个软链接：
```yaml
# 在目录 lib 下创建一个软链接 lib.so，链接到 /home/damon/src/library.so
ln -s /home/damon/src/lib.so /home/damon/lib

# 创建目录的软链接
ln -s /home/damon/src source

## 7、 rm - 删除文件或目录
`rm`删除文件或目录。示例如下：
```yaml
# 删除当前目录下的文件 file1.txt、file2.txt、file3.txt
rm file1.txt file2.txt file3.txt

# 使用 -i 选项，可以在删除每个文件或目录前提示用户确认
rm -i *

# 删除当前目录下所有以".doc"结尾的文件
rm *.doc

# 删除 /tmp 目录下的所有文件及其子目录(慎用)
rm -rf /tmp/*
```

参考：[常用Bash命令整理之操作文件和目录]（https://blinkfox.github.io/2018/10/10/ruan-jian-gong-ju/linux/chang-yong-bash-ming-ling-zheng-li-zhi-cao-zuo-wen-jian-he-mu-lu/）
