# Manual test pass

The packages are covered by 96 automated tests; everything below the app
layer is verified on every commit. What no test touches is the SwiftUI
editor, the widget extension on a real device, and the App Group handoff
between them — so those get tested by hand.

Run on device where noted (the simulator lies about battery and widget
refresh behavior). Log failures inline as `FAIL:` notes and commit them —
that's how they reach me.

---

## A. Gallery

| # | Step | Expected |
|---|---|---|
| A1 | Launch a fresh install (delete the app first) | Gallery seeds with 12 starter templates |
| A2 | Inspect each preview tile | Every one renders; no magenta fills (missing token), no ⚠︎ (broken binding) |
| A3 | Pull to refresh | No hang; battery-driven templates show current level |
| A4 | Tap **+** | New "Untitled" document appears and opens |
| A5 | Long-press a tile → Rename | Alert accepts text; name updates in the grid |
| A6 | Long-press → Duplicate | Copy appears named "… Copy", independent of the original |
| A7 | Long-press → Share .facet | Share sheet offers a `.facet` file; save it to Files |
| A8 | Toolbar import → pick that file | Imports as a separate document, does not overwrite the original |
| A9 | Long-press → Show in Widget | Checkmark moves to that tile |
| A10 | Long-press → Delete | Tile disappears; survives an app relaunch |

## B. Editor — selection & geometry

| # | Step | Expected |
|---|---|---|
| B1 | Open "Battery Ring", tap the percentage text | Selection border + 8 handles appear around it |
| B2 | Drag it around | Moves under the finger, 1:1 with the gesture |
| B3 | Drag toward the horizontal centre | Snaps at centre; accent guide line appears |
| B4 | Drag a corner handle outward | Grows on both axes |
| B5 | Drag an edge handle | Grows on one axis only |
| B6 | Drag any handle far past the canvas | Clamps, never inverts or disappears |
| B7 | Tap empty canvas | Deselects |
| B8 | Undo repeatedly | Steps back through edits; grouped by ~1s, not one step per pixel |

## C. Editor — layers & inspector

| # | Step | Expected |
|---|---|---|
| C1 | Open the layer panel | Tree matches the canvas, front-most listed first |
| C2 | Tap a row | Selects that layer on the canvas |
| C3 | Swipe a row → Hide | Layer disappears from canvas; row shows the eye-slash |
| C4 | Context menu → Bring Forward / Send Backward | Draw order visibly changes |
| C5 | Context menu → Duplicate | Copy offset slightly, independently selectable |
| C6 | Swipe → Delete | Removed; undo restores it |
| C7 | Add each of the 8 layer types | Each appears centred and renders correctly |
| C8 | Inspector → edit a text template to `{percent(battery.level)}` | Canvas updates on submit |
| C9 | Enter a deliberately broken template, e.g. `{foo(` | Orange warning; canvas does **not** change |
| C10 | Inspector → font size / weight / design | Each visibly changes the text |
| C11 | Inspector → tracking, case | Letter spacing widens; case transforms |
| C12 | Shape layer → Fill → Linear, set two colors + angle | Gradient renders, angle slider rotates it |
| C13 | Gauge → change expression to `0.25` | Ring/bar jumps to a quarter |
| C14 | Chart → data path `weather.hourly`, style Area | Area chart renders |
| C15 | Chart → nonsense path | Chart empties; app does not crash |
| C16 | Toggle shadow, adjust radius | Shadow appears and softens |
| C17 | Rotation slider | Rotates about the layer centre |

## D. Theme tokens

| # | Step | Expected |
|---|---|---|
| D1 | Theme editor → change a color token's light value | Every layer using it updates at once |
| D2 | Switch canvas to Dark | Dark values take over |
| D3 | Change a font token's size | All text bound to that token resizes |
| D4 | Delete a token still in use | Affected layers render magenta (intentional — no silent fallback) |
| D5 | Add a token | Appears in the swatch row of every color picker |

## E. Renditions & overrides

| # | Step | Expected |
|---|---|---|
| E1 | Switch to Medium | Canvas resizes; layout adapts by proportion |
| E2 | Switch to Lock ◯ | Renders monochrome/vibrant |
| E3 | In Medium, drag a layer | "override" badge appears on the selection |
| E4 | Return to Small | Base layout unchanged by the Medium edit |
| E5 | Back to Medium, inspector → Clear override | Reverts to the base position; badge clears |

## F. Persistence & widget (device only)

| # | Step | Expected |
|---|---|---|
| F1 | Edit, Save, reopen the document | Changes persisted |
| F2 | Force-quit and relaunch | Still persisted |
| F3 | Home screen → add a Facet widget (small) | Renders the selected document, not a placeholder |
| F4 | Change the selected document in-app | Widget updates within a few seconds |
| F5 | Edit a layer, Save | Widget reflects the edit |
| F6 | Add medium + large widgets | Each uses its rendition's layout |
| F7 | Lock Screen → add circular + rectangular | Render monochrome, legible |
| F8 | Watch a clock template for 2–3 minutes | Minute ticks over without opening the app |
| F9 | Plug in / unplug the phone | Battery templates reflect it within ~15 min |
| F10 | Leave widgets overnight | Still populated in the morning — no blank/stale widgets |

## G. Robustness

| # | Step | Expected |
|---|---|---|
| G1 | Import a malformed `.facet` (edit a byte in a text editor) | Clear "not a valid .facet" alert; no crash |
| G2 | Delete the document the widget is showing | Widget falls back gracefully |
| G3 | Airplane mode → add a widget | Renders from cache |
| G4 | Rotate device, use Dynamic Type at a large setting | Editor chrome stays usable |
| G5 | Rapid undo spam mid-drag | No corrupted layout, no crash |

---

## Results

Record per pass: date, iOS version, device, and any `FAIL:` lines with the
test number. Failures with a test number and expected-vs-actual are directly
actionable; "it looked weird" is not.

### Pass 1 — (date, device, iOS)

- [ ] A · [ ] B · [ ] C · [ ] D · [ ] E · [ ] F · [ ] G
