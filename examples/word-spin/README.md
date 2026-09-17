# Word Spin Example

English | [简体中文](README.zh-CN.md)

This example shows the input/output pair for the `video-to-ios-game` workflow.

| Stage | File | Description |
|---|---|---|
| Original input | [`original-wordspin.mp4`](original-wordspin.mp4) | The reference gameplay recording used to analyze the mechanic and levels. |
| Generated output | [`generated-word-spin.mp4`](generated-word-spin.mp4) | An iOS Simulator recording of the SwiftUI game generated from the confirmed requirements. |

The repository copy of the original input is compressed from the supplied `wordspin.mp4` for practical distribution; the source file itself remains unchanged. The generated MP4 is a browser-friendly conversion of the supplied `Simulator Screen Recording - iPhone 17 Pro.mov` recording. Verify that the reference video has redistribution permission before publishing a public repository.

## Level comparison snapshots

The original recording is on the left and the generated iOS game is on the right.

### Level 10 — STAR

![Level 10 comparison](level-10-comparison.jpg)

### Level 11 — SNEAKER

![Level 11 comparison](level-11-comparison.jpg)

### Level 12 — APPLE

![Level 12 comparison](level-12-comparison.jpg)

## Flow represented

```text
original gameplay video
        ↓
requirements document
        ↓
configuration-driven SwiftUI game
        ↓
simulator recording
```
