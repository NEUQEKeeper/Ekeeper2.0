---
title: AX6S刷入openwrt记录
date: 2023-5-29 18:18:18
author: 研酱哒哟
description: 被导师叫去当工具人了捏🥺
categories:
  - 教程
tags:
  - 网络
---

# AX6S刷入openwrt启用mentohust记录

## 前言

被导师叫去帮忙装`mentohust`，准备好了`ipk`文件空手就过去了，结果被告知路由器是新的，没刷过`openwrt`😭

于是把路由器借了回来刷一波，好在这个型号网上教程很全，特此记录。

## 为什么要mentohust

mentohust可以在路由器登录校园网，这样路由器就可以把网络分享出去了，通过连接wifi，直接访问互联网，并且可以访问校内内网资源。以前校园网账户有包月策略时，我们用它24小时薅校园网，现在本科生只给免费5G了，所以这种共享互联网的方式可能只适合老师或者研究生了。

## 路由器型号

`Redmi AX6S`

## Step 1： 换开发版

### 准备

1. 连接WiFi，登录网关进行初始化

2. 密码设置为：`Passw0rd`

3. 下载内测固件：AX6S开发版固件1.2.7：[https://wwp.lanzoul.com/ib8UK0fofpdi](https://wwp.lanzoul.com/ib8UK0fofpdi) （密码:5v1s）

### 开刷

重新连接设置好的ssid

再次进入网关后台，在常用设置选择手动升级

![image-20230529163635126](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-9d2cc3-image-20230529163635126.png)

选择那个miwifi开头的bin file，点击升级

![image-20230529164217182](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-fbdfe0-2023-05-29-161c6d-image-20230529164217182.png)

等待路由器亮起蓝灯

重新进入管理界面，若rom版本变为开发版，则成功。

![image-20230529164625023](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-549c2e-2023-05-29-ed0a26-image-20230529164625023.png)

## Step2：获取SSH密码

### 准备

SSH密码获取网站：[https://www.oxygen7.cn/miwifi](https://www.oxygen7.cn/miwifi)

备用：[https://miwifi.dev/ssh](https://miwifi.dev/ssh)

### 开整

进入https://miwifi.dev/ssh

输入SN，计算出SSH的密码

![image-20230529164805498](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-8d283a-image-20230529164805498.png)

我这里算出来的是：`4a10224d`

## Step3：刷入底包

### 准备

[ax6s-openwrt固件.zip（密码:38o0）](https://wwp.lanzoul.com/iIFgB0fofhzc)

解压，得到三个bin文件：

- `factory.bin`为底包
- `ax6s-full.bin`为OpenWrt高大全版
- `ax6s-mini.bin`为OpenWrt精简版

### 使用telnet登录路由器

ip填路由器ip，用户名`root`

![image-20230529165650804](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-bdaa70-image-20230529165650804.png)

进入

![image-20230529165757774](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-629691-image-20230529165757774.png)

输入`root`，回车

密码输入刚刚算出来的`4a10224d`，回车

![image-20230529165917791](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-9e6d5c-image-20230529165917791.png)

### 输入命令获取SSH权限

- `nvram set ssh_en=1& nvram set uart_en=1 & nvram set boot_wait=on & nvram setbootdelay=3 & nvram set flag_try_sys1_failed=0 & nvram setflag_try_sys2_failed=1 `

- `nvram setflag_boot_rootfs=0 & nvram set "boot_fw1=run boot_rd_img;bootm" `
- `nvram setflag_boot_success=1 & nvram commit & /etc/init.d/dropbear enable &/etc/init.d/dropbear start`

![image-20230529170132469](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-5eac0c-image-20230529170132469.png)

### SSH登录

输入`ssh root@192.168.31.1`，输入yes保存指纹

![image-20230529170323658](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-565199-image-20230529170323658.png)

密码使用之前计算出来的密码：`4a10224d`

![image-20230529170455573](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-e7b1ad-image-20230529170455573.png)

### 上传底包

在底包所在文件夹shift加右键

![image-20230529170658186](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-32168a-image-20230529170658186.png)

在此处打开`powershell`

输入：`scp .\factory.bin root@192.168.31.1:/tmp/factory.bin`

输入密码

![image-20230529170821046](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-3deb65-image-20230529170821046.png)

### 开刷

回到SSH登录好路由器的地方

`ls /tmp/factory.bin`

![image-20230529171041408](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-813024-2023-05-29-27ffd4-image-20230529171041408.png)

没有返回`no such file or directory`说明上传成功

输入：`mtd -r write /tmp/factory.bin firmware`

![image-20230529171139027](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-ce55a9-image-20230529171139027.png)

等待路由器亮蓝灯，搜索wifi，如果出现两个`Openwrt`的wifi，则成功。

![image-20230529171245939](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-4b0e16-image-20230529171245939.png)

## Step4：开刷Openwrt

### 准备

连上`Openwrt`的wifi或者网线查lan口连上电脑

查看wifi连接属性

![image-20230529171703633](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-28c768-image-20230529171703633.png)

看到网段为`192.168.6.x`，路由器地址为`192.168.6.1`

### 进入后台刷固件

在浏览器输入路由器地址，进入

![image-20230529171834792](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-5904f3-2023-05-29-cc4308-image-20230529171834792.png)

用户名`root`，密码默认`password`

左边菜单选系统，备份/升级

![image-20230529171950134](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-eedcc8-image-20230529171950134.png)

刷写新固件，选择`ax6s-full.bin`

![image-20230529172045362](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-259db3-image-20230529172045362.png)

点击刷写固件，跳出来的界面点处理

![image-20230529172141412](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-4c31c6-image-20230529172141412.png)

继续等待路由器指示灯变蓝

## mentohust认证

刷入后有0.3.1的 `mentohust`，惊喜

不需要从`ipk`包开始刷了，登录后台填好了就用

`NEUQ`的校园网只能用旧版本的，新版本用不了（踩过坑QAQ。这个固件自带版本正好和上次成功的版本一样。

先进入接口，找到网卡

![image-20230529175044458](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-9dfd84-image-20230529175044458.png)

这里wan口用的`eth0.2`

### mentohust设置

进入服务-`MenToHUST`

先设置高级设置

![image-20230529175247909](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-74f835-image-20230529175247909.png)

ip地址填办公室的网口ip段中的任意一个

子网掩码改为`255.255.255.0`，网关对应着改一下

DNS服务器填公共DNS服务器比如`8.8.8.8`

![image-20230529175510023](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-bc1264-image-20230529175510023.png)

组播地址选锐捷，`DHCP`选未配置

保存&应用

然后回到常规设置

![image-20230529175607485](https://gitee.com/niimi_sora/pic-upload/raw/master/pics/2023-05-29-0d5799-image-20230529175607485.png)

勾选启用，用户名密码填校园网的

接口选择上面查到的`eth0.2`

ping主机设置为`114.114.114.114`

保存&应用

过一会儿就有网络了。
