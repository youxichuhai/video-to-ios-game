# Video-to-iOS-Game Requirements Specification

Use this template for the single document the user confirms before implementation. Keep it factual and compact. Replace the `{{...}}` tokens; do not leave a fabricated value in the document.

```markdown
# {{App or game name}} — Video-to-iOS-Game Requirements

- Source video: `{{filename only}}`
- Analysis date: `{{YYYY-MM-DD}}`
- Video facts: `{{duration}}, {{resolution}}, {{frame rate}}`
- Xcode project: `{{repository-relative path}}`
- Status: Draft — awaiting the single implementation confirmation

## 1. Scope and confidence

### Observed

- {{facts directly supported by video frames or transitions}}

### Inferred

- {{implementation assumptions that explain observed behavior}}

### Needs confirmation

- {{only decisions that cannot be safely inferred}}

## 2. Level coverage

| Level | Evidence timestamps | Image/clue | Board or layout | Goal/answer | Difficulty/UI | Confidence |
|---|---|---|---|---|---|---|
| {{id}} | {{mm:ss–mm:ss}} | {{clue}} | {{dimensions and relevant layout}} | {{answer or visible goal}} | {{label/counter}} | {{observed/inferred}} |
| {{id}} | {{mm:ss–mm:ss}} | {{clue}} | {{dimensions and relevant layout}} | {{answer or visible goal}} | {{label/counter}} | {{observed/inferred}} |
| {{id}} | {{mm:ss–mm:ss}} | {{clue}} | {{dimensions and relevant layout}} | {{answer or visible goal}} | {{label/counter}} | {{observed/inferred}} |

The table must contain at least three distinct playable levels before implementation begins.

## 3. Gameplay loop and state machine

1. Launch → {{home/start state}}
2. Start/select level → {{initial puzzle state}}
3. Player input → {{legal gesture/tap and exact state change}}
4. Progress → {{what updates and when}}
5. Success → {{feedback sequence and next action}}
6. Failure/pause/reset → {{observed behavior, or “not observed”}}

## 4. Interaction contract

- Board geometry: {{rows/columns or other playable regions}}
- Movable elements: {{what moves}}
- Input mapping: {{gesture/tap}} → {{rule}}
- Move granularity: {{one slot/free distance/etc.}}
- Bounds/wrap/undo: {{behavior}}
- Hint/assist: {{count, effect, and cost}}
- Completion predicate: {{deterministic rule}}

## 5. Visual and audio contract

- Safe areas/orientation: {{value}}
- Layout hierarchy: {{header, clue, board, controls}}
- Colors/type: {{tokens}}
- Motion/feedback: {{markers, transitions, confetti, sound}}
- Accessibility: {{labels, dynamic type, contrast, reduce motion}}

## 6. Content and asset contract

- Config source: `{{repository-relative path}}`
- Schema: `{{schema name/version}}`
- Image/audio references: {{asset names}}
- Provenance/rights: {{licensed, user-owned, placeholder, or unresolved}}
- New level rule: {{what a contributor changes and what must remain unchanged}}

## 7. Acceptance criteria

- [ ] At least three observed levels are playable.
- [ ] A legal input produces the state change described above.
- [ ] Success and reset behavior match the recording.
- [ ] Adding a level requires config plus approved assets, not rule/view edits.
- [ ] Level/config validation fails clearly for malformed data.
- [ ] Build and focused tests pass.
- [ ] At least one UI smoke test launches and enters a level.
- [ ] Open-source and media-rights checks are complete.

## 8. Confirmation record

- Confirmed by user: {{not yet / yes}}
- Confirmation date: {{YYYY-MM-DD or blank}}
- Confirmation note: {{short note}}
```

Do not turn this document into a transcript. Its job is to make the mechanic, level data, and acceptance criteria unambiguous enough that implementation can proceed without another product review.
