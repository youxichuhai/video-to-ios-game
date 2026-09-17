# Open-Source Release Checklist

Use this before the final handoff for a repository intended to be public. The code license and media rights are separate decisions.

## Repository hygiene

- [ ] README explains prerequisites, Xcode version/deployment target, how to open/build/test, and how to add a level.
- [ ] The requirements document records the mechanic and the one-time confirmation outcome.
- [ ] Repository-relative paths are used in docs and scripts; no personal home-directory paths appear.
- [ ] `xcuserdata/`, DerivedData, simulator screenshots, local logs, `.DS_Store`, and editor state are ignored or untracked.
- [ ] No passwords, tokens, certificates, provisioning profiles, private URLs, or personal identifiers are committed.
- [ ] Bundle IDs, app names, and service settings are clearly marked as examples when they are not intended for reuse.

## Ads and services

- [ ] Debug uses platform/vendor test IDs where an ad SDK is included.
- [ ] Release production IDs are supplied by the app owner through documented configuration, not embedded in a reusable Skill or template.
- [ ] Banner, interstitial, and rewarded unit IDs are kept in an explicit configuration boundary; the public repository contains no private account values.
- [ ] The fifth-level interstitial rule is based on ordered level position and has a no-ad fallback.
- [ ] Rewarded callbacks are idempotent and grant nothing when the ad is closed or fails.
- [ ] Optional services fail without moving or blanking the game layout.
- [ ] Third-party SDK licenses and notices are documented.

## Video and asset rights

- [ ] The source video is either excluded from the repository or distributed under a license that permits redistribution.
- [ ] Extracted frames, crops, illustrations, fonts, sounds, and icons have documented provenance.
- [ ] User-provided or client-owned media is not treated as open-source merely because the Swift code is open-source.
- [ ] Unresolved assets are replaced by original/placeholder assets or clearly documented as local-only files.
- [ ] The Skill itself ships instructions and schemas, not private recordings, extracted frames, or credentials.

## Reproducibility

- [ ] A clean checkout can load the project without the author’s local filesystem.
- [ ] The canonical level source is bundled and validated at build/test time or by a documented command.
- [ ] Build, unit-test, and UI-smoke-test commands are documented and pass on the supported setup.
- [ ] The app still launches if optional ads/audio are unavailable.

Before publishing, choose and add an OSI-approved code license appropriate to the repository. Do not use that code license to make a separate claim about the rights to reference-video media.
