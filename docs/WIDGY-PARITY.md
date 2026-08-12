# Capability audit vs Widgy

Written 2026-08-12, after Widgy's withdrawal (unmaintained, repo gone, the
developer citing a cease and desist without elaborating).

This is a *gap list*, not a plan. `ROADMAP_V2.md` holds the plan, and it
deliberately rejects parity-as-strategy: Scenes are the thesis, and matching
Widgy feature-for-feature is not the goal. What this file is good for is
answering two narrower questions — **what could someone do in Widgy that they
cannot do in Facet today**, and **which Widgy controls do we not yet
understand well enough to judge**.

**Legend:** ✅ implemented · 🟡 partial · ⬜ absent · ❓ unidentified Widgy
control · 🚫 blocked by the platform, documented in ROADMAP_V2

Scope: capabilities only. We match what an app can *do*; we never copy code,
assets, or trade dress. The importer reads their export format so users can
migrate their own work — that is its entire purpose.

---

## 1. Layer types

| | Capability | Note |
|---|---|---|
| ✅ | Text, symbol, shape, image, gauge, line, chart, container | |
| ✅ | Arbitrary path shapes | SVG-subset parser, node editing, shape studio |
| ✅ | Procedural blob generator | Seeded, slider-driven |
| ✅ | Launcher tiles | Composed from ordinary layers, so they theme for free |
| ✅ | User photos | Content-addressed, downsampled, travels with the document |
| ⬜ | **Repeater / list layer** | The missing primitive behind agenda lists and forecast rows — the one structural layer gap |
| ⬜ | Web / HTML layer | Widgy had one; hard to justify inside a 30 MB extension |

## 2. Geometry

| | Capability | Note |
|---|---|---|
| ✅ | Normalized position/size, rotation, z-order, padding, stack spacing + alignment | |
| ✅ | Numeric entry for position and size | |
| ✅ | Align / distribute actions | |
| ✅ | Per-rendition overrides | Edit once, tune per surface |
| ⬜ | Multi-select | Blocks bulk transforms and group/ungroup |
| ⬜ | Anchor-point selection | Everything anchors center |
| ⬜ | Aspect-ratio lock | |
| ⬜ | Clip-subviews toggle on containers | Masks cover most of this today |

## 3. Appearance & effects

| | Capability | Note |
|---|---|---|
| ✅ | Corner profiles | Per-corner radii + corner styles (squircle etc.) |
| ✅ | Shadow list, each with inset | Neumorphism/emboss presets ride on this |
| ✅ | Glow, blur, border, color adjust | Fixed effect order, matched across both backends |
| ✅ | Blend modes | |
| ✅ | Masks | Shape + alpha ramp + invert |
| ✅ | Linear / radial gradients, incl. on text and symbols | |
| ⬜ | Angular / conic gradient | |
| ⬜ | Image / pattern fill | Image layers exist; images-as-fill do not |
| ⬜ | Mask by another layer's alpha | Text knockouts; needs cycle detection + a second pass |
| 🚫 | Backdrop blur of the wallpaper | Widget backgrounds can't be transparent on iOS 27; wallpaper-crop illusion is the only route |

## 4. Text

| | Capability | Note |
|---|---|---|
| ✅ | Size, weight, design, custom font import + picker | |
| ✅ | Solid or gradient fill, alignment, line limit, tracking, case | |
| 🟡 | Auto-shrink | Fixed 0.5 minimum scale, not user-configurable |
| ⬜ | Line spacing | |
| ⬜ | Vertical alignment within the frame | |
| ⬜ | Text stroke / outline | |
| ⬜ | Truncation mode (head/middle/tail) | |
| ⬜ | Number-format and date-format builders | Expressions do it; no inspector affordance |

## 5. Data

| | Capability | Note |
|---|---|---|
| ✅ | Time, battery, weather (unit-aware), health, calendar, reminders, focus, astronomy | Real providers with permission flows |
| ✅ | Arbitrary user JSON APIs, with an editor UI | The thing Widgy never had |
| ⬜ | Now Playing (title, artist, artwork) | Common in Widgy designs |
| ⬜ | Storage / memory | |
| ⬜ | Network (SSID, IP, connection type) | |
| ⬜ | Location (city, coordinates, altitude) | |
| ⬜ | Connected device batteries (Watch, AirPods) | API access is limited — research before promising |
| ⬜ | World clocks | |
| ⬜ | Device info (name, model, OS, uptime) | |
| ⬜ | First-class countdown/timer source | Expressible today, but deserves a source |
| 🚫 | Focus *name* | `INFocusStatus` exposes only `isFocused: Bool?` |

## 6. Logic

| | Capability | Note |
|---|---|---|
| ✅ | Expression language with ~35 builtins, inline validation | |
| ✅ | Conditional visibility (`visibleWhen`) | Fails open, so a broken condition never blanks a widget |
| ✅ | Tap actions with expression-templated URLs | Deep links carrying live data |
| ✅ | Simplified-detail hiding | Mirrors WidgetKit `LevelOfDetail` |
| ⬜ | Conditional *styling* by expression | Colour that changes below 20% battery, without duplicate layers |
| ⬜ | Named user variables per document | Compute once, reuse |
| ⬜ | Expression autocomplete from discovered paths | Discovery exists in the data layer, unused by the UI |

## 7. Surfaces

| | Capability | Note |
|---|---|---|
| ✅ | Home Screen small / medium / large / XL / XL-portrait | |
| ✅ | Lock Screen circular / rectangular / inline | |
| ✅ | Multiple widget instances, each bound to its own design | `AppIntentConfiguration` |
| ⬜ | **Interactive buttons** (in-widget toggles/actions) | Tap actions are deep links today; App Intents in-widget is the real prize and structurally impossible for a raster-based competitor |
| ⬜ | StandBy | |
| ⬜ | Apple Watch complications | Widgy shipped these |
| ⬜ | Live Activities / Dynamic Island | Nothing in this category does it |
| ⬜ | Control Center widgets | Widgy 26.1.1 has them |

## 8. Editor

| | Capability | Note |
|---|---|---|
| ✅ | Live canvas, resident inspector, wallpaper backdrop, home-screen preview | |
| ✅ | Drag with snapping, 8-point resize, layer list, shape studio, asset/app pickers | |
| ✅ | Theme tokens + scene palettes, AI generation from a prompt | |
| ✅ | Undo | |
| ⬜ | Redo | |
| ⬜ | Copy / paste layers between documents | |
| ⬜ | Group / ungroup a selection | |
| ⬜ | Lock layer | |
| ⬜ | Canvas zoom / pan | Fixed zoom |
| ⬜ | Grid overlay / rulers | |
| ⬜ | Reusable components / presets | |
| ⬜ | Eyedropper from the wallpaper | Palette sampling exists; a live picker doesn't |

## 9. Documents & distribution

| | Capability | Note |
|---|---|---|
| ✅ | Create / rename / duplicate / delete | |
| ✅ | `.facet` export + import | |
| ✅ | Scene bundles — a whole home screen as one shareable file | Beyond anything Widgy could express |
| ⬜ | **Widgy importer** | Their users are stranded *right now*; this is the migration wedge and it just became time-critical |
| ⬜ | iCloud sync | Also the gating feature for any Mac editor |
| ⬜ | Community gallery | Widgy 26.1.1 had one with search — their moat, now unmaintained |
| ⬜ | Folders / organization | |
| ⬜ | Backup & restore | Scene bundles partially cover this |
| ⬜ | Per-document refresh settings UI | |

---

## Unidentified Widgy controls

The point of this section is the thing that can't be answered from memory:
Widgy exposed dozens of toggles with no documentation, no tooltips, and often
no visible effect on the layer you were editing. Before deciding whether to
implement any of them, they have to be *identified*.

Candidate names to look for while testing (grouped by where they appear in
Widgy's inspector) — confirm, screenshot, and annotate:

- **Sizing/layout:** "Fit", "Fill", "Aspect", "Anchor", "Relative", "Offset
  unit" (points vs percent), constraint-style options
- **Rendering:** "Render mode", "Tint mode", "Composite", "Cache", "Quality",
  "Retina/scale", "Corner smoothing"
- **Progress/gauge:** "Progress type", "Direction", "Cap", "Segments" (we
  have equivalents — confirm the mapping is complete)
- **Info fields:** the variant dropdowns on each data field, which appear to
  change formatting rather than the value
- **Global/document:** refresh options, "safe area", grid/snap values,
  anything under advanced or debug sections

### Resolving them cheaply

The app bundle's localization table lists every control label, which
enumerates the surface without touching code:

```sh
ls /Applications/Widgy.app/Contents/Resources/*.lproj
plutil -p /Applications/Widgy.app/Contents/Resources/en.lproj/Localizable.strings > ~/Desktop/widgy-strings.txt
```

Drop that in `docs/reference/` and this section can be reconciled against it —
neighbouring labels usually reveal what a mystery toggle governs. If the
strings are compiled into a `.strings` binary, `plutil -p` still prints them.

## What actually matters next

Ranked for the current moment, not for completeness:

1. **Widgy importer** — a user base with libraries they can't take anywhere,
   and no competing destination. Cheapest goodwill available.
2. **Interactive buttons** — where Facet passes Widgy rather than catches it.
3. **iCloud sync** — table stakes, and the prerequisite for a Mac editor.
4. **Conditional styling + repeater layer** — the two remaining expressive
   gaps that make some Widgy designs literally unbuildable here.
5. **Now Playing / storage / network** — the data breadth people notice.
6. **Community gallery** — their moat is unmaintained and inheritable.
