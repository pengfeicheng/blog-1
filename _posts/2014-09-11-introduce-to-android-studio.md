---
layout: post
title:  "帮你上手Android Studio"
author: wutiantong
tags: [Android Studio, android, 使用工具的技巧]
---

## 前言

Android的开发技术可谓日新月异，小伙伴们刚适应Eclipse+ADT还没多久就已经过时了！  
目前的Android Studio还处于Beta阶段，版本号是0.8.9，不过你会发现那些热爱开发的家伙们已经完全掌握了这个新的IDE。  
工具就是生产力，对待新的开发工具我一向是采取积极主动的态度，越早掌握收益越大。  
请不要抱有——“以前的方式还行得通，等到实在不行的那天再说吧”——这种想法，消极懒惰的态度只会使得事情越来越糟糕。

## 序

我刚刚用了不到一天的时间浏览了一遍android开发网站上关于Android Studio使用的相关文章，你可以在[这个页面](http://developer.android.com/sdk/installing/studio.html)的左侧边栏的Android Studio类别下找到这些文章。本文相当于是这些文章里的关键信息的摘要。

## 正文

在[Creating a Project](http://developer.android.com/sdk/installing/create-project.html)中唯独值得一提的是，你可以看到在选择`Phone and Tablet`，`TV`，`Wear`，`Glass`的那一页上，其实是可以多选的（而非单选）。

![](http://developer.android.com/images/tools/wizard3.png)

在[Tips and Tricks](http://developer.android.com/sdk/installing/studio-tips.html)中我们了解到Android Studio的界面是基于一款叫[IntelliJ IDEA](http://www.jetbrains.com/idea/index.html)的IDE软件，而编译系统(Building System)则是基于[Gradle](http://tools.android.com/tech-docs/new-build-system/user-guide)。  
在Android Studio创建的项目目录结构中，好多都是跟上面这两个东西相关的。如果只想简单使用的话，那么基本上你只需要修改`src/`目录下的东西就行了（`src/`并不在项目的根目录下，通常它位于`项目根目录/app/`下面）。  
顺带一提，有一个快捷键特别重要，叫项目快速修正(project quick fix)，它是`ALT+ENTER`。

[Building Your Project with Gradle](http://developer.android.com/sdk/installing/studio-build.html)中包含了非常多的关键信息，下面一个一个来讲。

首先讲两个概念：`project`和`module`。`project`代表整个项目，或者说代表这个Android app项目的整体（但可能对应多个apk），一个`project`由若干个`module`构成。每个`module`都拥有各自的源代码和资源文件，并且可以单独进行编译调试。总共有三种`module`，分别是：

- `Java library modules`，包含可重用的Java代码，在编译系统中生成JAR包。
- `Android library modules`，包含可重用的Android代码或资源，在编译系统中生成AAR包。
- `Android application modules`，包含应用逻辑代码，可以引用（依赖，depend）上面那两种`module`，在编译系统中生成APK包。

通常情况下，你的项目里只会包含一个`Android application module`，也就是前文中提到的`项目根目录/app/`这个app目录。不过，如果你在选择`Phone and Tablet`，`TV`，`Wear`，`Glass`的那一页上多选的话，你就会得到多个`Android application module`，其中隐含的逻辑想必不用我多说了。  
顺带一提，无论是`project`还是`module`，都有自己对应的Gradle编译文件(`Gradle build file`)，文件名统一是`build.gradle`。

每个Android Studio项目中都包含自己的`Gradle wrapper`，位于`gradle/wrapper/`目录下，这里面实际上包含了Gradle的本体。Android Studio项目使用`Gradle wrapper`进行编译而非在操作系统里安装的Gradle，其主要意义在于严格控制项目所使用的Gradle版本，换言之，不同的项目可能会使用不同版本的Gradle来编译，`Gradle wrapper`确保它们互不干扰。（吐槽一下：这就是所谓的自备干粮啊！）

原文中有[详细的例子](http://developer.android.com/sdk/installing/studio-build.html#creatingBuilding)展示如何创建并使用一个`Android library module`，务必浏览一下。

关于`build.gradle`的基本情况请自行参考[原文](http://developer.android.com/sdk/installing/studio-build.html#configBuild)，其中关于`defaultConfig`的说明一下要仔细看一下。

接下来我想重点讲一下`build variants`，为了使你得到一个直观的概念，我要先指出一个重要的事实：每个`build variant`都对应（生成）一个特定的APK包。`build variants`主要用于把一个app搞出多个不同版本，比如有很多工具类app会分成试用版(Demo)，免费版(Lite)，专业版(Pro)，或者是针对不同的机型分成好多个特定的版本。  
`build variant`是由`build type`和`product flavor`构成的，`build type`有且仅有两个：`debug`和`release`，`product flavor`是我们自己定义的，默认情况下不存在`product flavor`，所以在默认情况下存在两个`build variant`，就是debug版本和release版本。如果你定义了两个`product flavor`－`full`和`demo`，结果就会有四个`build variant`，分别是：

- demo-debug
- demo-release
- full-debug
- full-release

真正的重点在于Gradle编译系统是如何自动的编译出不同版本的`build variant`的呢？  
答案就是——利用`src/`目录下的目录结构。  
实际上`src/`并没有直接包含相关的代码和资源，默认情况下，`src/`中有个`main/`目录，代码和资源文件都是在这个`src/main/`下面。而`src/main/`只是所谓的公共目录而已，这里存放的文件会被所有的`build variant`共享，相对而言，你可以在`src/`目录下创建`src/<buildType>/`以及`src/<flavorName>/`目录，这些目录中存放的文件是针对特定的`build type`以及`product flavor`的。  
关于`build variant`的使用方法，在原文中有一个很详细的例子，请务必看一下。

在[Debugging with Android Studio](http://developer.android.com/sdk/installing/studio-debug.html)中值得一提的是`Exception Breakpoints`这个功能，它可以在任何发生异常的地方自动产生断点。`Capture Screenshots and Videos`也是个非常有趣的功能。

---

全文结束。