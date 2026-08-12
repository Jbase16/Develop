# Manual test pass

383 package tests cover the model, resolver, and both render backends, and
`FacetAppTests` covers `App/Shared`. What no test touches is the SwiftUI
editor under a finger, the widget extension on a real device, and the App
Group handoff between them. Those get tested by hand.

Run on device where marked 📱 — the simulator lies about battery, permission
prompts, and widget refresh. Record failures as `FAIL:` lines with the test
number and expected-vs-actual, then commit; that is how they reach a session
that can fix them.

Headless shortcuts for setup: `xcrun simctl launch booted com.JasonPhillips.app
-facet-show-sources` (also `-facet-open-editor`, `-facet-open-scene`).

---

## A. Gallery & documents

| # | Step | Expected |
|---|---|---|
| A1 | Fresh install (delete first), launch | Seeds the starter templates; no empty state |
| A2 | Inspect every preview tile | All render; no magenta (missing token), no ⚠︎ (broken binding) |
| A3 | New document | Opens in the editor, renders something sane |
| A4 | Rename / duplicate / delete | Each persists across a relaunch |
| A5 | Export `.facet`, then import it back | Imports as a separate document; original untouched |
| A6 | Import a corrupted `.facet` (flip a byte) | Clear error alert, no crash |
| A7 | Assign a document to a widget slot | Selection sticks |

## B. Canvas manipulation

| # | Step | Expected |
|---|---|---|
| B1 | Tap a layer | Selection border + handles |
| B2 | Drag | Tracks the finger 1:1 |
| B3 | Drag toward centre / quarters | Snaps with a visible guide |
| B4 | Corner handle / edge handle | Both axes / one axis |
| B5 | Drag a handle far past the canvas | Clamps; never inverts |
| B6 | Numeric position/size entry | Matches what dragging produces |
| B7 | Alignment actions | Layers align as labelled |
| B8 | Undo repeatedly | Steps back sanely; grouped, not per-pixel |
| B9 | Rapid undo during a drag | No corrupted layout, no crash |

## C. Inspector — per layer type

| # | Step | Expected |
|---|---|---|
| C1 | Add each layer type from the palette | Each appears and renders |
| C2 | Text: template `{percent(battery.level)}` | Updates on submit |
| C3 | Text: deliberately broken template | Warning shown; canvas unchanged |
| C4 | Text: size, weight, design, tracking, case | Each visibly changes |
| C5 | Text: custom font from the picker | Applies, and survives reopening |
| C6 | Text/symbol: gradient fill | Ramp resolves across the glyphs, not the layer box |
| C7 | Shape: kind, fill, stroke | |
| C8 | Shape studio: blob sliders, then a path shape | Silhouette matches the preview |
| C9 | Gauge: partial arc, start angle, direction, caps, segments | Each parameter behaves as named |
| C10 | Chart: `weather.hourly`, each style | Line/area/bars all render |
| C11 | Chart: nonsense data path | Empties gracefully; no crash |
| C12 | Corners: per-corner radii, unlink, corner style | Squircle differs visibly from a plain radius |
| C13 | Shadows: add several; flip one to inset | Neumorphism preset reads correctly on a same-colour background |
| C14 | Glow, blur, border, colour adjust, blend mode | Each applies; order matches the SVG preview |
| C15 | Mask: shape, alpha ramp, invert | Cut edge blurs with the layer, not before it |
| C16 | Image layer: pick a photo | Imports, downsamples, renders |
| C17 | Tap action with an expression URL | Saves; validated |
| C18 | `visibleWhen` condition true/false | Layer appears/disappears |
| C19 | `visibleWhen` with a broken expression | Layer stays visible (fails open) |

## D. Theme & palette

| # | Step | Expected |
|---|---|---|
| D1 | Change a colour token's light value | Every layer using it updates at once |
| D2 | Switch canvas to dark | Dark values take over |
| D3 | Delete a token still in use | Affected layers go magenta (intentional, no silent fallback) |
| D4 | Apply a scene palette to a widget | Token colours recolour; **literal** colours do not |

## E. Renditions

| # | Step | Expected |
|---|---|---|
| E1 | Cycle every rendition | Canvas resizes; layout adapts |
| E2 | Accessory sizes | Monochrome, legible |
| E3 | Move a layer in a non-base rendition | Override badge appears |
| E4 | Return to systemSmall | Base layout unchanged |
| E5 | Clear override | Reverts; badge clears |

## F. Scenes

| # | Step | Expected |
|---|---|---|
| F1 | Create a scene, import a wallpaper | Backdrop shows behind the canvas |
| F2 | Place widgets into real grid slots | Positions match the home-screen preview |
| F3 | Add launcher tiles from the app picker | Tiles render, theme with the palette |
| F4 | Edit a widget referenced by the scene | Scene reflects it — reference, not copy |
| F5 | Delete a widget the scene references | Scene degrades gracefully |
| F6 | Export a scene bundle, import on a clean install | Wallpaper, widgets, assets, palette all restored; ids remapped without collision |

## G. Data sources

| # | Step | Expected |
|---|---|---|
| G1 | 📱 Grant Health permission | Steps populate; denial leaves a sane fallback |
| G2 | 📱 Grant Calendar / Reminders | Next event and counts populate |
| G3 | 📱 Weather with units toggled | Values convert; symbols match conditions |
| G4 | 📱 Focus on/off | Boolean flips (name is unavailable by API — expected) |
| G5 | Custom source: add a public JSON API | Discovered paths listed; values bind in a text layer |
| G6 | Custom source: bad URL / 500 / oversized body | Clear error; last good snapshot retained |
| G7 | Astronomy for your location | Sunrise/sunset within a few minutes of reality |

## H. AI generation

| # | Step | Expected |
|---|---|---|
| H1 | Generate from a plain prompt | Produces an **editable** layer tree, not an image |
| H2 | Inspect the result's layers | Sensible names, tokens used, no orphan literals everywhere |
| H3 | Generate something deliberately absurd | Fails gracefully, no crash or empty document |

## I. 📱 Widget extension (device only)

| # | Step | Expected |
|---|---|---|
| I1 | Add small / medium / large widgets | Each renders its rendition's layout |
| I2 | Two widgets bound to different documents | Each shows its own |
| I3 | Edit and save in-app | Widget reflects the change within seconds |
| I4 | Lock Screen circular + rectangular | Monochrome, legible |
| I5 | Watch a clock template 2–3 minutes | Ticks without opening the app |
| I6 | Tap a layer with a tap action | Deep link fires (medium/large per-layer; small uses the first action) |
| I7 | Plug/unplug | Battery designs update within ~15 min |
| I8 | Airplane mode, then add a widget | Renders from cache |
| I9 | Leave overnight | Still populated in the morning — no blank or stale widgets |
| I10 | Photo-heavy design in the extension | No memory-limit crash (budget ~30 MB) |

---

## Results

### Pass 1 — (date · device · iOS)

- [ ] A · [ ] B · [ ] C · [ ] D · [ ] E · [ ] F · [ ] G · [ ] H · [ ] I

`FAIL:` notes here.
