---
title: 对Composer的认识
top: false
cover: false
date: 2019-12-12 20:50:00
password:
summary: Composer三步曲：安装、使用、发布
tags:
 - composer
categories: 编程之道
---

![composer](/medias/tmp/composer.jpg)
引自Composer中文网的描述：
> 是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。
我们今天就来认识以下`Composer`
## 组件
在说composer之前，我们先来了解一下什么组件。因为组件和composer之间密切相关，要想知道composer是如何工作的，我们要先清楚什么是组件。
---
### 什么是组件
组件是打包的代码，用于我们在实际项目中解决某个问题。组件适用于某个场景下使用，比如：在导入导出时候会碰到各种不同业务场景下的功能需求，直接调用excel组件的类库代码，便可自定义使用，十分方便
### 为什么要使用组件
避免重复造轮子
### 组件的特点
* 功能单一。一个方法只做一件事
* 专属的命名空间，遵循组件库的规范，不与其他组件冲突
* 开箱即用。测试方便
* 文档完善。

## Composer
通过`composer`，我们可以使用大量的第三方库，而无需自己造轮子,更加快速提高开发效率。如果说Packagist是组件库，那么composer就是PHP组件管理工具了。composer是PHP组件的依赖管理器，它在命令行中使用，简单而又方便。所以，推荐phper必须学会composer。
---

### 安装
首先确认已经安装了php，并且把php设置在了全局变量中，打开命令行输入php -v能够看到php版本信息
```php
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"

php composer-setup.php

php -r "unlink('composer-setup.php');"
```
如何全局安装请看:
![](/medias/tmp/composer-1.png)

### 使用
以下以`monolog`为例
>声明依赖
在项目目录下创建一个composer.json文件，指明依赖，比如，你的项目依赖 `monolog`：
```
{
    "require": {
        "monolog/monolog": "1.2.*"
    }
}
```
如果不需要使用https，可以这么写，以解决有时候因为https造成的问题
```
{
    "require": {
        "monolog/monolog": "1.2.*"
    },
    "config": {
        "secure-http": false
    }
}
```
安装依赖
安装依赖非常简单，只需在项目目录下运行：
`composer install`
如果没有全局安装的话，则运行：
`php composer.phar install`
更新全部的包（谨慎使用）：
`composer update`

*注意：使用`composer install`或者`composer update`命令将会更新所有的扩展包，项目中使用需谨慎！！！*

### 命令汇总
```
composer list  列出所有可用的命令
composer init   初始化composer.json文件(就不劳我们自己费力创建啦)，会要求输入一些信息来描述我们当前的项目，还会要求输入依赖包
composer install  读取composer.json内容，解析依赖关系，安装依赖包到vendor目录下
composer update   更新最新的依赖关系到compsoer.lock文件，解析最新的依赖关系并且写入composer.lock文件
composer search packagename 搜索包，packagename替换为你想查找的包名称
composer require packagename 添加对packagename的依赖，packagename可修改为你想要的包名称
composer show packagename
composer self-update 更新 composer.phar文件自身
composer dump-autoload --optimize 优化一下自动加载

composer command --help 以上所有命令都可以添加 --help选项查看帮助信息
```

## 参考
1、利用 Composer 一步一步构建自己的 PHP 框架（一）——基础准备 - 岁寒
https://lvwenhan.com/php/405.html

2、PHP 开发者该知道的 5 个 Composer 小技巧 - 新闻 - SegmentFault
https://segmentfault.com/a/1190000000355928

3、Composer 中文网
http://www.phpcomposer.com/

4、Packagist / Composer 中国全量镜像
http://pkg.phpcomposer.com/

5、Composer安装
https://getcomposer.org/download/

6、composer之创建自己的包 - 始终不够
http://www.huyanping.cn/composer%e4%b9%8b%e5%88%9b%e5%bb%ba%e8%87%aa%e5%b7%b1%e7%9a%84%e5%8c%85/

7、php - 请各位分享或推荐一下composer里面好用的包 - SegmentFault
https://segmentfault.com/q/1010000000484379

8、给 CI 插上翅膀——在 CodeIgniter 2 中使用 Laravel Eloquent ORM - 岁寒
https://lvwenhan.com/php/414.html

9、Composer进阶使用 —— 常用命令和版本约束 - icyfire - SegmentFault
https://segmentfault.com/a/1190000005898222

10、Composer 国内加速：可用镜像列表 | Composer 技术论坛
https://learnku.com/composer/wikis/30594