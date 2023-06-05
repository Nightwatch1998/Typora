## ffmpeg工具 

### 简介

ffmpeg是一套可以用来记录、转换数字音频、视频，并可转化为流。

组成部分：

- ffmpeg：命令行工具，用来对视频文件进行格式转换
- ffplay：是一个简单的播放器，使用ffmpeg库解析和解码
- ffprobe：收集多媒体文件和流的信息，并以人和机器可读的方式输出
- libavformat： 各种音视频封装格式的生成和解析
- libavcodec：声音图像编解码
- libavutil：公共的工具函数
- libswscale：视频场景比例缩放、色彩映射转换
- libpostproc：后期效果处理



### 安装

- 安装ffmpeg工具，下载win64版本，bin目录添加至环境变量

```
https://github.com/BtbN/FFmpeg-Builds/releases
```

### ffplay使用指南

https://zhuanlan.zhihu.com/p/582884983

### ffmpeg选项

主要选项

```
-f fmt (input/output) 指定输入或者输出文件格式
-i filename 指定输入文件
-y 默认自动覆盖输出文件，不再确认
-t duration (input/output) 限制输入/输出的时间,单位为秒
-timestamp 容器中记录时间戳
-codec  选择编解码模式
```

视频选项

```
-vframes number 输出文件的帧数
-r 设置帧率
-s 设置帧尺寸
-vn 禁止输出视频
-vcodec 设置视频编码器
aspect 指定横纵比
```

音频选项

```
-aframes number 音频帧输出
-ar 音频采样率
-aq 音频品质
-ac 通道数
-an 禁止输出音频
-acode 音频编解码器
```

字幕选项

### ffmpeg命令示例



## ffmpeg应用

### ffmpeg+nginx+video,rtsp转rtmp

1. 下载视频流测试工具[**vlc**](https://get.videolan.org/vlc/3.0.6/win64/vlc-3.0.6-win64.exe)
2. 安装nginx，[**下载地址**](http://nginx-win.ecsds.eu/download/)，选择nginx 1.7.11.3 Gryphon.zip
3. 下载nginx rtsp模块，[**github仓库**](https://github.com/illuspas/nginx-rtmp-win32)，这里是封装后的nginx.exe，配置文件包含了rtmp模块
4. 下载ffmpeg版本号 gcc7.3.0
5. 使用ffmpeg命令

```
// 转rtmp(失败了)
ffmpeg -i "rtsp流路径" -vcodec copy -acodec copy -f flv "rtmp://127.0.0.1:1935/live/"

// 转hls
ffmpeg -i "rtsp流路径" -c:v copy -c:a aac -hls_time 2 -hls_list_size 6 -f hls output.m3u8
```



### ffmpeg+video,rtsp转hls