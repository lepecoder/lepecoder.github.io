---
title: linux下视频转gif
date: 2017-11-23T17:27:00
tags: ['Linux']
categories: 工具
---

安装`ffmpeg`

ffmpeg是一套非常强大的音视频录制 转换工具,今天只讨论怎样用它把视频转化为gif动态图,因为在用markdown写博客时偶尔会需要插入录制的视频,转化为gif后会方便很多.

```

ffmpeg -i input.mkv out.gif

```

简单到一行命令就可以完成,不过它还有很多非常强大的功能,例如更改视频速率

假设原来的视频是60fps,你想要两倍播放,那么你可以:

```

ffmpeg -r 60 -i input.mkv -r 30 out.gif

```

将fps设置为原来的一半会丢掉一半的帧,播放速度也会变为原来的两倍,如果不想丢帧的话,还可以将输入的帧速率扩大一倍.



```

ffmpeg -r 120 -i input.mkv -r 60 out.gif

```

同样可以达到两倍播放的目的.
    