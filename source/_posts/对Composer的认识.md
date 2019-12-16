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

## 依赖
>硬依赖性：
```
{  
    "require": {  
        "acme/foo": "^1.0"  
    }  
}
```
>可选的依赖关系:
```
{  
    "suggest": {  
        "monolog/monolog": "Advanced logging library",  
        "ext-xml": "Required to support XML"  
    }  
}
```
>开发依赖:
```
{  
    "require-dev": {  
      "monolog/monolog": "^1.0",  
      "phpunit/phpunit": "^6.0"  
    }  
}
```
截止到目前还是很顺利。那么什么地方会出错呢? 主要在 require-dev 上会有一定的限制。

### 过多依赖`Composer`的一些弊端
* 安全问题:
使用包管理器解决依赖是非常好的。这种方式可以很好的更新和重用代码。但是，你得对你引入了哪些包、多少包负责。
* 兼容：
添加越多的依赖就越可能出现冲突。

### 解决问题
>*PHARs*
PHARs (PHP 档案)是将应用程序打包为单一文件的一种方法。平时使用不多，故不做过多描述，如果您想了解更多，建议您查阅 PHP官方网站。且难以追踪，因为在`Composer`中没有原生支持

>使用多个存储库
我们不需要在一个 composer.json 中要求所有的桥依赖关系，而是将这个包分解到多个存储库中。比如对于一个依赖确有多个版本acme/foo，那么我们将为Symfony创建另一个包 acme/foo-bundle，为Laravel创建 acme/foo-provider

>使用多个composer.json
这种方法相对来说是比较新的（在 PHP 中），主要是因为所需的工具不是现成的,例如:
```
    {  
        "autoload": {...},  
        "autoload-dev": {...},
    
    
        "require": {...},  
        "require-dev": {  
            "phpunit/phpunit": "^6.0",  
            "phpstan/phpstan": "^1.0@dev",  
            "phpmetrics/phpmetrics": "^2.0@dev"  
        }  
    }    
```
我们将安装 phpstan/phpstan 和 phpmetrics/phpmetrics 使用不同的 composer.json 文件。但是这首先会有一个疑问：我们把它们放在哪里？采用哪种结构？
[composer-bin-plugin](https://github.com/bamarni/composer-bin-plugin) 便应运而生。一个非常简单的 Composer 插件，它允许您以不同的目录与一个 composer.json 进行交互。因此我们先假设我们有一个 composer.json 根文件
```
    {  
        "autoload": {...},  
        "autoload-dev": {...},
    
    
        "require": {...},  
        "require-dev": {  
            "phpunit/phpunit": "^6.0"  
        }  
    }

```
安装插件:
   ` $ composer require --dev bamarni/composer-bin-plugin`
现在插件已经安装好了，每当您执行 composer bin acme smth ，它就会在子目录 vendor-bin / acme 中执行composer smth命令。 所以我们现在可以像这样安装PhpStan和PhpMetrics：
```
 $ composer bin phpstan require phpstan/phpstan:^1.0@dev  
 $ composer bin phpmetrics require phpmetrics/phpmetrics:^2.0@dev
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