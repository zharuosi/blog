---
title: Adobe Premiere视频剪辑入门
date: 2018-06-07 22:01:38
categories: 工技篇之吴带当风
tags: [Post Processing,Video Editing,Premiere]
---

本文介绍如何使用Adobe公司旗下的Premiere来剪辑视频。本文只介绍一些基本操作，仅供入门。其他多种多样的玩法以后有时间再慢慢摸索。

## Premiere简介

首先简单介绍一下[Premiere](https://zh.wikipedia.org/zh-cn/Adobe_Premiere_Pro)这个软件。以下摘自中文维基百科：Adobe Premiere Pro是由Adobe公司开发的非线性编辑的视频编辑软件。Premiere是Creative Suite套装的一部分，可用于图像设计、视频编辑与网页开发。Premiere Pro支持许多不同插件以加强其功能、增加额外的视频/声音效果及支持更多的文件格式。Premiere Pro能支持非常高的清晰度，可最高支持10,240x8,192的显示屏清晰度，以及高至32位的色深，可使用RGB和YUV颜色模型。在声音方面，能支持VST声音插件，以及5.1声道环绕立体声。Premiere Pro的插件能够导入与导出至QuickTime或DirectShow的格式。Premiere Pro也支持很多视频与声音格式，在导出和导入视频时，也提供很多编码解码器。通过使用Cineform Neo line的插件，就可以支持3D编辑功能，也可以在2D的显示屏上看见3D物料。Photoshop文件（psd档）可直接导入至Premiere Pro中。

注意，<!-- more --> 当前版本仅支持64位操作系统。使用Premiere的另一个原因是其对wmv格式的视频文件支持很友好。如果出现无法打开的视频素材，请使用“[格式工厂](http://www.pcgeshi.com/)”进行转换。PS. 格式工厂是目前windows上我用过的最好的格式转换软件。

非线性编辑指的是对视频或音频的素材通过电脑设备或其他数字随机存取的方式剪辑，意味着能够对视频片段中的任意一帧进行操作。在最初电影剪接的过程中，电影底片必须被剪断。数字视频技术出现后，产生了非线性剪接手段。与过去需要两台以上的录像机，从不同的磁带合成到一盘磁带的线性编辑方式相比，能立即重新排列、替换、增加、删除、修改映像数据，以达到快速编辑的目标，并且理论上素材质量不会损失。

## 视频加速

我在工作中一个需求是对视频加速。原始视频文件是经过慢放处理的，市场一共接近三分钟，而我希望在演讲PPT时在10秒内放完，那我就需要加速至1800%。那么打开Premiere，进行如下操作。

- 新建项目。
- 导入视频文件。双击左下区域内的位置，选择所有要导入的素材即可。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere1.PNG" width="95%" height="95%">

- 新建轨道。右击图中1箭头所示的按钮即可新建轨道等。或者直接将左下区域内出现的视频文件拖至此按钮上。然后右下区域就会出现对应的文件和轨道，按行排列。上部为视频轨道，下部为音频轨道。左上区域可以对单个轨道进行编辑，右上区域可以预览叠加后的效果。拖动右下区域的红色竖线可预览不同时刻。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere2.PNG" width="95%" height="95%">

- 调整速度。在轨道上视频素材对应位置处右击可以看到很多操作，编辑“剪辑速度/持续时间”，改变数值可使视频加速播放或减速播放。此时可以看到对应轨道上的长度变化。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere%EF%BC%96.PNG" width="95%" height="95%">

## 视频剪辑

将视频导入轨道之后，便可以对时间线进行删减。使用快捷键C，此时时间线会从你cut的地方断开，视频会变成两段，重新拖动便可改变顺序，或提取出你需要的部分重新进行拼接。使用快捷键D，可以删掉当前所在的片段。不使用波纹编辑的话，视频删掉的那部分会变成空白。使用波纹编辑，删掉的部分后续片段会自动衔接至上一片段。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere5.PNG" width="95%" height="95%">

## 合二为一

我的另一个需求是对两个视频一左一右进行对比，同时播放。于是不仅要调整播放速度使之每个时刻保持一致，还要调整每个视频的位置和比例。

- 效果控件。左上区域顶部有一排菜单，调整视频位置和比例需要用到其中的效果控件。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere3.PNG" width="95%" height="95%">

- 单击“运动”。然后转到右上区域，点击要调整的视频，可以看到视频四周出现了调整位置大小的控制点，拖动即可编辑。预览一下，看看是不是实现了同步播放。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere4.PNG" width="95%" height="95%">

- 调整背景。默认合成的视频背景色是黑色，我们可以调整为白色。有很多方法，这里介绍其中一个。在之前新建轨道的左下角那个按钮上点击，新建彩色蒙版。颜色改为白色。将该片段拖至所有视频轨道**下方**即可。

## 输出

输出编辑好的视频时，可以调整输出格式、位置等等。也可以添加滤镜、更改视频解码器等等。这里也可以选取输出的时间片段。拖动橘色的时间线，便可更改视频的**起始位置**。最后点击导出即可。

<img src="https://raw.githubusercontent.com/zharuosi/2017-2018/master/X2018/premiere_edit_video/premiere7.PNG" width="95%" height="95%">

以后有其他需求再更新本文。








