---
layout: post
title: "debain下挂载nas文件"
description: ""
category: Linux
tags: [举重若轻]
---
{% include JB/setup %}
* 查看是否有安装mount.cifs命令，如果没有安装就需要安装这个命令  
> sudo apt-get install smbfs  
    
* 挂载相应的文件，命令如下:  
> sudo mount.cifs nasdir  localdir –o user=XXX,pass=XXX,dom=XXX, iocharset=utf8  
    
其中nasdir为nas文件目录，注意不要在后面加上“/”  
其中localdir为本地文件目录，注意不要在后面加上“/”  

* 如果要卸载该目录，命令如下：  
> sudo umount.cifs localdir  

同样也不要在localdir后面加上“/”,有时候有可能没有umount.cifs命令，可以尝试通过下面的方法来卸载：  
> sudo umount -t cifs localdir  
