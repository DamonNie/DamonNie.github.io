---
title: 让PHP更快的提供文件下载
top: false
cover: false
date: 2020-06-14 11:31:19
password:
summary: php提供文件下载也是一门技术活。
tags:
categories:
---

# 让PHP更快的提供文件下载

一般来说, 我们可以通过直接让URL指向一个位于Document Root下面的文件, 来引导用户下载文件.

但是, 这样做, 就没办法做一些统计, 权限检查, 等等的工作.  于是,  很多时候,  我们采用让PHP来做转发, 为用户提供文件下载.
```html
$file = "/tmp/dummy.tar.gz";
header("Content-type: application/octet-stream");
header("Content-Disposition: attachment; filename="" . basename($file) . """);
header("Content-Length: ". filesize($file));
readfile($file);
```
但是这个有一个问题, 就是如果文件是中文名的话, 有的用户可能下载后的文件名是乱码.

于是, 我们做一下修改(参考: :
```html
$file = "/tmp/中文名.tar.gz";
$filename = basename($file);
header("Content-type: application/octet-stream");
//处理中文文件名
$ua = $_SERVER["HTTP_USER_AGENT"];
$encoded_filename = rawurlencode($filename);
if (preg_match("/MSIE/", $ua)) {
header("Content-Disposition: attachment; filename="" . $encoded_filename . """);
} else if (preg_match("/Firefox/", $ua)) {
header("Content-Disposition: attachment; filename*=\"utf8""" . $filename . """);
} else {
header("Content-Disposition: attachment; filename="" . $filename . """);
}
header("Content-Length: ". filesize($file));
readfile($file);
```
恩, 现在看起来好多了, 不过还有一个问题,  那就是readfile,  虽然PHP的readfile尝试实现的尽量高效, 不占用PHP本身的内存, 但是实际上它还是需要采用MMAP(如果支持), 或者是一个固定的buffer去循环读取文件, 直接输出.

输出的时候, 如果是Apache + PHP mod, 那么还需要发送到Apache的输出缓冲区. 最后才发送给用户. 而对于Nginx + fpm如果他们分开部署的话, 那还会带来额外的网络IO.

那么, 能不能不经过PHP这层, 直接让Webserver直接把文件发送给用户呢?

今天, 我看到了一个有意思的文章:[How I PHP: X-SendFile](http://www.jasny.net/articles/how-i-php-x-sendfile/ "").

我们可以使用Apache的module[mod_xsendfile](https://tn123.org/mod_xsendfile/ ""), 让Apache直接发送这个文件给用户:
```html
$file = "/tmp/中文名.tar.gz";
$filename = basename($file);
header("Content-type: application/octet-stream");
//处理中文文件名
$ua = $_SERVER["HTTP_USER_AGENT"];
$encoded_filename = rawurlencode($filename);
if (preg_match("/MSIE/", $ua)) {
header("Content-Disposition: attachment; filename="" . $encoded_filename . """);
} else if (preg_match("/Firefox/", $ua)) {
header("Content-Disposition: attachment; filename*=\"utf8""" . $filename . """);
} else {
header("Content-Disposition: attachment; filename="" . $filename . """);
}
//让Xsendfile发送文件
header("X-Sendfile: $file");
```
X-Sendfile头将被Apache处理, 并且把响应的文件直接发送给Client.

Lighttpd和Nginx也有类似的模块,  大家有兴趣的可以去找找看
Filed in[PHP应用](https://www.laruence.com/category/php-usage "title="View all posts in PHP应用"")				*[PHP](https://www.laruence.com/tag/php "title="View all posts tagged PHP"")
*[Xsendfile](https://www.laruence.com/tag/xsendfile "title="View all posts tagged Xsendfile"")
*[下载文件](https://www.laruence.com/tag/%e4%b8%8b%e8%bd%bd%e6%96%87%e4%bb%b6 "title="View all posts tagged 下载文件"")
*[中文文件名](https://www.laruence.com/tag/%e4%b8%ad%e6%96%87%e6%96%87%e4%bb%b6%e5%90%8d "title="View all posts tagged 中文文件名"")

转自:鸟哥的[让PHP更快的提供文件下载]())
