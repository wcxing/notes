### 一、使用 xgplayer 做flv直播
#### 需求：使用 xgPlayer 插件做 flv 协议些直播。原理是video标签的 mse（Media Source Extensions） 能力。

#### 前期调研结果

- ios 不支持 mse，所以 ios 不能使用，以下所说机型都是 安卓；
- 微信端有些机型观看直播时卡顿（有无x5内核都卡）；
- 微信端有些机型在开启 x5 内核后观看直播卡顿，关闭 x5 内核可正常观看；x5 内核本身就支持
- 业界小鹅通并不支持学生在微信端上课；
- 业界b站微信端只看到点播和回放，无直播；

[腾讯浏览器-视频能力](https://x5.tencent.com/tbs/product/video.html)

### 二、使用 xgPlayer 做点播
#### 使用 xgPlayer 做点播
使用 xgPlayer 做点播时要注意，视频源是 mp4 还是 m3u8 。MP4 需要使用 xgplayer-mp4 插件，m3u8 需要使用 xgplayer-hls.js 插件（兼容性比 xgplayer-hls 好）。可以根据不同的视频源初始化不同的播放器。

#### 遇到的问题
1. 最初用的 xgplayer-mp4 插件初始化的播放器，测试时不能播，后来发现是视频源格式是 m3u8 。切换为 xgplayer-hls.js 插件初始化播放器后可以正常播。

2. 在使用 xgplayer-hls.js 播放 m3u8 视频时，所有的 controls 控件的图标样式都乱了。发现，播放 m3u8 时，video 播放器的父元素上少了名为 xgplayer-skin-default 的 class。加上后样式正常了。

3. 业务方在测试时，发现会有小窗播放，且有广告。可能需要添加相应的配置来阻止小窗播放。