## Video.js介绍

Video.js是一个开源的H5播放库，用于在网页上播放视频

### 主要功能

1. 跨平台兼容性：Video.js 支持主流的现代浏览器（包括 Chrome、Firefox、Safari、Edge 等）和移动设备，兼容不同操作系统和平台。
2. 自定义界面：Video.js 允许开发者自定义播放器的外观和行为，包括控制栏、进度条、音量控制等，以适应特定的设计需求和品牌风格。
3. 强大的扩展性：Video.js 提供了丰富的插件和组件生态系统，使开发者可以扩展播放器的功能，例如添加广告支持、字幕显示、视频品质切换等。
4. 支持多种视频格式：Video.js 支持常见的视频格式，如 MP4、WebM、Ogg，并提供适配流媒体协议如 HLS 和 DASH。
5. 自适应比特率：Video.js 支持自适应比特率（Adaptive Bitrate）播放，根据网络状况动态调整视频的质量和码率，以提供更流畅的播放体验。
6. 交互式事件和 API：Video.js 提供了丰富的事件和 API，开发者可以监听播放器的各种状态变化、控制播放进度、暂停/播放视频等，以实现自定义的交互和功能。
7. 广告支持：Video.js 提供了广告插件和 API，使开发者能够在视频播放期间插入和管理广告内容。

### 视频源格式

- MP4: `type="video/mp4"`
- WebM: `type="video/webm"`
- Ogg: `type="video/ogg"`
- HLS: `type="application/x-mpegURL"`
- DASH: `type="application/dash+xml"`
- FLV: `type="video/x-flv"`
- RTMP: 可以使用 `type="rtmp/mp4"` 或 `type="rtmp/flv"` 

### 使用方式

data-setup是一个json字符串，表示videojs的options选项

#### 嵌入video标签

```
<!-- via data-setup -->
<video id="vid1" class="video-js" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video>

<!-- via code -->
<video id="vid1" class="video-js">
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video>
const player = videojs('vid1', {});
```

#### player div接收

```html
<!-- via data-setup -->
<div data-vjs-player>
  <video id="vid1" class="video-js" data-setup='{}'>
    <source src="//vjs.zencdn.net/v/oceans.mp4">
  </video>
</div>

<!-- via code -->
<div data-vjs-player>
  <video id="vid1" class="video-js">
    <source src="//vjs.zencdn.net/v/oceans.mp4">
  </video>
</div>
const player = videojs('vid1', {});
```

#### 嵌入video-js标签

```
<!-- via data-setup -->
<video-js id="vid1" data-setup='{}'>
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video-js>

<!-- via code -->
<video-js id="vid1">
  <source src="//vjs.zencdn.net/v/oceans.mp4">
</video-js>
const player = videojs('vid1', {});
```

### 配合框架使用

配合Angular、Vue、React使用