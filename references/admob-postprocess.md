# AdMob Post-Process

This reference applies only after the core video-derived app is complete and verified. It describes the default integration requested for this Skill; it does not require ads in every generated app.

## Order of work

1. Confirm the core game already builds and its rule/config/UI checks pass.
2. Inspect the project’s package manager, app entry point, Info.plist/configuration, deployment target, and any existing consent/privacy implementation.
3. Add the current official Google Mobile Ads SDK using the project’s existing dependency convention. Consult the SDK’s current official documentation for API and policy changes instead of copying stale setup code.
4. Add an `AdService` or equivalent coordinator. Keep SDK objects, loading state, presentation, and callbacks out of the level model and rule engine.
5. Wire the three placements below.
6. Run core tests again, then run ad-specific tests/smoke checks and a Debug simulator pass with test IDs.

Do not pause for another product confirmation after the requirements document. The user’s request for these three placements is the ad specification. If production IDs, consent choices, or a required account setting are missing, keep Debug usable with test IDs/placeholders and report the Release blocker precisely.

## Placement contract

### 1. Gameplay banner

- Show on the gameplay screen near the bottom safe area.
- Use a fixed overlay/container outside the board’s layout constraints so loading, failure, or rotation does not move the puzzle.
- When the ad fails or is unavailable, hide it and reclaim the reserved space cleanly; do not show a permanent blank ad box.
- Do not cover the board, gesture target, hint button, or home-indicator area.
- Keep banner state independent of level state. Leaving a level must stop or detach the banner cleanly.

### 2. Automatic interstitial

- Define “fifth level” by ordered playable position, not by an arbitrary level ID. The default condition is ordinal position `>= 5` after a successful completion.
- Present automatically only after the success state is committed and the level is no longer accepting moves.
- Prefer preloading opportunistically. If an interstitial is not ready, continue the normal completion/next-level flow without delay.
- After dismissal, presentation failure, or an SDK callback indicating the ad is unavailable, resume the pending next action exactly once.
- Do not present on the first four levels, failed attempts, reset, app launch, level selection, or while another ad is visible.
- Guard against duplicate completion callbacks so one level completion cannot advance twice or present two interstitials.
- Keep the completion overlay/next-level action coherent if the ad is skipped or fails.

### 3. Rewarded ad for a tool

- Present only after an explicit user action, such as tapping a hint/assist button.
- Explain the reward briefly in the UI before presentation when the product has a choice of copy.
- Grant the tool exactly once and only after the SDK reports that the user earned the reward. Closing, timeout, load failure, or presentation failure grants nothing.
- Disable or hide the action while loading and while another ad is presented; restore it after dismissal/failure.
- Keep the reward idempotent. A repeated SDK callback must not grant multiple hints, moves, lives, or other items.
- If no rewarded ad is ready, keep the tool’s normal fallback behavior explicit: either show a “not available” state or allow the non-ad version only if that behavior is already part of the confirmed requirements.

## Configuration and privacy

Use one typed configuration boundary for:

- AdMob application ID;
- banner unit ID;
- interstitial unit ID;
- rewarded unit ID;
- enable/disable flags and optional debug diagnostics.

Keep Debug and Release values separate. Debug must use Google’s test identifiers or an equivalent documented test configuration. Release must require app-owner-supplied production identifiers. Do not place personal IDs, secrets, or account-specific values in this public Skill, a reusable template, or committed sample configuration.

Review the current SDK and App Store requirements for consent/UMP, App Tracking Transparency, privacy manifests, age/child-directed treatment, and data disclosures when applicable. Do not claim compliance merely because the SDK compiles; document what the generated app still needs from its owner.

## Test and failure matrix

At minimum, verify:

| Case | Expected behavior |
|---|---|
| Banner loads | It appears at the bottom without moving board content. |
| Banner fails | It is hidden; gameplay remains usable with no blank reserved area. |
| Level 4 success | No automatic interstitial. |
| Level 5 success with ready ad | One interstitial presents, then normal completion/next flow resumes once. |
| Level 5 success without ready ad | Normal completion/next flow proceeds immediately. |
| Interstitial dismissed or fails | Pending game action resumes once. |
| Rewarded ad earned | Exactly one configured tool reward is granted. |
| Rewarded ad closed/failed | No reward is granted and the control recovers. |
| App background/foreground | No duplicate presentation or stuck loading state. |

Test the coordinator with an injectable fake ad provider where practical. UI smoke tests should assert stable gameplay controls and level progression, not a live network response. Live ad traffic is not a reliable CI test.

## Handoff requirements

Report:

- SDK/dependency and configuration files changed;
- banner, interstitial, and rewarded placements wired;
- Debug test-ID status and whether Release IDs are placeholders or owner-supplied;
- consent/privacy work completed versus still required;
- ad-specific tests and simulator checks;
- fallback behavior when ads are unavailable.
