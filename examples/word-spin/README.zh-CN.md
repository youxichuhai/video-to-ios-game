# Word Spin 示例

[English](README.md) | 简体中文

这个示例展示 `video-to-ios-game` 的输入和输出对应关系。

| 阶段 | 文件 | 说明 |
|---|---|---|
| 原版输入 | [`original-wordspin.mp4`](original-wordspin.mp4) | 用于分析玩法机制和关卡内容的原始游戏录屏。 |
| 生成结果 | [`generated-word-spin.mp4`](generated-word-spin.mp4) | 根据确认后的需求生成的 SwiftUI 游戏 iOS Simulator 录屏。 |

仓库中的原版输入是根据用户提供的 `wordspin.mp4` 压缩后的副本，原始源文件没有被覆盖。生成结果 MP4 是将用户提供的 `Simulator Screen Recording - iPhone 17 Pro.mov` 转换成更适合浏览器播放的格式。公开仓库发布前，请确认参考视频具有可公开再分发的授权。

## 展示的流程

```text
原版游戏录屏
        ↓
需求文档
        ↓
配置化 SwiftUI 游戏
        ↓
Simulator 录屏
```
