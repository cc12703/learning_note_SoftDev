

# WebCodecs


## 概述
* 提供了一套比较底层的接口
* 直接访问浏览器的编码器与解码器


### 操作
* 视频与音频的解码
* 视频与音频的编码
* 原始视频帧
* 图片解码


## 类

### 视频
* `VideoFrame` 原始图像数据
* `EncodedVideoChunk` 压缩图像数据 
* `VideoEncoder` 图像编码器
* `VideoDecoder` 图像解码器


![](http://picbed.cc12703.com/20230911001142.png)


### 音频
* `AudioData` 原始音频数据
* `EncodedAudioChunk` 压图音频数据
* `AudioEncoder` 音频编码器
* `AudioDecoder` 音频解码器
