---
title: AR场景无阴影
date: 2017-09-04 10:03:13
tags: [Vuforia,Unity]
---

###### Vuforia SDK v5.5.9.	

在项目中发现场景视口（Scene）观看，物体有阴影；游戏视口（Game）观看，物体无阴影。
经过在网上查找资料，找到问题所在。Vuforia自身的问题。投影矩阵出现问题。
解决方法：在ARCamera物体上（或场景其他地方）挂载如下脚本

``` C#
	/*
	 * Created by keefor on 6/13/2017.
	 */
	using UnityEngine;
	using System.Collections;
	using Vuforia;
	public class FixProjectionMatrix : MonoBehaviour, IVideoBackgroundEventHandler
	{
    	private Camera[] mCameras;

    	void Start()
    	{
    	    mCameras = VuforiaBehaviour.Instance.GetComponentsInChildren<Camera>();
    	    VuforiaBehaviour.Instance.RegisterVideoBgEventHandler(this);
   	}

    	public void OnVideoBackgroundConfigChanged()
    	{
         foreach (var cam in mCameras)
			{
            var projMatrix = cam.projectionMatrix;
            for (int i = 0; i < 16; i++)
            {
                if (System.Math.Abs(projMatrix[i]) < 1e-6)
                {
                    projMatrix[i] = 0.0f;
                }
            }
            cam.projectionMatrix = projMatrix;
			}
    	}
	}
```