
---
title: 七人以上视频场景
description: 
platform: Android
updatedAt: Sun Sep 29 2019 08:24:58 GMT+0800 (CST)
---
# 七人以上视频场景
## 功能描述

在一般的视频直播场景中，如果参与的主播超过七人，可能会引起音画不同步和信息丢失的问题。如果参与连麦的各方将订阅流设置为 1-N 模式，即订阅 1 路大流和 N 路小流，那么直播频道内的主播最多可以支持 17 人。本页为你展示如何使用 Agora SDK  实现七人以上视频互动直播和相关的注意事项。

> PC 端最多支持 17 人，移动端最多支持 7 人。

## 实现方法

在实现七人以上视频场景前，请确保已在你的项目中实现基本的实时音视频功能。详见[开始互动直播](../../cn/Interactive%20Broadcast/start_live_android.md)。

参考如下步骤，在你的项目中实现七人以上视频场景功能：

1. 加入频道前，调用 `setParameters("{"che.audio.live_for_comm":true}")` 开启多人视频直播模式。

> 如果你使用 v2.3.1 和之前版本的 SDK，你还需要调用 `setParameters("{\"che.video.moreFecSchemeEnable\":true}”) ` 开启 ULP FEC，提高视频数据传输的可靠性。

2. 加入频道后，直播频道内的主播各方调用 `enableDualStreamMode` 方法开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。

   > 开启双流模式后，SDK 会根据视频大流参数默认设置对应的视频小流参数。

3. 频道内主播各方调用 `setRemoteVideoStreamType` 方法将订阅的一路视频流设置为大流，其余路视频流均设置为小流。

   > 大流理论上可以设置为所有支持的视频属性（Video Profile），但建议使用不超过 640 x 480，15 fps 的视频属性，例如：
   >
   > <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>分辨率</strong></td>
<td><strong>帧率</strong></td>
<td><strong>比特率</strong></td>
</tr>
<tr><td>640 &times; 480</td>
<td>15 fps</td>
<td>500 Kbps</td>
</tr>
<tr><td>640 &times; 360</td>
<td>15 fps</td>
<td>400 Kbps</td>
</tr>
<tr><td>640 &times; 360</td>
<td>24 fps</td>
<td>800 Kbps</td>
</tr>
</tbody>
</table>

4. （可选）调用 `setParameters("{"che.video.lowBitRateStreamParameter":{"width":<width>,"height":<height>,"frameRate":<frameRate>,"bitRate":<bitRate>}")` 定制视频小流参数设置。

   ```java
   // 定制视频小流参数设置。设置 Video Profile 为 320 (px) × 180 (px), 15 fps, 140 Kbps。
   setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}"；
   ```

   > 小流的分辨率（宽高）比例需要和大流的分辨率（宽高）比例相同。
   

### 示例代码

我们在 Github 提供一个开源的 [Large-Group-Video-Chat](https://github.com/AgoraIO/Advanced-Video/tree/master/Large-Group-Video-Chat) 示例项目。

### API 参考

- [`enableDualStreamMode`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a645cb7d0f3a59dda27b157cf130c8c9a)
- [`setRemoteVideoStreamType`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a51756b4d2e7997fbe6481d2deb5c0396)
- [`setParameters`](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab07f44c88e788ce59ba1858b522dc4ad)

## 开发注意事项

- Agora 建议你在界面布局里采用一大多小的窗口布局：大窗口申请大流；小窗口申请小流。

- 若有主播离线，频道内其他主播可以在收到该主播离线回调后，调用 `setupRemoteVideo` 方法并将该主播的 `view` 设为空，应用程序即可彻底释放该主播占用的 `view`。

