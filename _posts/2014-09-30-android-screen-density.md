---
layout: post
title:  "Android UI设计－屏幕像素密度所带来的影响"
author: wutiantong
tags: [Android, UI设计, dpi]
---

##### 本文内容主要参考[这篇官方文档](http://developer.android.com/training/multiscreen/screendensities.html)。

### 屏幕像素密度(dpi)

**屏幕像素密度**指的是屏幕上每英寸的范围内所包含的像素数量，计量单位是`dpi`(Dots Per Inch)。

屏幕的dpi越大，所能显示的细节就越丰富细腻，相应的，价格也会更昂贵。

iPhone4是首款采用*Retina*屏幕的iPhone手机，Retina屏幕的主要意义就是更高的dpi。

iPhone4虽然和iPhone3G的屏幕尺寸一样－同为3.5英寸，但它们的显示效果却有着天壤之别，在Retina屏幕的对比下，iPhone3G的屏幕几乎可以称为是“颗粒状”的。

3.5英寸屏幕的宽度是**2英寸**，  
iPhone3G的屏幕分辨率是**320x480**，也就是说它的dpi是 `320/2 = 160`，  
iPhone4的屏幕分辨率是**640x960**，因此它的dpi是`640/2 = 320`。

### 屏幕像素密度给视图布局带来的影响

程序员在写视图布局的代码时，面临的一个主要问题是“为所有的控件指定合适的位置和大小”。

在传统的*基于像素的UI设计*中，这里提到的“位置”或“大小”通常是以屏幕分辨率为参照坐标系，以单个像素为单位进行计量。

但是，这种以像素作为单位的视图布局，并不能很好的适应不同dpi的屏幕，我们举例来说明这里面的主要问题：

假如我们需要在屏幕的左上角显示一个用户头像，我们把显示这个用户头像的控件的大小定义为`64像素*64像素`。

64个像素，在iPhone3G的屏幕上所占据的尺寸是`2(inch) * (64 / 320) = 0.4(inch)`，这个尺寸大小可能刚好是我们希望的。

然后，在iPhone4的屏幕上64个像素所占据的尺寸是`2(inch) * (64 / 640) = 0.2(inch)` ——  
我们发现这个控件在iPhone4的屏幕上所占据的实际尺寸缩水了一倍，另一方面，屏幕的整体尺寸都是3.5寸，因此这个控件在屏幕上看起来就会显得特别小，这不是我们所期望的。

我们可以稍微思考一下，这个问题的本质是什么？

把这个控件的大小定义为`64像素*64像素`真的合适吗？

如果能把这个控件的大小直接定义为`0.4英寸*0.4英寸`可能会更符合我们的期望，对吧？

### 密度无关的像素 (Density-independent Pixels)

在Android的官方文档中，对于**密度无关的像素**的定义是：

    一个密度无关的像素，或者说是一个dp，对应于：在dpi为160的屏幕上，一个实际像素的物理尺寸。

很多人没太搞懂，顾名思义，觉得“密度无关的像素”应该跟“实际像素”是差不多同类的概念，其实这是误解（或者说是名字带来的误导）。

`dp`实际上就是个物理尺寸单位，或者更直白点说，就是个距离单位。

“在dpi为160的屏幕上，一个实际像素的物理尺寸”是多少？

既然dpi为160，也就是说每英寸包含160个实际像素，那么一个实际像素的物理尺寸就是`1/160英寸`。

因此实际上，`1dp` 就等于 `1/160英寸`。

我们可以再回顾一下前面那个用户头像的例子，在最后我们发现也许应该把控件尺寸直接定义为`0.4英寸*0.4英寸`，而`dp`这个单位就是用来满足这个愿望的。

    0.4英寸 = 64 * (1/160英寸) = 64 dp
    
因此，我们可以把显示用户头像的控件的尺寸大小定义为`64dp * 64dp`（不同于`64像素*64像素`），这个尺寸大小无论在哪种dpi的屏幕上都相当于是`0.4英寸*0.4英寸`，也就是说**看起来是一样大的**。

我们来总结一下：

1. `dp`说白了只是Android定义出来的一个距离单位。
2. 因为Android设备可能会有各种不同的屏幕dpi，所以`dp`做单位会比像素做单位更合适，毕竟UI设计更应该关注视图的**实际物理尺寸**。
3. `dp`并不是问题的终点，而是问题的起点，建立在`dp`这个概念的基础上，我们起码应该进一步考虑以下两个问题——

### 问题其一：为一张图片制作多个版本

为什么要为一张图片制作多个版本呢？

还是以前面提到的用户头像为例，`64dp * 64dp`(`0.4英寸*0.4英寸`)的控件在dpi为160的屏幕上所占据的实际像素大小为`64像素*64像素`，而在dpi为320的屏幕上所占据的实际像素大小则是`128像素*128像素`。

代表用户头像的图片如果只存在一个版本，比如是`64像素*64像素`的，那么当它显示于dpi为160的屏幕上时看起来会很正常，而在dpi为320的屏幕上就会被强制拉伸到`128像素*128像素`的显示大小，这种程序实时渲染的拉伸效果通常都不会太理想，看起来会很糊。

即使是提供较大尺寸的版本，比如图片是`128像素*128像素`的，那么当它显示于dpi为160的屏幕上时又会被强制缩小成`64像素*64像素`的显示大小，这种程序实时渲染的缩小效果通常也不会太理想，看起来可能会出现毛刺。

因此，为了妥善的解决这个问题，就有必要针对不同的屏幕dpi提供图片的多种尺寸的版本，也就是说，代表用户头像的图片应该同时存在`64像素*64像素`和`128像素*128像素`两种尺寸的版本。如果还想支持160，320以外的其他dpi的屏幕，就应该提供更多尺寸的版本。

Android考虑到了这方面的需求，并提供了直接的支持：开发者只需要把不同版本的图片放入到指定dpi的文件夹里面，程序就会自动根据当前设备的屏幕dpi加载相应dpi的文件夹中的那个版本的图片。

这些指定的文件夹包括：

1. `res/drawable-xhdpi/`，对应于dpi为320的屏幕，用户头像的图片的尺寸应该是`128像素*128像素`
2. `res/drawable-hdpi/`，对应于dpi为240的屏幕，用户头像的图片的尺寸应该是`96像素*96像素`
3. `res/drawable-mdpi/`，对应于dpi为160的屏幕，用户头像的图片的尺寸应该是`64像素*64像素`
4. `res/drawable-ldpi/`，对应于dpi为120的屏幕，用户头像的图片的尺寸应该是`48像素*48像素`

ps1: 如果程序在指定dpi的文件夹里没有找到想要的图片，就会到`res/drawable`里寻找图片。`res/drawable`里提供的图片具有“确保存在但不确保最合适”的意义，即使你提供了图片的各种dpi的版本，也应该在`res/drawable`放置一份你觉得“最通用”的那个版本的图片。如果你不打算提供多个版本的图片，那么你应该把图片的唯一版本直接放在`res/drawable`中。

ps2: 实际的屏幕dpi可能五花八门，因此上面提到的的对应关系并不是严格相等，比如针对dpi为360的屏幕，程序会加载`res/drawable-xhdpi/`中的图片。

ps3: Android 所提供的指定dpi的文件夹会不断新增拓展，因此当前可能已经不止上面列举的这四个文件夹了。

### 问题其二：不同尺寸的屏幕

Android设备不仅会有各种不同的屏幕dpi，实际上还会有各种不同的屏幕尺寸。

3.5寸屏幕只是智能手机的起点，智能手机的屏幕尺寸每年都在不断翻新，4寸，4.7寸，5寸，5.5寸...

而且还要考虑到新的设备类型，比如屏幕更大的Pad和屏幕更小的穿戴式设备。

不同的屏幕尺寸给UI设计的视图布局带来的挑战主要体现在两方面：

1. 同一个控件的位置在不同尺寸的屏幕上可能会需要调整。比如，如果我们想在屏幕的中心点上放置一个按钮，3.5寸屏幕的中心点位置是`(160dp, 240dp)`，而4寸屏幕(iPhone5)的中心点位置则是`(160dp, 284dp)`。
2. 同一个控件的大小在不同尺寸的屏幕上可能会需要调整。比如，如果我们想在屏幕上方放置一个`8:1`的横幅图片，在3.5寸屏幕上它的大小应该是`320dp*40dp`，而在6寸屏幕上（华为Mate7）上它的大小应该是`480dp*60dp`。

Android为这些问题提供了完善的解决方案，不过这方面的知识更为复杂，而且已经超出了本文的讨论范围。  
有兴趣的读者可以自行查阅[这篇官方文档](http://developer.android.com/training/multiscreen/screensizes.html)，稍后我也可能会专门写篇关于这方面的Blog。
