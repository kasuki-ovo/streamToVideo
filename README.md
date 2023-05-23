# streamToVideo

#### 介绍
Vue，rtsp/rtmp视频流播放解决方案，使用ffmpeg把rtsp/rtmp视频流媒体转为flv格式，再用flv.js播放

#### 软件架构
Vue、ffmpeg


#### 安装教程

npm install

#### 使用说明
  本方案为vue项目，可分为前端和服务端，这里主要重点说一下服务端，服务端使用的是WebSocket服务，根据ffmpeg工具实现实时转流，对实时流进行格式转换输出flv格式，前端使用flv.js插件播放flv格式的视频，如果没有安装ffmpeg工具，请先安装！

1.  服务端启动
```
npm run serve
```
2.  前端启动
```
npm run start
```

#### 预览图

