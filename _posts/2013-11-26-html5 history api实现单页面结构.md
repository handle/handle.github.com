---
layout: post
title: "html5 history api实现单页面结构"
description: ""
category: Mobile
tags: [举重若轻]
---
{% include JB/setup %}  
为了提高wap页面的响应速度，以及减少用户的流量，无线端越来越多地开始采用"单页面结构"。所谓的单页面结构就是指多个页面间切换时，不刷新当前整个页面，而是更新页面中的某些需要更新的dom节点;并且在通过点击跳加载另一个页面内容时，要相应地改变地址栏中的url。要做到页面内容的无刷更新并不困难，使用ajax或者jsonp等技术就可以实现。而本文主要介绍如何在点击后页面无刷地来改变地址格栏的url。  

目前使用的比较多的一种实现单页面结构的方法是使用锚点来区分不同的页面。锚点本来是用来定位一个资源要显示位置的。而当点击锚点时，只会起到一个定位作用，不会去刷新页面。根据这一特性，就可以实现改变地址栏url的效果，而且根据锚点的值信息，可以调用相对应的接口来相应的页面。   
但是根据http协议，锚点不同，前面的ulr相同的所有页面都会被识别为同一个网络资源，这样会带来一些问题：  

- 搜索引擎会将这些页面都识别为同一个资源，而只收录一个，因此会降低seo的效果;当然可以通过在井号后加一个叹号来对seo友好，但是这样的url始终对用户也不是太友好，尤其是在一些用户要用url来推广时;   

- 由于锚点信息浏览器是不会传递到服务器端的，因为浏览器认为这个信息只有浏览器端会用到，服务器端不需要。因此服务器就无法识别当前请求具体请求是单页面结构下的哪个页面。这样当你想对某个页面进行流控，安全限制时就会比较麻烦;   
   

- 当你对这些url有可能做重定向时，很可能导致android和ios在webview中打开时，丢失掉锚点信息，永远打开的同一个页面。     

上述的问题根源在于当页面加载另一个页面的内容时，地址栏中的url无法真正改变所致，而要解决这个问题，还有另外一种作法，那就是通过html5的history api来实现。  
html5 history api是html5通过js用来控制浏览器的history的标准方法。history接口的详细方法清单如下：  

    interface History {
      readonly attribute long length;
      readonly attribute any state;
      void go(optional long delta);
      void back();
      void forward();
      void pushState(any data, DOMString title, optional DOMString? url = null);
      void replaceState(any data, DOMString title, optional DOMString? url = null);
    };

而在这里我们要使用到的方法是pushState,它的第三个参数指的即为你想在地址栏中想要显示的url(前面两个参数，一个是要传递的数据，一个是新页面的title,但是好像现在还不支持)，如下：  

    window.history.pushState(null, null, "http://xxxx/url1");  

这种方法相较于使用锚点来说，解决了所有单页面结构下的页面被http识别为同一个资源的问题，但是另外一个问题就是，你需要实现http://xxxx/url1这个url的页面展示，否则用户直接通过这个链接进来就会出错。
