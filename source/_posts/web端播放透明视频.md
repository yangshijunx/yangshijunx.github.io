---
title: web端播放透明视频
date: 2023-10-08 21:59:41
categories: 
- web前端
tags:
- Javascript
- ffmpeg
- 视频处理
---

# web端播放透明视频

前两天做了一个官网的项目，产品要求在网页上放一个视频用来介绍这个网站，但是呢要求只保留介绍的人物，不能有多余的环境内容，效果大致是这样的。

![Untitled](/web端播放透明视频/Untitled.png)

产品拿来的效果是一个视频文件，所以没有办法判断参考对象采用的是建模还是直接就在视频上做的效果，仔细想一下直接播放视频不就可以了，想实现这样的效果首先播放器要透明，然后就需要在浏览器中播放带有透明通道的视频就可以了，ok逻辑理清了，就可以开工了。

### 第一步，播放器透明

好像不用哈哈哈哈，播放器就是透明的，不显示播放控件就可以了，那接下来主要的工作就是将介绍视频只保留人像就可以了，产品提供的是绿幕的人像视频使用pr的超级键处理之后，导出格式为mov的保留透明通道的视频就可以了。

### 第二步，将视频导出只包含人像

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%201.png)

在效果里面找到超级键，然后拖拽到视频轨道上面。

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%202.png)

点击视频轨道找到效果控件中的超级建选中绿幕，在这个过程中可以选择Alpha通道多次调整，宽容度等参数直到合适。

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%203.png)

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%204.png)

接着就可以导出视频了，Pr中可以选择QuickTime中具有透明通道的选项。

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%205.png)

不出意外，导出的视频是mov格式的，并且相比于原视频会变大很多，我这里两分钟的视频导出的视频3个多G，放到web端这不是开玩笑(别急)。

刚开始，想的是能不能通过压缩视频的形式实现，多番尝试之后失败了，单纯压缩mov视频不能实现，那我可不可以采用别的方式？想了好久，我突然想到之前在做数据可视化项目的时候UI使用AE导出过一个带有动效的图片，格式是webm，于是我想能不能把视频也转换成webm这种格式的，查了一下。

**WebM**是一个由[Google](https://zh.wikipedia.org/wiki/Google)资助的项目，目标是构建一个[开放](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E6%A1%A3%E6%A1%88%E6%A0%BC%E5%BC%8F)的、免著作权费用的[视频文件格式](https://zh.wikipedia.org/wiki/%E8%A7%86%E9%A2%91%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F)。该视频文件格式应能提供高质量的[视频压缩](https://zh.wikipedia.org/wiki/%E8%A7%86%E9%A2%91%E5%8E%8B%E7%BC%A9)以配合[HTML5](https://zh.wikipedia.org/wiki/HTML5)使用。 ——— 维基百科

这不是巧了吗，这不是刚好符合要求，现在唯一的问题就是不知道支不支持透明通道，这还不简单试一下就可以了，一开始采用的是线上转换的方式，这里如果视频比较小推荐使用[https://converter.app/mov-to-webm/](https://converter.app/mov-to-webm/) 这个平台的（线上转换比较慢，视频太大还有G的风险，后面还有一个方案），其他的诸如格式工厂或者别的在线转换平台不知道是不是我操作又问题，导出的webm都是黑底的。

实验结果webm支持透明通道，并且相对于原视频体积会小很多，并且清晰度没有损耗，效果如下：

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%206.png)

到此所有的问题基本上都解决了，还有一个就是mov转webm线上转换太慢的问题，一开始想的是能不能使用现成的软件解决，结果尝试了各种各样的软件都不行，原本透明的视频变成了黑底的，想了好久终于想到了一个贼牛的项目，**ffmpeg。**github地址：https://github.com/FFmpeg/FFmpeg。

windows的使用比较简单，官网提供的有现成的exe文件，下载下来就可以了，下载地址：[https://ffmpeg.org/download.html#build-windows](https://ffmpeg.org/download.html#build-windows)

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%207.png)

下载下来之后，配置环境变量方式使用命令行进行调用。

![Untitled](web%E7%AB%AF%E6%92%AD%E6%94%BE%E9%80%8F%E6%98%8E%E8%A7%86%E9%A2%91%201e904dc5cb264785b83de7d93d5cdaf5/Untitled%208.png)

转webm的命令如下，这样就可以保留透明通道。

```bash
ffmpeg -i input.mov -c:v libvpx-vp9 output.webm
```

到此完成，总结ffmpeg是一个非常强大的工具,之前还有一个项目用到了，还没做总结。。