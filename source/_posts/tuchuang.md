---
layout: post
title: "Markdown图片插入"
date: 2020-3-29 19:30
tags: 
  - Markdown
---

在使用 markdown 写文章时，会遇到要插入图片，markdown 本身是不支持图片直接插入，只能通过 `![]()` 的方式插入。

插入的图片路径有两种方式，一种是相对路径(相对于文档所在位置)，一种是绝对路径，如果使用相对路径，有一天不小心改动了图片的在本地的位置，这个图就会挂掉，为了解决这个问题一般使用绝对路径，先把图片上传到图床，然后会生成一个 url，在 markdown 中插入生成的 url，就可以显示图片了。

这里我选用七牛云的图床

第一步: 先去官网注册认证

官网地址: https://www.qiniu.com

>注册认证后有10G永久免费空间，每月10G国内和10G国外流量。

<!-- more -->
第二步: 下载图床神器
  + Windows 系统使用 MPic [点击此处下载](http://file.lzhaofu.cn/MPic%202.2.1.3.rar)
  + Mac 系统使用 PicU [点击此处下载](https://github.com/chenxtdo/UPImageMacApp/releases/download/1.54/PicU-1.54.dmg)

> 都支持拖拽、复制、截图上传，超级好用！！！

由于笔者着的是 Windows 系统，所以介绍 Windows 的 MPic 用法
  1.登录七牛云，创建一个存储空间 (获取空间名)
  ![mpic-1](/assets/blogImg/mpic-1.png)
  2.进入个人中心，获取 Access Key 与 Secret Key
  ![mpic-2](/assets/blogImg/mpic-2.png)
  3.先点击设置账号，配置账号信息
  ![mpic-3](/assets/blogImg/mpic-3.png)

  然后就可以愉快的使用图片上传神器了