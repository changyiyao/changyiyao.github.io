---
layout: post
title:  "[iOS-URL-Loading-System笔记一]概述"
date:   2016-01-25 21:00:00
categories: iOS development
---
URL Loading System 是一套通过URL获取内容的解决方案，包括classes和protocols：

![URL Loading System](/images/URL-Loading-System.png)

## 加载URL

在iOS7及以后，NSURLSession是非常推荐的一个执行NSURLRequest请求的API。NSURLSession可以理解为一项技术而不仅仅是一个类，是一个将NSURLRequest放入其中并且可供配置的容器。作为新的API，是Apple推荐的在新代码中使用的，用于取代NSURLConnection。它除了提供了所有NSURLConnection的能力外，还在设计模式上有了改进，接口更丰富灵活，功能更强大。

### 获取内容到内存

在高层次上，有两种方式获取URL对应的数据：

* 对于简单的请求，如保存到NSData或磁盘文件，直接使用NSURLSession获取NSURL对应的内容。
* 对于复杂的请求，如使用NSURLRequst(NSMutableURLRequest)上传数据，可以使用NSURLSession或者NSURLConnection。

无论使用那种方式获取数据，你可以通过以下两种方式得到response数据：

* 提供一个completion handle block。URL Loading类在完成从服务器获取数据后会调用该block。
* 提供一个delegate。URL Loading类会周期性地在获得数据后调用delegate方法。

## 辅助类

URL Loading类使用两个辅助类用于提供额外的元数据——NSURLRequest和NSURLResponse。

* NSURLRequest
* NSURLResponse封装的元数据包含了大多数协议的MIME type, 期望数据长度，文本编码以及对应的URL。

## 重定向

通过delegate方法决定重定向行为

## 缓存管理

URL Loading System提供了一套磁盘缓存和内存缓存相结合的缓存管理机制。该缓存是app独立的。缓存管理策略由NSURLConnection通过NSURLRequest对象中的cache policy决定的。

NSURLCache提供了配置cache大小和cache在磁盘上存储位置的方法，以及管理NSCachedURLResponse对象的方法。

不是所有协议都支持reponse缓存，目前只有http和https请求会被缓存。

NSURLConnection对象可以通过`connection:willCacheResponse:`delegate方法控制reponse是否被缓存，reponse是否应该仅被缓存在内存中。

## Cookie存储

NSHTTPCookieStorage提供管理NSHttpCookie对象的接口。

## 支持协议

* http
* https
* file
* ftp
* data
* 自定义应用层协议

## NSURLSession对HTTP协议版本支持情况

iOS 9 : HTTP1.X, SPDY, HTTP/2

iOS 8 : HTTP/1.x, SPDY

iOS 7 : HTTP/1.x

## 参考链接

[About the URL Loading System](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html#//apple_ref/doc/uid/10000165i)

[Networking with NSURLSession](https://developer.apple.com/videos/play/wwdc2015-711/)

[What’s New in Foundation Networking](https://asciiwwdc.com/2013/sessions/705)

[Apple Developer Forums - HTTP/2 support for iOS 8](https://forums.developer.apple.com/thread/11991)


