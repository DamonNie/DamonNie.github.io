## PHP 安装

## 下载`PHP 7`源码

 直奔 `PHP` 的官方网站： [php.net](http://php.net/) 

在下面，我们可以看到：

- php-7.1.1.tar.bz2 (sig) [15,405Kb] 19 Jan 2017
- php-7.1.1.tar.gz (sig) [19,800Kb] 19 Jan 2017
- php-7.1.1.tar.xz (sig) [12,467Kb] 19 Jan 2017

 用你的 `wget` 或者 `curl` 去下载吧。任选一个命令 

```
wget http://cn2.php.net/get/php-7.1.1.tar.bz2/from/this/mirror
curl http://cn2.php.net/get/php-7.1.1.tar.bz2/from/this/mirror -o php-7.1.1.tar.bz2
```

## 安装必要的库

 熟悉编译安装软件的老司机肯定记得，在 `./configure` 之前一定要装些什么。 

不然有可能会碰到下面的话：

```
configure: error: xml2-config not found. Please check your libxml2 installation.
```

我们这里使用`yum`安装

```
`yum -y install gcc g++ libxml2 libxml2-devel openssl openssl-devel bzip2 bzip2-devel libcurl libcurl-devel libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel gmp gmp-devel libmcrypt libmcrypt-devel readline readline-devel libxslt libxslt-devel`
```

## 编译PHP

> 这里需要注意的是enable-gd-native-ttf 新版本里面已经废弃，
> 同时，旧版本里
> --with-freetype-dir \
> --with-jpeg-dir\
> --with-png-dir \
> 改为
> --with-freetype \
> --with-jpeg \
> --with-png \

```
./configure \
--prefix=/usr/local/php \
--with-config-file-path=/etc \
--enable-fpm \
--enable-inline-optimization \
--disable-debug \
--disable-rpath \
--enable-shared  \
--enable-soap \
--with-libxml-dir \
--with-xmlrpc \
--with-openssl \
--with-mcrypt \
--with-mhash \
--with-pcre-regex \
--with-sqlite3 \
--with-zlib \
--enable-bcmath \
--with-iconv \
--with-bz2 \
--enable-calendar \
--with-curl \
--with-cdb \
--enable-dom \
--enable-exif \
--enable-fileinfo \
--enable-filter \
--with-pcre-dir \
--enable-ftp \
--with-gd \
--with-openssl-dir \
--with-jpeg \
--with-png \
--with-zlib-dir  \
--with-freetype-dir \
--enable-gd-jis-conv \
--with-gettext \
--with-gmp \
--with-mhash \
--enable-json \
--enable-mbstring \
--enable-mbregex \
--enable-mbregex-backtrack \
--with-libmbfl \
--with-onig \
--enable-pdo \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-zlib-dir \
--with-pdo-sqlite \
--with-readline \
--enable-session \
--enable-shmop \
--enable-simplexml \
--enable-sockets  \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--enable-wddx \
--with-libxml-dir \
--with-xsl \
--enable-zip \
--enable-mysqlnd-compression-support \
--with-pear \
--enable-opcache
```

## 安装`PHP 7`

> 一行搞定

```
make && make install
```
## 配置 PHP
```
vi /etc/profile
```

在文件的最末尾填上：

```
PATH=$PATH:/usr/local/php/bin
export PATH
```

保存修改，然后执行 `source /etc/profile` 使环境变量配置生效。

如果某个用户想要定义自己的命令别名，可以将命令添加到当前目录中的文件.bash_profile中。

```
echo "alias cdt='cd /APP/isTester.com'">>~/.bash_profile
```



## 安装 `PHP-FPM`

```
cp sapi/fpm/init.d.php-fpm /usr/local/bin/php-fpm
chmod +x /usr/local/bin/php-fpm
```

## 初始化 `PHP` 和 `PHP-FPM` 的配置

```
cp php.ini-production /etc/php.ini
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```

### 严格控制权限管理

*   假设我们用 `website` 用户组管控网站,`site-www`账号来管理，那就先用 `groupadd website` 创建好这个用户组。

  然后就创建用户 `site-www`： `adduser -g website site-www`

*  还有一步就是 `vi /etc/passwd` 把 `site-www` 的 `bash` 改回 `/sbin/nologin` 来防止这个用户被登录。 

* 修改` /usr/local/php/etc/php-fpm.d/www.conf ` ，原内容如下 

```
; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;  will be used.
user = nginx
group = nginx
```

- `user` 改成 `site-www`
- `group` 改成 `website`

### 开机自启动 `PHP-FPM`

先添加服务：

```
vim /etc/systemd/system/php-fpm.service
```

内容加上：

```
[Unit]
Description=The PHP FastCGI Process Manager
After=syslog.target network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/php-fpm start
ExecReload=/usr/local/bin/php-fpm reload
ExecStop=/usr/local/bin/php-fpm stop

[Install]
WantedBy=multi-user.target
```

保存服务配置之后，设置开机自启动：

```
systemctl enable php-fpm.service
```

