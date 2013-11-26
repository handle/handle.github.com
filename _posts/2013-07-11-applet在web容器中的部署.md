---
layout: post
title: "applet在web容器中的部署"
description: ""
category: "Java" 
tags: [沧海拾遗]
---
{% include JB/setup %}
将一个applet部署到web应用中，首先在html端很简单，只要在html嵌入如下的applet代码即可：   

    <applet id="TestApplet" archive="TestApplet.jar" code="TestApplet.class" codebase="." width="100" height="100"></applet>

但是这个时候要把TestApplet.jar部署到哪呢？假设上面的html页面的访问url为如下：

    http://XXXXX/path/test.html

而applet在执行时需要去下载对应的jar包，而这个jar的下载地址由两个来决定，当前的url路径和codebase,如上面嵌入的applet的codebase为“.”即是说当前目录，因此这个jar文件的下载目录应该如下：

    http://XXXXX/path/TestApplet.jar

所以你只要放在一个在上述地址能够请求到的地方即可，这样在打开这个页面时，浏览器就会自动去下载这个jar包到本地，然后去执行applet了。
