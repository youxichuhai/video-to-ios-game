# video-to-ios-game

English | [简体中文](README.zh-CN.md)

A Codex Skill for turning a gameplay recording into a configuration-driven SwiftUI iOS game.

The workflow is intentionally staged:

1. Inspect the video and Xcode project.
2. Require a recording with at least three playable levels.
3. Write one evidence-based requirements document.
4. Pause for one user confirmation.
5. Implement the shell, mechanic, and level data.
6. Build, test, visually inspect, and prepare an open-source-friendly handoff.
7. Optionally add AdMob after the core game passes: gameplay banner, automatic interstitial from the fifth ordered level, and explicit-action rewarded ads for tools.

The level schema and templates are in `references/`. The Skill does not contain private recordings, extracted reference assets, ad credentials, or machine-specific paths.

AdMob is intentionally a post-process. It uses Debug test IDs, owner-supplied Release configuration, graceful no-ad fallbacks, and reward idempotency; see `references/admob-postprocess.md`.

## Example

The [`examples/word-spin/`](examples/word-spin/) folder demonstrates the complete flow with the original gameplay recording as input and a new iOS Simulator recording as output.

### Original input

[Watch `wordspin.mp4`](examples/word-spin/original-wordspin.mp4)

### Generated iOS game

<video src="https://raw.githubusercontent.com/youxichuhai/video-to-ios-game/main/examples/word-spin/generated-word-spin.mp4" controls width="300"></video>

[Open the generated recording](examples/word-spin/generated-word-spin.mp4)

### Level comparison snapshots

Each image shows the original recording on the left and the generated iOS game on the right.

#### Level 10 — STAR

![Level 10 comparison](examples/word-spin/level-10-comparison.jpg)

#### Level 11 — SNEAKER

![Level 11 comparison](examples/word-spin/level-11-comparison.jpg)

#### Level 12 — APPLE

![Level 12 comparison](examples/word-spin/level-12-comparison.jpg)

## Local use

Copy the `video-to-ios-game` directory into the Codex skills directory, or reference this directory explicitly from a local Codex setup. The target project should provide an Xcode project and a gameplay video. The workflow uses Xcode command-line tools and, when available, `ffprobe`/`ffmpeg` for media inspection.

Before publishing this Skill or the app it helps create, add the repository’s chosen open-source license and verify the rights for every video-derived asset.
