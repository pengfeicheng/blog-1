---
layout: post
title:  "关于Android Studio项目的.gitignore"
author: wutiantong
tags: [Android Studio, Git, 使用工具的技巧]
---

## 前言

对于一个新接触的IDE工具来讲，如何正确的把项目提交到Git中并不简单——因为凡是IDE都会有很多所谓的项目配置文件，这些文件有的需要被添加到Git仓库中，有的不需要，如何区分它们并不容易。

本文要讲的正是关于Android Studio的.gitignore该如何正确设置。

## 过程

首先，你应该很容易发现，用Android Studio创建项目后，它里面已经包含了配置过的.gitignore，而且还不止一个（`app/`目录下也有），本指望自带的这个.gitignore够用了，不过稍微运行调试了几次程序后就发现`.idea/`目录下出现untracked files，而且其中位于`.idea/dictionaries/`中的xml文件明显跟用户相关(user related)，一般情况下用户相关的文件是需要ignore的，这说明自带的.gitignore还是有点问题的（没有ignore `.idea/dictionaries/`）。

于是乎就搜索了一通，取得了圆满的成果。

首先，作为意外惊喜，发现了一个很nice的网站：<https://www.gitignore.io>，几乎囊括了所有你想要的.gitignore！

我在上面尝试检索`intellij`，得到了一个看上去看厉害的.gitignore，但是仔细一看我还是觉得无法认同，因为它居然ignore了所有的iml文件以及整个`.idea/`目录，这显然是不合适的。

接下来，我在搜索引擎上找到了[stackoverflow上的一个帖子](http://stackoverflow.com/questions/11968531/what-to-gitignore-from-the-idea-folder)，在回答里给出了这个问题的[关键链接](https://intellij-support.jetbrains.com/entries/23393067)。

在最后这个链接里给出了全面，清晰，精确的界定。不过有一点可以提一下，它里面建议"not to share .idea/gradle.xml"，并给出了理由的出处，我看了一下，大意是`.idea/gradle.xml`主要用于指出`grable`在本地的安装位置（local path），因为不是项目相对路径，所以不适合share。不过呢，这个问题在Android Studio中不存在，因为我们知道Android Studio使用`Gradle Wrapper`，事实上，Android Studio自动生成的.gitignore是没有排除`.idea/gradle.xml`，你可以创建项目看一下其中的`.idea/gradle.xml`，很明显是需要share的。

## 结论

Android Studio自动生成的.gitignore大体上是非常正确的，不过你仍然可以在其中补充两条:

    ## 貌似不要也行，因为貌似不会产生 
    /.idea/tasks.xml
    
    ## 这个是需要的
    /.idea/dictionaries