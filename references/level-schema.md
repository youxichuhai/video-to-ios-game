# Configuration-Driven Level Schema

Use a single replaceable content source. JSON is the default because it is easy to review, diff, validate, and load from an app bundle. CSV/XLSX may be an authoring format, but normalize it into the app’s canonical schema during implementation.

## Word Spin-style canonical shape

```json
{
  "schemaVersion": 1,
  "gameplay": {
    "id": "word-spin",
    "target": { "kind": "row", "index": 0 },
    "move": { "kind": "cyclic-line-shift", "step": 1 },
    "defaultHintCount": 5
  },
  "levels": [
    {
      "id": 10,
      "image": "StarImage",
      "answer": "STAR",
      "rows": 3,
      "columns": 4,
      "layout": [".S..", "R.A.", "T..."],
      "difficulty": "standard",
      "hintCount": 5,
      "targetRow": 0
    },
    {
      "id": 11,
      "image": "SneakerImage",
      "answer": "SNEAKER",
      "rows": 5,
      "columns": 7,
      "layout": ["..EE..K", ".......", "...A...", "S....NR", "......."],
      "difficulty": "hard",
      "hintCount": 5,
      "targetRow": 0
    },
    {
      "id": 12,
      "image": "AppleImage",
      "answer": "APPLE",
      "rows": 5,
      "columns": 5,
      "layout": ["AP...", ".....", "...P.", "...L.", "....E"],
      "difficulty": "standard",
      "hintCount": 5,
      "targetRow": 0
    }
  ]
}
```

The example reflects the reusable mechanic found in the reference project; replace content with the levels observed in the user’s video. The schema is not a license to invent answers or assets.

## Field rules

- `schemaVersion` is required and must be bumped only for a breaking data change.
- `gameplay.id` selects the rule adapter. A new mechanic gets a new adapter instead of hidden level-specific branches.
- `id` is a unique, stable, ascending level identifier.
- `image` is an asset-catalog name, never an absolute filesystem path or remote URL.
- `answer` is normalized at load time according to the game’s case/locale rules.
- `rows` and `columns` are positive integers. `layout` must contain exactly `rows` strings, each exactly `columns` characters wide.
- In the Word Spin adapter, `.` means an empty cell. The non-empty letters must have the same multiset as `answer`; repeated letters are valid.
- `targetRow` must be in range and have room for the answer. The solved predicate must be implemented in the rule layer, not duplicated per level.
- `difficulty` and `hintCount` are content values; they must not change the movement rules.
- Asset references must resolve to bundled assets or be explicitly marked as placeholders during development.

## Contributor contract

Adding a level should mean:

1. Add one valid object to the level source.
2. Add an approved image/audio asset using the exact referenced name.
3. Run the repository’s data validator and focused tests.

If a contributor must edit SwiftUI views or the rule engine to add a normal level, the content boundary is not finished and should be refactored before publishing.
