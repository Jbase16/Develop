# Widgy parity checklist

A working inventory of the capability surface Widgy exposed, mapped against
what Facet has today. Widgy shipped no documentation, so this list is
reconstructed from its editor UI; treat unverified rows as claims to confirm
against the app bundle (see "Verifying this list" at the bottom).

**Legend**

| Mark | Meaning |
|---|---|
| ✅ | Implemented in Facet |
| 🟡 | Partially implemented — see note |
| ⬜ | Not implemented |
| ❓ | Exists in Widgy, purpose unclear — research before deciding |

Scope note: this is a *capability* checklist. We match features, never
Widgy's code, assets, or file format. The importer (below) reads their
export format for user migration only.

---

## 1. Layer types

| Status | Feature | Note |
|---|---|---|
| ✅ | Text | Template strings with embedded expressions |
| ✅ | Shape — rectangle / circle / capsule | |
| ✅ | SF Symbol | |
| ✅ | Line / divider | Dash patterns supported |
| ✅ | Progress gauge — ring | |
| ✅ | Progress gauge — bar | |
| ✅ | Chart — line / area / bars | Over any list-valued data path |
| ✅ | Group / container | absolute, row, column, overlay |
| 🟡 | Image | Model + renderer exist; no photo picker or asset bundle yet |
| ⬜ | Blur / backdrop layer | Frosted panels — very common in Widgy designs |
| ⬜ | Mask / clipping layer | Shape-masked images, text knockouts |
| ⬜ | App launcher grid | Tappable icon rows linking to apps |
| ⬜ | Calendar agenda list | Repeating rows bound to a list of events |
| ⬜ | Weather forecast row | Repeating rows (needs a repeater primitive) |
| ⬜ | **Repeater / list layer** | The general primitive the two rows above need |
| ⬜ | Web / HTML layer | Widgy had one; likely not worth the memory in an extension |

## 2. Geometry & transform

| Status | Feature | Note |
|---|---|---|
| ✅ | Position (normalized X/Y, center anchor) | |
| ✅ | Size (normalized W/H) | |
| ✅ | Rotation | |
| ✅ | Z-order | Reorder in layer list |
| ✅ | Padding (containers) | |
| ✅ | Stack spacing + cross-axis alignment | |
| ⬜ | Numeric entry for position/size | Inspector is sliders/gestures only — precision editing missing |
| ⬜ | Anchor point selection | Everything anchors center; Widgy allows corner/edge anchors |
| ⬜ | Aspect-ratio lock | |
| ⬜ | Align / distribute tools | Align selected layers left/center/right, even spacing |
| ⬜ | Multi-select | Blocks align/distribute and group operations |
| ⬜ | Clip subviews to bounds | Per-container toggle |
| ❓ | "Fit" / auto-size modes | Widgy has several sizing modes whose behavior is undocumented |

## 3. Appearance & effects

| Status | Feature | Note |
|---|---|---|
| ✅ | Opacity | |
| ✅ | Corner radius | |
| ✅ | Drop shadow (color, radius, offset) | |
| ✅ | Stroke / border (shapes) | |
| ✅ | Solid fill | |
| ✅ | Linear gradient | Editor UI edits 2 stops + angle; model supports N stops |
| ✅ | Radial gradient | |
| ⬜ | Angular / conic gradient | |
| ⬜ | Image fill (pattern/photo as fill) | |
| ⬜ | Blend modes | multiply, screen, overlay, etc. — big visual-style unlock |
| ⬜ | Gaussian blur on a layer | |
| ⬜ | Backdrop blur (blur what's behind) | Requires renderer support, not just a modifier |
| ⬜ | Inner shadow | |
| ⬜ | Per-corner radius | Different radius per corner |
| ⬜ | Border dash / inset control | |
| ❓ | Vibrancy / material styles | Which iOS material maps to Widgy's options is unclear |

## 4. Text

| Status | Feature | Note |
|---|---|---|
| ✅ | Font size / weight / design (system fonts) | |
| ✅ | Color (token or literal) | |
| ✅ | Horizontal alignment | |
| ✅ | Line limit | |
| ✅ | Letter spacing (tracking) | |
| ✅ | Text case transform | upper / lower |
| ✅ | Auto-shrink to fit | Fixed 0.5 minimum scale, not user-configurable |
| 🟡 | Custom font family | Model accepts a family name; no font import or picker UI |
| ⬜ | Vertical alignment within the frame | |
| ⬜ | Line spacing | |
| ⬜ | Gradient-filled text | |
| ⬜ | Text stroke / outline | |
| ⬜ | Per-text shadow controls | Inherits generic layer shadow only |
| ⬜ | Truncation mode selection | head/middle/tail |
| ⬜ | Number formatting UI | Expressions can do it; no inspector affordance |
| ⬜ | Date format builder | `dateFormat()` exists; needs a token picker UI |

## 5. Data sources

| Status | Feature | Note |
|---|---|---|
| ✅ | Date & time (components, 12/24h, names) | Recomputed per timeline entry — never stale |
| ✅ | Battery (level, state, low-power) | Real device data |
| ✅ | Astronomy (sunrise, sunset, day length, moon phase) | Computed, no network or permissions |
| ✅ | Custom URL → JSON source | Engine complete (auth headers, size cap, path discovery) |
| 🟡 | Weather | Sample payload; WeatherKit provider not wired |
| 🟡 | Health (steps, energy, stand) | Sample payload; HealthKit provider not wired |
| 🟡 | Calendar (next event, count) | Sample payload; EventKit provider not wired |
| ⬜ | **Editor UI for custom URL sources** | Engine has no front door — highest-leverage gap |
| ⬜ | Reminders | |
| ⬜ | Now Playing (title, artist, artwork) | |
| ⬜ | Storage (used / free / total) | |
| ⬜ | Memory / RAM | |
| ⬜ | Network (SSID, IP, connection type) | |
| ⬜ | Location (city, coordinates, altitude) | |
| ⬜ | Connected device batteries (Watch, AirPods) | Widgy favorite; API access is limited — research needed |
| ⬜ | Countdown / countup to a date | Expressible today, but deserves a first-class source |
| ⬜ | World clocks (multiple timezones) | |
| ⬜ | Device info (name, model, OS version, uptime) | |
| ⬜ | Stocks / RSS | Custom URL source may cover these |

## 6. Logic & expressions

| Status | Feature | Note |
|---|---|---|
| ✅ | Arithmetic, comparison, boolean, ternary | |
| ✅ | String concatenation and functions | |
| ✅ | Math / format / unit-conversion builtins | ~35 functions |
| ✅ | `has()` for missing-data fallbacks | |
| ✅ | Environment variables in expressions | `env.dark`, `env.rendition` |
| ✅ | Inline validation on entry | Editor rejects unparseable input |
| ⬜ | **Conditional layer visibility** | Show/hide by expression — Widgy leans on this heavily |
| ⬜ | Conditional color / style by expression | e.g. red below 20% battery |
| ⬜ | Expression autocomplete for available paths | Discovery exists in the data layer, unused by the UI |
| ⬜ | Named user variables per document | Compute once, reuse across layers |

## 7. Surfaces & renditions

| Status | Feature | Note |
|---|---|---|
| ✅ | Home Screen small / medium / large | |
| ✅ | Lock Screen circular / rectangular / inline | Monochrome resolution handled |
| ✅ | Per-rendition overrides (sparse patches) | Edit once, tune per size |
| ⬜ | iPad extra-large | |
| ⬜ | StandBy | |
| ⬜ | Apple Watch complications | Widgy shipped these; real differentiator |
| ⬜ | **Interactive buttons (App Intents)** | iOS 17+; Widgy's static-image model can't follow us here |
| ⬜ | Per-layer tap targets / deep links | Turns one widget into a launcher |
| ⬜ | Live Activities / Dynamic Island | Nothing comparable exists in this category |

## 8. Editor experience

| Status | Feature | Note |
|---|---|---|
| ✅ | Live canvas at true widget size | Same resolver as the shipping widget |
| ✅ | Tap to select | |
| ✅ | Drag to move, with snap guides | Snaps to 0/¼/½/¾/1 |
| ✅ | 8-point resize handles | |
| ✅ | Layer list (select, hide, reorder, duplicate, delete) | |
| ✅ | Add-layer palette | All eight layer types |
| ✅ | Per-type inspector | |
| ✅ | Theme/token editor (light + dark) | |
| ✅ | Undo | Coalesced; no redo yet |
| ✅ | Light/dark and rendition preview switching | |
| ⬜ | Redo | |
| ⬜ | Canvas zoom / pan | Fixed 2× zoom today |
| ⬜ | Copy / paste layers between documents | |
| ⬜ | Group / ungroup selection | |
| ⬜ | Lock layer | |
| ⬜ | Grid overlay / rulers | |
| ⬜ | Wallpaper preview behind the canvas | Essential for transparency-style designs |
| ⬜ | Reusable layer components / presets | |
| ⬜ | Color palettes / eyedropper | |

## 9. Documents, sharing, system

| Status | Feature | Note |
|---|---|---|
| ✅ | Create / rename / duplicate / delete | |
| ✅ | Export & import `.facet` files | |
| ✅ | Choose which document the widget shows | |
| ✅ | Snapshot cache + refresh planner (cadence classes) | Honest about iOS reload budgets |
| ⬜ | iCloud sync | Spec'd, not built |
| ⬜ | **Widgy JSON importer** | Migration path for a stranded user base — now urgent |
| ⬜ | Community gallery (browse / install / remix) | |
| ⬜ | Multiple widget instances with per-instance selection | Needs an App Intent configuration |
| ⬜ | Folders / organization | |
| ⬜ | Backup & restore | |
| ⬜ | Per-document refresh settings UI | |

---

## Priority read

Given Widgy's exit and the iOS 27 window, the ordering that matters:

1. **Widgy JSON importer** — a stranded user base with libraries of designs
   they can't take anywhere. Nothing else buys goodwill this cheaply.
2. **Conditional visibility + conditional styling** — the single most-used
   Widgy mechanic still missing; many designs are unbuildable without it.
3. **Custom URL source editor UI** — the engine is done; this is front-door
   work that unlocks the "any API" story Widgy never had.
4. **Interactive buttons + tap targets** — where we pass Widgy rather than
   catch it, and structurally out of reach for a raster-based competitor.
5. **Blur / blend modes / masking** — the visual vocabulary gap; these three
   account for most "why doesn't mine look like theirs" complaints.
6. **Real providers** (WeatherKit, HealthKit, EventKit) — sample data can't
   ship.
7. **Wallpaper preview + image picker** — the transparency aesthetic.

## Verifying this list

Reconstructing from memory has limits. The cheapest high-fidelity source is
the app bundle's own localization table: every editor control's label lives
there, so it enumerates the feature surface without touching code.

On a Mac with the Widgy app installed:

```sh
ls /Applications/Widgy.app/Contents/Resources/*.lproj
plutil -p /Applications/Widgy.app/Contents/Resources/en.lproj/Localizable.strings > ~/Desktop/widgy-strings.txt
```

Drop that file into `docs/reference/` and this checklist can be reconciled
against it — including the ❓ rows, where a label's neighbours usually reveal
what a mystery toggle actually governs.
