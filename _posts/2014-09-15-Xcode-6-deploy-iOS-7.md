---
layout: post
title:  "Xcode 6创建项目在iOS 7设备上运行的一个问题"
author: wutiantong
tags: [Xcode, iOS, UIScreen, LaunchImage]
---

今天开始用 Xcode 6＋swift 写代码，创建项目后发现了两个问题：

1. Universal Project 居然只有一个storyboard？
2. 在我的iOS 7.1.2手机上运行后屏幕上下出现黑条，也就是说屏幕的尺寸被强制成了2:3。

第一个问题在[What's New in iOS](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS8.html#//apple_ref/doc/uid/TP40014205-SW1)中找到了答案，这其实是iOS 8带来的一大福利，叫`Unified Storyboards for Universal Apps`，涉及一个新的概念叫`Size Class`。

本文并不详细讨论这个问题，仅提供[一篇博客](http://blog.sunnyxx.com/2014/09/09/ios8-size-classes/)以供参考。

第二个问题，我测试了一番，发现项目运行在iOS 7系统上时`UIScreen.mainScreen()`的`bounds`总是`(0, 0, 320, 480)`。

实在是没有头绪，后来总算找到了[stackflow上的一个相关问题](http://stackoverflow.com/questions/21668497/uiscreen-mainscreen-bounds-returning-wrong-size)，回答者指出，对于iPhone 5+，iOS会依赖LaunchImage的分辨率来决定App运行时所处的分辨率。那么如果没有提供LaunchImage的话，分辨率就会默认成320x480。我们知道新的Xcode 6项目默认采用[Launch Screen File](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW3)，而Launch Screen File所依赖的`Size Class`并不支持旧的iOS 7，因此默认的项目相当于并没有为iOS 7的设备提供LaunchImage。

知道了原因后，解决方法就简单了，采用老的`Image Asset`给iOS 7配置相应的LaunchImage后，显示就正常了。

顺带一提，似乎`Image Asset`和`Launch Screen File`并不冲突，iOS 8的设备会自动引用后者，而旧系统就会引用前者。