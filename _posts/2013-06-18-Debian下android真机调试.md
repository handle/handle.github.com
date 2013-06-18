---
layout: post
title: "Debian下android真机调试"
description: ""
category: Android
tags: [举重若轻]
---
{% include JB/setup %}
Linux下android连真机设备经常显示设置名为"??????",并且查看设备时也出现"????????? no permission",并且导致连上不真机，无法调试。google了一下，解决方法如下：     
1. 进入root用户（在debian下好像必须是在root用户下才行，使用sudo也不行）     

2. 关闭服务

    ./adb kill-server


3. 重启服务

    ./adb start-server

4. 查看所连设备

    ./adb devices

显示如下：

> List of devices attached    
> 0123456789ABCDEF	device
