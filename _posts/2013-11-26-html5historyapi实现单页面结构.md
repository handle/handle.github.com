---
layout: post
title: "html5 history api实现单页面结构"
description: ""
category: Mobile
tags: [举重若轻]
---
{% include JB/setup %}  
为了提高wap页面的响应速度，以及减少用户的流量，无线wap端越来越多地开始采用"单页面结构"（Single-page application）。所谓的单页面结构就是指多个页面间切换时，不刷新当前整个页面，而是更新页面中的某些需要更新的dom节点;并且要相应地改变地址栏中的url。

目前使用的比较多的一种实现单页面结构的实现方法是使用锚点来实现的。锚点是一种特殊的超链，用来在打开一个页面时定位到某一个位置，方便浏览。而当点击锚点时，也只会起到一个定位作用，不会刷新页面。根据这一特性，就可以实现在改变地址栏url（准确来说是锚点值）的同时，根据锚点的值来调用相应的接口，取得相应的数据加载到当前页面。   

但是根据http协议，仅只有锚点不同的ulr会被认为指向是同一个网络资源，只是浏览器在显示的时候，位置不同而已，这样会带来一些问题：  

1. 搜索引擎会将这些页面识别为同一个资源，而只收录一个，因此会降低seo的效果;当然可以通过在井号后加一个叹号来对seo友好，但是这样的url就会显得相当丑陋，曾经的twitter尝试使用，受到用户的抑制;   

-  由于锚点信息浏览器是不会传递到服务器端的，因为浏览器认为这个信息只有浏览器端会用到，服务器端不需要。因此服务器就无法识别当前请求具体请求是单页面结构下的哪个页面。这样当你想对某个页面进行流控，安全限制时就会比较麻烦;   
   

-  当你对这些url有要做重定向的需求时，且这些链接又是在android和ios的webview中打开时，重定向回去之后会丢失掉锚点信息。     
  
而html5的history api是html5通过js用来控制浏览器的history的标准方法。它是真正在改变浏览器地址栏中的url的同时，不去刷新整个页面。history接口的详细方法清单如下：  

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

但是这个url和当前打开的url必须是同源的，在测试时不能通过file://这样的方式在浏览器打开，还是需要开一个web服务器来测试。

这种方法相较于使用锚点来说，可以在保证单页面结构的同时，使不同的页面拥有不同的url，但是却要解决的一个问题是当用户直接通过这些不同url进来时，还能进入到这个单页面结构里。