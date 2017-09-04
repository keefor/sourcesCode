---
title: Spine 动画残影
date: 2017-09-03 10:30:13
tags: [Unity,Spine]
---
###### FairyGUI 1.4.3 , SpineToUnity 3.5.x.x , Unity5.2.3
	
在界面上点击按钮，场景中的2D模型出现有相应动画，动画播放结束后2D模型动画状态重置并且模型消失。
然而每次点击时，都会有上一次动画的残影出现。

在Spine插件的教程显示。每次动画播放前执行SkeletonAnimation.state.ClearTracks();可消除残影。
经尝试此方法对于该项目无效。

经多次问题排查后，发现，使用Unity原生GUI.Button控制动画无残影出现，使用FairyGUI 才会出现残影。
问题不是在Spine，而是在所使用的FairyGUI。FairyGUI作者告知，FairyGUI按钮事件是在LateUpdate里执行。

最终选择的解决方法：将2D模型在看不见的地方显示出来并且播放动画，下一帧再将模型放到可见的位置。