---
layout: post
title:  android蓝牙耳机及手咪开发
date:   2022-04-11 09:35:22 +0800
categories: bluetooth
---


最近公司在做蓝牙手咪开发，功能就是通过蓝牙手咪麦克风作为音频输入，自己找了很多资料才终于解决，其实方法很简单，这里做一个总结，希望对相关开发的人员有帮助。

代码如下：

//获取音频管理器和蓝牙适配器
```
AudioManager mAudioManager = null;
BluetoothAdapter mBluetoothAdapter = null;

mAudioManager = (AudioManager) this.getSystemService(Context.AUDIO_SERVICE);
mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter(); 

//只要通过以下两个方法就可以设置蓝牙手咪麦克风作为音频输入，但是这两个方法需要权限

mAudioManager.setBluetoothScoOn(true);
mAudioManager.startBluetoothSco();
//此开发所有权限包括

    <uses-permission android:name="android.permission.BLUETOOTH"></uses-permission>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"></uses-permission>
    <uses-permission android:name="android.permission.BROADCAST_STICKY" />

  <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS"/>
```
原文链接：https://blog.csdn.net/u014428442/article/details/41514887