---
layout: post
title: "html5 history api实现单页面结构"
description: ""
category: Mobile
tags: [举重若轻]
---
{% include JB/setup %}  
为了提高wap页面的响应速度，减少用户的流量，无线wap端越来越多地开始采用"单页面结构"（Single-page application）。所谓的单页面结构就是指多个页面间切换时，不刷新当前整个页面，更新页面展示数据，并且相应地改变地址栏中的url，以使用户可以分享这个url。

目前使用的比较多的一种实现单页面结构的实现方法是使用锚点来实现的。锚点是一种特殊的超链，用来在打开一个页面时定位到某一个位置方便浏览。当点击锚点时，只会起到一个定位作用，而不会刷新页面。根据这一特性，就可以实现在改变地址栏url（准确来说是锚点值）的同时，然后根据锚点的值来调用相应的接口，以展示对应的内容。   

但是根据http协议，锚点是不用来区分不同的网络资源的，只是浏览器在显示的时候，定位到不同位置而已。这样通过锚点来实现单页面结构就会在其它方面带来一些问题：  

1. 搜索引擎会将这些页面识别为同一个资源，而只收录一个，因此会降低seo的效果;当然可以通过在井号后加一个叹号来对seo友好，但是当用户想要长期推广这个url时，会觉得比较丑陋，之前twitter尝试过，然后就没有然后了;   

-  由于锚点信息浏览器是不会传递到服务器端的，因为浏览器认为这个信息只有浏览器端会用到，服务器端不需要。因此服务器端就无法识别当前请求具体请求是单页面结构下的哪个页面。这样当你想对某个页面进行流控，安全限制时就会比较麻烦;   

-  当你对这些url有要做重定向的需求时，且这些链接又是在android或者ios的webview中打开时，重定向之后webview会丢失掉锚点信息。     
  
而html5的history api是html5通过js用来控制浏览器的history的标准方法。它可以真正在改变浏览器地址栏中的url的同时，不去刷新整个页面。history接口的详细方法清单如下：  

    interface History {
      readonly attribute long length;
      readonly attribute any state;
      void go(optional long delta);
      void back();
      void forward();
      void pushState(any data, DOMString title, optional DOMString? url = null);
      void replaceState(any data, DOMString title, optional DOMString? url = null);
    };

而要实现改变地址栏中的url的功能要使用到的方法是pushState,它的第三个参数指的即为在地址栏中想要显示的url(前面两个参数，一个是要传递的数据，一个是新页面的title,但是好像现在还不支持)，如下：  

    window.history.pushState(null, null, "http://xxxx/url1");  

如果要测试需要搭建一个web服务器，以http://host/的形式去访问才能生效，如果你在本地测试以file://这样的方式在浏览器打开，就会出现如下的问题：

> Uncaught SecurityError: A history state object with URL 'file:///C:/Users/Administrator/Desktop/historyapi1.html' cannot be created in a document with origin 'null'. 

这是因为你要pushState的url与当前页面的url必须是同源的，而file://形式打开的页面是没有origin的，所以会报这个错误。  
  
这种方法相较于使用锚点来说，可以在保证单页面结构的同时，使不同的页面拥有不同的url，避免了使用同一个url所带来的一系列问题。但是同时要解决的是当用户直接打开这些不同的url时也能到达这个单页面结构页。最简单的办法是直接将这些url统一内部重定向到单页面结构页。