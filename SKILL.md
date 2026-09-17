---
name: video-to-ios-game
description: Analyze a gameplay video with at least three playable levels, produce one confirmable requirements spec, then implement a configuration-driven SwiftUI iOS app in an existing Xcode project and verify it. Use when turning a recorded iOS game flow into a reusable, open-source-friendly app; not for ordinary video editing or one-off UI mockups.
---

# Video To iOS Game

Turn a gameplay recording into a working iOS game while preserving a clean boundary between the reusable app shell, the gameplay rules, and level content.

Read the supporting references only when needed:

- Read [requirements-spec-template.md](references/requirements-spec-template.md) when producing the video breakdown document.
- Read [level-schema.md](references/level-schema.md) when creating or validating the data-driven level source.
- Read [admob-postprocess.md](references/admob-postprocess.md) only when the user requests AdMob integration after the core app is complete.
- Read [open-source-checklist.md](references/open-source-checklist.md) before the final handoff or any commit intended for publication.

## Non-negotiable workflow

1. Locate the user-provided video and the Xcode project. Do not assume file names, schemes, bundle identifiers, simulator IDs, or directory paths.
2. Confirm that the recording contains at least three distinct, playable levels. A level must show a recognizable puzzle state and interaction or completion transition, not merely three screenshots. If fewer than three levels are present, stop and ask for a longer or additional recording.
3. Analyze the video and write one requirements document, normally `docs/video-to-ios-game-requirements.md` in the app repository. Separate observations supported by frames from inferences. Include level coverage, state transitions, interaction semantics, feedback, assets, and acceptance criteria.
4. Ask for one product confirmation after the document is ready. Use a concise request such as: “请确认 `docs/video-to-ios-game-requirements.md`；确认后我会按文档实现并验证。” Do not ask a second round of design questions. If the user already explicitly confirmed this exact document in the current task, treat the checkpoint as satisfied.
5. After confirmation, inspect the project and implement the app. Make reasonable reversible assumptions and record them in the document. Ask again only for a hard blocker that cannot be represented with a safe placeholder, such as a missing source project or an essential credential required to compile.
6. Verify the result with a build, focused unit tests, and at least one UI smoke test. Perform simulator visual QA when a simulator is available.
7. If AdMob was requested, integrate it only after the core app passes its checks. Follow [admob-postprocess.md](references/admob-postprocess.md); this is a post-processing phase, not a second product-confirmation checkpoint.

The requirements confirmation is the only normal product-decision checkpoint. Operational errors, missing inputs, and rights issues are blockers or handoff notes, not extra design-review cycles.

## Analyze the recording

Use the cheapest reliable combination of `ffprobe`, `ffmpeg` frame sampling, OCR when useful, and visual inspection. Sample around the start, active play, move feedback, solved state, and transition of every candidate level. Record timestamps and confidence rather than guessing from a single frame.

Extract these facts:

- app launch, home, level selection, gameplay, success, failure, pause, and next-level states;
- the player goal, legal gestures/taps, move granularity, and whether actions are cyclic, bounded, or undoable;
- board dimensions, fixed targets, movable pieces, counters, hints, progress indicators, timers, and difficulty labels;
- success/failure conditions and the exact feedback sequence;
- observed level IDs, image clues, answers or labels, layouts, and asset references;
- visual tokens worth reproducing: spacing, colors, typography, corner radius, markers, animation, confetti, sound cues, and safe-area behavior.

Keep a small evidence table in the requirements document. Mark each statement as `observed`, `inferred`, or `needs confirmation`. Never fabricate a level that is not visible in the recording; use a placeholder only after the user confirms the spec.

## Preserve the project boundary

Inspect the existing repository before editing with `rg --files` and `rg`. Classify what already exists:

- Shell: app entry point, launch/home flow, level entry, saved progress, result/next/restart flow, ads, and global styling.
- Game module: level model/parser, rule engine or ViewModel, board interaction, feedback, win/loss logic, and module tests.
- Content: level data, image/audio assets, and their provenance.

If the user points to the current project, modify that project in place and preserve unrelated work. If they request a new package, create a separate package while leaving the source game available as reference. Reuse a proven shell when it exists, but do not invent production ad IDs, credentials, or services that are absent.

For a SwiftUI game, keep shell-owned concerns out of the gameplay module. The module should expose deterministic state transitions that can be tested without rendering. Major controls and the board need stable accessibility identifiers/labels for UI smoke tests.

## Make levels configuration-driven

The success criterion is that adding a level does not require changing gameplay logic or view code. Prefer a bundled JSON/CSV source with a typed Swift model and validation; use a typed Swift repository only when the project cannot safely bundle external data, and keep all level content in one replaceable source.

Every level source must validate:

- schema version and required fields;
- unique, ordered IDs;
- dimensions matching the layout;
- normalized answer and piece multiset matching the board;
- legal target positions and move parameters;
- referenced images/audio existing in the asset catalog or being explicitly marked as a placeholder;
- at least the three levels used to validate the mechanic.

For the Word Spin-style mechanic from the reference project, the canonical data shape is in [level-schema.md](references/level-schema.md): an image clue, answer, rectangular board, scattered letters, target row, difficulty, and hint count. Implement row/column cyclic movement, progress, hints, reset, and completion as rules—not as level-specific branches. If a future recording has a materially different mechanic, create a small mechanic adapter and keep content fields data-driven instead of forcing incompatible rules into this schema.

## Implement and verify

After the single confirmation:

1. Normalize the observed levels into the canonical config and add only assets whose redistribution rights are clear. Keep raw video, extracted frames, and uncertain assets outside the distributable source.
2. Implement the smallest complete state machine: initial state, legal input, state update, progress, success/failure, reset, hint/assist, persistence, and next-level behavior.
3. Match the observed interaction and feedback before polishing. Do not add timers, lives, scoring, monetization, or screens that were not supported by the recording unless clearly labeled as an assumption in the spec.
4. Run the project’s discovered build command, focused rule/config tests, and one launch/start-level UI test. Capture a simulator screenshot or recording for visual QA when possible.
5. Fix real failures and rerun the affected checks. Report unavailable tools or destinations with the exact command and reason.

Use the existing project’s conventions and deployment target. Avoid hard-coding personal absolute paths, machine-specific simulator identifiers, generated DerivedData, or local user settings into source or documentation.

## Optional AdMob post-process

Run this phase only after the video-derived game is implemented, builds, and has passing focused tests. The three requested placements are the default ad plan:

- a fixed banner at the bottom of the gameplay page;
- an automatically presented interstitial after each successful level from the fifth ordered level onward;
- a user-triggered rewarded ad for an in-game tool or assist.

Use a small ad service/coordinator boundary so the gameplay rules do not depend directly on an ad SDK. Ad availability must never block a level, move the board, reserve blank layout space, or prevent reset/next-level behavior. Debug must use vendor test IDs. Release values must come from the app owner’s explicit configuration; never invent or commit production IDs. Read the detailed sequencing, reward, failure, and verification rules in [admob-postprocess.md](references/admob-postprocess.md).

## Open-source handoff

Before publication, apply [open-source-checklist.md](references/open-source-checklist.md). In particular:

- remove personal paths, usernames, `xcuserdata`, simulator IDs, secrets, production ad IDs, and private service configuration;
- do not commit the source recording or extracted images by default; document asset provenance and licensing instead;
- keep Debug ad configuration on test IDs and require explicit production values for Release when ads are part of the user’s app;
- document setup, build/test commands, the level schema, and how to add a level without changing code;
- include a project license and third-party notices appropriate to the repository, without implying that reference-video media is covered by the code license.

## Final report

Report the outcome first, then concise evidence:

- requirements document path and whether the one confirmation checkpoint was satisfied;
- changed project/package path and gameplay module files;
- config source, schema version, level count, and asset rights status;
- build, unit-test, UI-test, and visual-QA results;
- if requested, AdMob placement status, Debug/Release ID mode, and fallback behavior;
- assumptions, unresolved blockers, and the exact next action if anything remains.
