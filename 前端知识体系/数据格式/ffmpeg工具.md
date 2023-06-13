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

- rtmp转hls

```bash
ffmpeg -i rtmp://localhost:1935/live -c:v libx264 -preset veryfast -c:a aac -f hls -hls_time 2 -hls_list_size 6 -hls_flags delete_segments -hls_segment_filename D:/video/stream_%v/segment_%03d.ts D:/video/playlist.m3u8
```

- rtmp转多码率hls，windows的bat脚本

```bash
@echo off

set input=rtmp://localhost:1935/live
set output_path=D:/video

set bitrate1=800k
set bitrate2=1200k
set bitrate3=2000k

ffmpeg -i %input% -c:v libx264 -preset veryfast -c:a aac ^
-b:v %bitrate1% -hls_time 10 -hls_list_size 6 -hls_flags delete_segments ^
-hls_segment_filename %output_path%/%bitrate1%/stream_%%03d.ts %output_path%/%bitrate1%/playlist.m3u8 ^
-b:v %bitrate2% -hls_time 10 -hls_list_size 6 -hls_flags delete_segments ^
-hls_segment_filename %output_path%/%bitrate2%/stream_%%03d.ts %output_path%/%bitrate2%/playlist.m3u8 ^
-b:v %bitrate3% -hls_time 10 -hls_list_size 6 -hls_flags delete_segments ^
-hls_segment_filename %output_path%/%bitrate3%/stream_%%03d.ts %output_path%/%bitrate3%/playlist.m3u8

pause
```

- bash脚本多个rtmp转hls

```bash
@echo off
@REM 启用延期变量扩展
setlocal enabledelayedexpansion

set base_input=rtmp://localhost:1935/live
set base_output=E:/stream/live

set bitrate1=100k
set bitrate2=1000k

for /L %%N in (1,1,8) do (
  set input=%base_input%%%N
  set output=%base_output%%%N
  ffmpeg -i !input! -c:v libx264 -preset veryfast -c:a aac ^
  -b:v %bitrate1% -s 158x106 -hls_time 2 -hls_list_size 6 -hls_flags delete_segments ^
  -hls_segment_filename !output!/low/stream_%%13d.ts !output!/low/index.m3u8 ^
  -b:v %bitrate2% -s 1280x720 -hls_time 2 -hls_list_size 6 -hls_flags delete_segments ^
  -hls_segment_filename !output!/high/stream_%%13d.ts !output!/high/index.m3u8
)

pause
```



## ffmpeg应用

### ffmpeg+nginx+video,rtsp转rtmp

### ffmpeg+video,rtsp转hls