---
title: Unity iOS录音失败
date: 2017-09-04 12:03:13
tags: [iOS,Unity]
---
###### Unity 版本 5.2.3

使用UnityEngine.Microphone录音时，在iOS系统下，首次录音正常，再次录音，录音失败。
可通过报错的字面意思了解到，大概就是由于录音没有关闭造成的，

* 第一种解决方法，在PlayerSetting-》Other Setting下勾选 Prepare iOS for Recording。
此种方法可以解决无法录音的问题，但是又会造成新的问题。声音在听筒发出。

* 第二种解决方法，调用iOS原生代码，关闭录音。代码如下：

iOS文件夹下放置文件：	

```
#import <AVFoundation/AVFoundation.h>
void SetPreferredSampleRate(int sampleRate) {
	AVAudioSession *audioSession = [AVAudioSession sharedInstance];
	[audioSession setPreferredHardwareSampleRate:sampleRate error:nil];
}
```
Unity调用：

```
#if UNITY_IPHONE && !UNITY_EDITOR
        
[DllImport ("__Internal")]
        
private static extern void SetPreferredSampleRate(int sampleRate);

#endif

public static void EnableRecording()
{
	if UNITY_IPHONE && !UNITY_EDITOR
       
	SetPreferredSampleRate(AudioSettings.outputSampleRate);
		
	#endif
}
```
