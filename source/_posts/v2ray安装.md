---
title: V2Ray 搭建教程以及一键安装脚本
top: false
cover: false
date: 2020-04-11 15:38:01
password:
summary: V2Ray 搭建教程以及一键安装脚本
tags:
 - vpn
categories: 互联网
---

## V2Ray 搭建教程以及一键安装脚本
> V2Ray 是一个网络转发程序，支持 TCP、mKCP、WebSocket 这3种底层传输协议，支持 HTTP、Socks、Shadowsocks、VMess 这4种内容传输协议（HTTP 只支持传入），并且有完整的 TLS 实现，是一个非常强大的平台。

### 主要特性
* 多入口多出口: 一个`V2Ray` 进程可并发支持多个入站和出站协议，每个协议可独立工作。
* 可定制化路由: 入站流量可按配置由不同的出口发出。轻松实现按区域或按域名分流，以达到最优的网络性能。
* 多协议支持:`V2Ray` 可同时开启多个协议支持，包括`Socks`、`HTTP`、`Shadowsocks`、`VMess` 等。每个协议可单独设置传输载体，比如`TCP`、`mKCP`、`WebSocket` 等。
* 隐蔽性: V2Ray 的节点可以伪装成正常的网站（HTTPS），将其流量与正常的网页流量混淆，以避开第三方干扰。
* 反向代理: 通用的反向代理支持，可实现内网穿透功能。
* 多平台支持: 原生支持所有常见平台，如 Windows、Mac OS、Linux，并已有第三方支持移动平台。
### 搭建v2ray的步骤大体如下：

* 购买一个VPS，购买后你会获得VPS的IP、root用户及密码、SSH端口等信息；
* 登录VPS，可以借助Xshell这个工具；
* 安装v2ray，使用xshell成功登录VPS后，开始搭建；
* 在你的设备上配置与VPS对应的v2ray信息，就可以成功使用了；

一、环境信息
服务器系统：Digitalocean CentOS 7.6 x64 ；
v2ray版本：v4.20.0
客户端系统：Windows 10
VPS
二、福利
1.搬瓦工(Bandwagon Host)

新用户通过我的推广链接(https://bandwagonhost.com/aff.php?aff=47655 )，然后在此页面获取最新优惠券：获取搬瓦工优惠码 ，可以折扣价购买。

2.Vultr抵用券

通过此【推广链接】注册Vultr，即可获得$100，目前搬瓦工缺货严重，如果搬瓦工没货推荐Vultr东京机房。
三、搭建v2ray
为了简化安装，我们使用一键安装脚本安装:
````sh
yum update -y && yum install curl -y
bash <(curl -s -L https://git.io/v2ray.sh)
````
按照提示操作，这里我都是选择默认选项。安装成功后，会有v2Ray信息，可以记录下来或者命令查看详细资料。
四、管理 V2Ray
*`v2ray info`：查看 V2Ray 配置信息
*`v2ray config`：修改 V2Ray 配置
*`v2ray link`：生成 V2Ray 配置文件链接
*`v2ray infolink`：生成 V2Ray 配置信息链接
*`v2ray qr`：生成 V2Ray 配置二维码链接
*`v2ray ss`：修改 Shadowsocks 配置
*`v2ray ssinfo`：查看 Shadowsocks 配置信息
*`v2ray ssqr`：生成 Shadowsocks 配置二维码链接
*`v2ray status`：查看 V2Ray 运行状态
*`v2ray start`：启动 V2Ray
*`v2ray stop`：停止 V2Ray
*`v2ray restart`重启 V2Ray
*`v2ray log`：查看 V2Ray 运行日志
*`v2ray update`：更新 V2Ray
*`v2ray update.sh`：更新 V2Ray 管理脚本
*`v2ray uninstall`：卸载 V2Ray

五、Windows 客户端
1.下载

这里使用的是Windows图形界面v2rayN，当前版本(2020年2月9日)是3.5版本，下载地址：[Github]("https://github.com/2dust/v2rayN/releases")

注： 如果你用的是旧版本的v2rayN,可能还需要下载 v2ray-windows-64.zip ，并将v2ray-windows-64.zip和v2rayN.zip解压到同一文件夹。

然后运行V2RayN.exe即可。

2.配置

运行`V2RayN.exe`，然后进行配置，下图中的配置信息，需要和你VPS搭建的时候的配置信息对应，VPS的v2ray配置信息位于`/etc/v2ray/config.json`文件里。

如果采用上面的默认方式安装，服务端配置是协议vmess，则配置如下：

![](https://img.snailshub.com/images/2019/02/10/new-vmess-config.jpg)
![](https://img.snailshub.com/images/2019/02/10/vmess-windows-client.jpg)
![](https://img.snailshub.com/images/2019/02/10/v2ray-pac-config.jpg)
五、测试
打开浏览器，访问www.google.com