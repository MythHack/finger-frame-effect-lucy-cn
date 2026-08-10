# 手指取景框 · 实时 AI 🎬⚡

**在线体验：https://sophiamyang.github.io/finger-frame-effect-lucy/**

举起双手，用手指框出一个区域——你会在取景框内部看到一个**实时生成的 AI 世界**。画面由 [Decart Lucy 2.5](https://docs.platform.decart.ai/models/realtime/lucy-2.5) 以 30fps 实时生成（通过 WebRTC 进行实时视频到视频转换）。与离线生成不同，AI 版本会真正跟随你的动作：你眨眼，它也会眨眼；你挥手，它也会挥手，模型延迟只有几十毫秒量级。

## 手指取景框系列

| 应用 | 生成方式 | 延迟 |
|---|---|---|
| [finger-frame-effect](https://sophiamyang.github.io/finger-frame-effect/)（[仓库](https://github.com/sophiamyang/finger-frame-effect)）— 实时摄像头、本地效果 | Canvas 2D（梵高、卡通、故障等） | 无 |
| [finger-frame-effect-ai](https://sophiamyang.github.io/finger-frame-effect-ai/)（[仓库](https://github.com/sophiamyang/finger-frame-effect-ai)）— 录制视频、AI 风格转换 | Gemini Omni Flash（离线视频编辑） | 数分钟 |
| **本应用** — 实时摄像头、实时 AI | Decart Lucy 2.5（实时视频到视频） | 接近实时 |

![示例：手指取景框中的实时 AI 世界](examples/lucy.gif)

*实时录制——取景框内的 AI 世界会随着摄像头画面实时运动（[高质量 MP4](examples/lucy.mp4)）。*

## 工作原理

- 摄像头画面（1280×720@30）会以镜像方式绘制到全屏 Canvas，同时 MediaPipe Hand Landmarker 使用原版应用中的追踪流程持续计算手指取景框四边形，包括固定解剖角点顺序、手势迟滞、瞬移拒绝、速度自适应平滑与短暂丢失保持。
- 同一摄像头视频流会通过 WebRTC 发送给 Lucy 2.5；转换后的实时视频流返回后，会按屏幕坐标对齐绘制在手指形成的四边形内部，因此手指取景框就像一扇通往 AI 世界的窗口。
- 效果（数字键 1–6）本质上是**实时风格提示词**：切换效果时，会把新的提示词直接发送到当前正在运行的会话，无需重新连接。`自定义 ✨` 会使用 🔑 面板中填写的自定义提示词。

## 使用自己的 API 密钥

在 [platform.decart.ai](https://platform.decart.ai/) 获取密钥，然后粘贴到 🔑 面板中。密钥只保存在浏览器里，并且仅用于与 Decart 建立 WebRTC 会话。

如果没有密钥，取景框内部会自动回退为本地颜色滤镜，因此仍然可以演示和测试手部追踪效果。

## 本地运行

```bash
python3 -m http.server 8125
```

打开 http://localhost:8125 并允许摄像头访问。

访问 `?demo` 可以启用合成测试画面和模拟手部关键点，从而在没有摄像头的情况下测试追踪流程；演示模式下会禁用 AI。
