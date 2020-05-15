## PHP 安装

> 工具准备：Linux服务器及shell终端

yum install 略。

这里，我们使用[php的官方文档]( http://php.net/manual/en/install.php )进行编辑安装，按照提示首先选择[版本]( https://www.php.net/downloads ) 这里，我们选择7.3.17

<img src="C:\Users\W9004022\Desktop\归档资料\PHP7.3.17.png" alt="PHP7.3.17" style="zoom:75%;" />

* 下载php安装包
`wget  https://www.php.net/distributions/php-7.3.17.tar.gz`
* 解压
`tar -zxvf php-7.3.17.tar.gz`
* 安装前准备工具包
`yum install gcc gcc++ libxml2-devel`
* 进入目录，编译
  `cd php-7.3.17 && ./configure --prefix = / usr / local / php --with-curl = / usr / local / curl --with-freetype-dir --with-gd --with-gettext --with-iconv-dir- -with-kerberos --with-libdir = lib64 --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite- -with-pear --with-png-dir --with-xmlrpc --with-xsl --with-zlib --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization- enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-xml --enable-zip`
  注意：

  --prefix =安装目录
  --with-使用包名称[=包目录]
  --enable-需要激活的功能

* 安装
  `make install && make`
  
* 配置
````
1) 配置php.ini，这是php的配置文件：cp /home/damon/php-7.3.17/php.ini-development /usr/local/php/lib/php.ini

2) 配置php-fpm.conf，这是php-fpm配置文件：cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf

3)配置www.conf，配置用户的文件：cp etc/php-fpm.d/www.conf.default etc/php-fpm.d/www.conf

4)将php-fpm启动文件复制到init.d文件夹中一个方便启动php：cp -R sbin/php-fpm /etc/init.d/php-fpm
````

* 启动

  ` 执行命令：/etc/init.d/php-fpm ，通过 ps -ef|grep php`

## 配置Nginx，支持php

> 配置nginx，略。

1)  https://zhuanlan.zhihu.com/p/98990924 