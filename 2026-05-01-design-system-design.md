# Design System Specification

**Date:** 2026-05-01
**Status:** Draft, awaiting review
**Authors:** Arijit Saha
**Scope:** Foundational design system for cross-platform apps (Web via Nuxt, Apple iOS/macOS, Android)
**Inspiration:** Apple Liquid Glass (macOS Tahoe 26 / iOS 26), Apple Reminders, Linear, Apple Music, Anthropic editorial typography, Nuxt UI v4 token taxonomy.

---

## 1. Purpose and Constraints

This is a generic, app-agnostic design system. It provides foundations (color, typography, spacing, motion, materials) and themes Nuxt UI on the web. On Apple and Android platforms it provides equivalent token outputs and rules of thumb. It does not own brand identity, copywriting, illustrations, or app-specific compositions.

Out of scope: app-specific components such as task rows, money inputs, transaction lists, chart styling, or any feature pattern. Those live in each consuming app's repository.

The output is a multi-doc reference (markdown) plus W3C DTCG token files. There is no bundled component library shipped from this repo (with the exception of a small set of Tier B additions described in section 4). Apps consume tokens through Style Dictionary outputs and theme Nuxt UI through `app.config.ts`. Docs target a Figma-reference reading workflow: dense tables of exact values, swatch SVGs, and anatomy diagrams. They are not tutorials.

---

## 2. Personality

The design language is called **Disciplined Warmth**. It synthesizes three references:

- **Bones from Linear:** monochrome neutrals, dense lists, keyboard-first flows, restrained motion, crisp 1 px dividers, command palette as a primary navigation tool.
- **Skin from Apple Reminders:** rounded corners (8 to 12 px), soft pastel category accents, friendly SF Symbols iconography, conversational microcopy.
- **Hero moments from Apple Music:** bold gradient summary tiles or balance cards, large hero imagery, expressive but always reserved for at most one or two surfaces per screen. Never a UI-wide treatment.

Editorial type comes from Anthropic's marketing aesthetic. The serif role is reserved for long-form content, settings descriptions, onboarding subheads, empty-state quotes, and marketing pages. UI controls always use sans.

---

## 3. Foundations

The token hierarchy is enforced as three tiers: **primitives** → **semantic** → **component**. App code reads only from the semantic tier (and component tier where it exists). Primitives are never referenced directly by apps.

### 3.1 Color

The neutral ramp lives in `tokens/primitives/color.json` with steps from 50 (lightest) to 950 (darkest). The 11-step ramp covers backgrounds, surfaces, borders, and text needs across both light and dark themes.

**Neutral ramp (light theme reference):**

| Step | Hex      | Use                                |
| ---- | -------- | ---------------------------------- |
| 50   | #FAFAFA  | page background                    |
| 100  | #F4F4F5  | muted background                   |
| 200  | #E4E4E7  | elevated surface, default border   |
| 300  | #D4D4D8  | strong border                      |
| 400  | #A1A1AA  | dimmed text                        |
| 500  | #71717A  | muted text                         |
| 600  | #52525A  | toned text                         |
| 700  | #3F3F46  | default body text                  |
| 800  | #27272A  | strong text                        |
| 900  | #18181B  | highlighted text (AAA candidate)   |
| 950  | #09090B  | maximum                            |

Dark theme ramp inverts roles: page background -> 950, default text -> 300, etc. The full mapping lives in `tokens/semantic/dark.json`.

**Accent palette.** Eight accents, each with a full 50 to 950 ramp:

`blue · purple · pink · red · orange · yellow · green · graphite`

These mirror the macOS System Settings accent palette. One accent at a time is mapped to the `primary` semantic role. Default is `blue`.

**Semantic color roles.** App code never references hex or numbered ramp steps. It reads role tokens, which are theme-aware:

- **Text roles:** `text.dimmed` · `text.muted` · `text.toned` · `text.default` · `text.highlighted` · `text.inverted` · `text.onAccent`
- **Surface roles:** `surface.default` · `surface.muted` · `surface.elevated` · `surface.accented` · `surface.inverted`
- **Border roles:** `border.default` · `border.muted` · `border.accented` · `border.inverted`
- **Ring roles** (focus/selection): `ring.default` · `ring.muted` · `ring.accented`
- **Status roles:** `primary` · `secondary` · `success` · `info` · `warning` · `error` · `neutral`

The role taxonomy is adopted from Nuxt UI v4 and proves itself across both web and native platforms.

**Gradients.** Four reserved hero gradient pairs. Used only on summary tiles, hero images, balance cards, or onboarding heroes. Never on buttons, list backgrounds, or chrome.

- `gradient.warm` — pink to orange, 135°
- `gradient.cool` — blue to purple, 135°
- `gradient.success` — green to teal, 135°
- `gradient.danger` — red to orange, 135°

All gradients are paired with a 5 percent black overlay layer for depth and to prevent oversaturation against bright backdrops.

### 3.2 Typography

**Font stack:**

- **Sans (UI default):** `SF Pro Text, SF Pro Display, "Public Sans Variable", system-ui, sans-serif`
- **Serif (editorial / long-form / hero):** `ui-serif, "Newsreader Variable", "Source Serif 4 Variable", Charter, Georgia, serif`
- **Mono (numbers, code, finance):** `SF Mono, "JetBrains Mono Variable", ui-monospace, monospace`

Variable fonts only. On Apple platforms, SF Pro and SF Mono are supplied by the system. On web and Android, Public Sans Variable, Newsreader Variable, and JetBrains Mono Variable are self-hosted (web via @nuxt/fonts; Android via bundled Resource fonts).

**When to use serif:**

| Surface                         | Family |
| ------------------------------- | ------ |
| Long-form note bodies           | serif  |
| Onboarding hero subtitles       | serif  |
| Settings page descriptions      | serif  |
| Empty-state quotes              | serif  |
| Marketing landing pages         | serif  |
| All UI controls and lists       | sans   |

**Type scale (sans and serif share the same scale):**

| Token              | Size / line height | Weight | Use                          |
| ------------------ | ------------------ | ------ | ---------------------------- |
| `text.caption2`    | 11 / 13            | 500    | tiny meta labels             |
| `text.caption1`    | 12 / 16            | 500    | timestamps, captions         |
| `text.footnote`    | 13 / 18            | 400    | helper text                  |
| `text.body.sm`     | 14 / 20            | 400    | dense list rows              |
| `text.body`        | 15 / 22            | 400    | default body                 |
| `text.body.lg`     | 17 / 24            | 400    | comfortable body             |
| `text.callout`     | 17 / 22            | 600    | inline emphasis              |
| `text.headline`    | 20 / 26            | 600    | section titles               |
| `text.title3`      | 22 / 28            | 700    | card titles                  |
| `text.title2`      | 28 / 34            | 700    | screen titles                |
| `text.title1`      | 34 / 41            | 700    | hero titles                  |
| `text.largeTitle`  | 44 / 52            | 700    | balance figures, hero stats  |

Two body weights (400 regular, 600 semibold). 700 is reserved for titles. 500 medium appears in caption sizes only. Tabular numerals are always enabled for finance, dates, and any numeric column.

### 3.3 Spacing — 4 pt grid

Token names: `none · 2xs · xs · sm · md · lg · xl · 2xl · 3xl · 4xl · 5xl · 6xl · 7xl · 8xl · 9xl`
Pixel values: `0 · 2 · 4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64 · 80 · 96 · 128`

Layout spacing always references the scale. Off-scale values fail review.

### 3.4 Shape — corner radius

| Token         | Value | Use                       |
| ------------- | ----- | ------------------------- |
| `radius.none` | 0     | flush surfaces            |
| `radius.xs`   | 4     | tight inputs, small tags  |
| `radius.sm`   | 6     | small buttons             |
| `radius.md`   | 8     | default buttons, inputs   |
| `radius.lg`   | 10    | cards                     |
| `radius.xl`   | 12    | modals, sheets            |
| `radius.2xl`  | 16    | hero cards                |
| `radius.3xl`  | 24    | onboarding surfaces       |
| `radius.full` | 999   | pills, avatars, chips     |

### 3.5 Elevation — shadow and glass layers

**Solid shadow scale.** All shadows use neutral black with low alpha. Y-offset only; no spread; X-offset zero.

| Token         | Approximate                                                                |
| ------------- | -------------------------------------------------------------------------- |
| `shadow.xs`   | `0 1px 2px rgba(0,0,0,0.04)`                                               |
| `shadow.sm`   | `0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)`                   |
| `shadow.md`   | `0 4px 8px rgba(0,0,0,0.06), 0 2px 4px rgba(0,0,0,0.04)`                   |
| `shadow.lg`   | `0 12px 24px rgba(0,0,0,0.08), 0 4px 8px rgba(0,0,0,0.04)`                 |
| `shadow.xl`   | `0 24px 48px rgba(0,0,0,0.12)`                                             |

**Glass material layers** (Liquid Glass approximation):

| Token            | Apple SwiftUI       | Web fallback                                                          | Use                       |
| ---------------- | ------------------- | --------------------------------------------------------------------- | ------------------------- |
| `glass.thin`     | `.regularMaterial`  | bg `rgba(255,255,255,.55)` + `backdrop-filter: blur(20px) saturate(180%)` | toolbars                  |
| `glass.regular`  | `.thickMaterial`    | bg `rgba(255,255,255,.75)` + `blur(40px) saturate(180%)`              | sidebars, popovers        |
| `glass.thick`    | `.ultraThickMaterial` | bg `rgba(255,255,255,.88)` + `blur(60px) saturate(180%)`            | sheets, command palette   |
| `glass.chrome`   | `.bar`              | bg `rgba(255,255,255,.92)`                                            | menu bar, tab bar         |

**Allowed glass surfaces:** sidebar / split-view pane, toolbar / tab bar / top app bar, popover (DropdownMenu, ContextMenu, large Tooltip), Sheet / Drawer / Slideover backdrop, Toast container, CommandPalette overlay.

**Forbidden glass surfaces (content always solid):** Card body (default and elevated), Input / Textarea, ListRow body, Modal / Dialog body, Buttons, Form surfaces.

When `prefers-reduced-transparency` is set, or when `backdrop-filter` is unsupported, glass automatically falls back to `surface.elevated` plus `shadow.md`.

### 3.6 Motion

| Token                     | Curve                              | Duration | Use                |
| ------------------------- | ---------------------------------- | -------- | ------------------ |
| `motion.micro`            | `cubic-bezier(0.2, 0, 0, 1)`       | 120 ms   | hover, focus       |
| `motion.short`            | same                               | 200 ms   | toggle, checkbox   |
| `motion.medium`           | same                               | 300 ms   | sheet, popover     |
| `motion.long`             | same                               | 500 ms   | page transition    |
| `motion.spring.standard`  | spring damping 0.5, stiffness 0.85 | -        | hero state         |
| `motion.spring.bouncy`    | spring damping 0.4, stiffness 0.7  | -        | success states     |

When `prefers-reduced-motion` is set, durations clamp to zero, replaced with 80 ms opacity-only fades. Spring motion falls back to short ease-out.

### 3.7 Density

Two density modes shipped. Switched at app- or user-level. Affects only row heights, button/input heights, list gaps, and section gaps. Type sizes and colors are unchanged across density modes.

| Token                          | Compact (Linear) | Comfortable (Reminders) |
| ------------------------------ | ---------------- | ----------------------- |
| `density.row.height`           | 28               | 36                      |
| `density.row.padX`             | 8                | 12                      |
| `density.button.height.md`     | 28               | 36                      |
| `density.input.height.md`      | 28               | 36                      |
| `density.list.gap`             | 0 (dividers)     | 4                       |
| `density.section.gap`          | 16               | 24                      |

**Default per platform:** desktop web and macOS use `compact`. iOS, Android, and mobile web use `comfortable`. Apps may override and may expose a user toggle.

### 3.8 Iconography

- **Apple platforms:** SF Symbols 6 (system, free). Default weight `Regular`. Active or selected state uses `Semibold`.
- **Web and Android:** Lucide. Default stroke `1.5`. Scales with text size.

Icon names are mapped between SF Symbols and Lucide in `docs/foundations/iconography.md`. Example: `chevron.right` corresponds to Lucide `chevron-right`. The canonical token name uses the SF Symbols form; the web layer translates at render time.

---

## 4. Component Scope

### 4.1 Doc template

Every component file in `docs/components/` follows a fixed structure: purpose, anatomy, variants, sizes, states, tokens consumed, behavior rules, accessibility, platform notes, do/don't, examples. Deviations from this structure fail review.

### 4.2 Tier A — themed via Nuxt UI v4

These components ship in Nuxt UI v4 directly. The DS does not provide implementations. Each gets a doc file specifying Nuxt UI prop-to-token mapping, variant matrix, deviations from defaults, and accessibility additions.

Accordion, Alert, Avatar, AvatarGroup, Badge, Breadcrumb, Button, ButtonGroup, Card, Checkbox, Chip, Collapsible, Container, ContextMenu, Drawer, DropdownMenu, Form, FormField, Input, InputMenu, InputNumber, InputTags, Kbd, Link, Modal, NavigationMenu, Pagination, PinInput, Popover, Progress, RadioGroup, Select, SelectMenu, Separator, Skeleton, Slideover, Slider, Stepper, Switch, Table, Tabs, Textarea, Toast, Tooltip, Tree.

### 4.3 Tier B — DS additions

Nuxt UI v4 lacks these or they need a DS-specific shape. The DS provides specs and small reference implementations.

- **CommandPalette** — Linear-style ⌘K palette. Action registration, scoping, recents, keyboard navigation.
- **ListRow** — Reminders-style row primitive: leading slot, title, subtitle, trailing slot, swipe actions on touch.
- **EmptyState** — icon + title + description + action; supports serif subtitle.
- **Stat** — KPI tile with label, value, and delta indicator.
- **Glass** — wrapper applying `glass.thin/regular/thick/chrome` with built-in `prefers-reduced-transparency` fallback.
- **DateTimePicker** — extends Nuxt UI's date input with natural-language input parsing and time portion.
- **SegmentedControl** — Apple-native pattern; Tabs covers most cases, but visually distinct contexts call for SegmentedControl.

### 4.4 Tier C — deferred

Advanced DataTable, Calendar grid, Carousel, FileUpload, Tour/Coachmark, Charts. These are not v1 scope. Charts are explicitly delegated to Recharts/Vue-ECharts/equivalent; the DS only provides color tokens to charting libraries.

### 4.5 Generic patterns

`docs/patterns/` holds composed, app-agnostic recipes:

- empty-state
- command-palette
- data-density-toggle
- swipe-actions
- form-layout
- confirmation-dialog
- settings-page
- loading-and-skeletons
- error-and-recovery

App-specific compositions (financial figures, transaction rows, task rows) do **not** live in this repo.

### 4.6 Nuxt UI v4 integration

The DS extends Nuxt UI v4 in two places, never by forking source.

1. **`app.config.ts`** of each consuming Nuxt 4 app: maps DS color roles to Nuxt UI's `ui.colors` keys (`primary`, `secondary`, `success`, `info`, `warning`, `error`, `neutral`) and supplies component-level `slots`, `variants`, and `defaultVariants` overrides per component.
2. **`assets/css/ds.css`** authored from token build output: a Tailwind v4 `@theme` block that aliases DS CSS variables to Tailwind utility names. After this drop-in, Tailwind classes such as `bg-primary-500`, `text-default`, `radius-md` resolve through DS tokens.

Build pipeline: when the first Nuxt app integrates, Style Dictionary emits both a `tokens.studio.json` (for the Tokens Studio Figma plugin) and a Tailwind v4 `@theme` snippet plus `app.config.ts` fragment for drop-in. Until that milestone, tokens stay authored as JSON and consumed manually.

---

## 5. Theming Model

### 5.1 Mode (light / dark)

OS-level preference is the source of truth. Apps may expose a setting with values `system | light | dark` to override per user.

Mechanism is platform-specific but conceptually uniform: a single attribute or environment value selects which semantic token bundle is active. Components never read raw primitives; they always read semantic tokens, so theme switching is a one-line concern at the app root.

### 5.2 Accent color

Default accent is `blue`. Eight options match macOS System Settings: blue, purple, pink, red, orange, yellow, green, graphite. Each accent has its full ramp pre-built; switching the accent re-aliases the `primary` semantic token to the chosen ramp.

Apps with a fixed brand accent set the accent at app boot and hide the user picker. The DS does not enforce showing a picker.

### 5.3 Density

Default per platform documented in section 3.7. Apps may override and may expose a user toggle.

### 5.4 User preference cascade

Order, top wins:

1. `prefers-reduced-motion: reduce` → all motion durations zero, opacity-only fades remain.
2. `prefers-reduced-transparency: reduce` (or macOS Reduce Transparency) → glass swaps to solid `surface.elevated` plus `shadow.md`; backdrop blur disabled.
3. `prefers-contrast: more` → swap to high-contrast token bundle (`light-high-contrast.json`/`dark-high-contrast.json`); borders thicken from 1 to 2 px; text uses `text.highlighted` everywhere.
4. App-level theme override (light / dark / system).
5. App-level accent override.
6. OS theme preference.

Each platform exposes a hook or environment value (`useDSPreferences`, `@DSPreferences`, `LocalDSPreferences`) that surfaces the merged state.

### 5.5 Liquid Glass material rules

Allowed and forbidden surfaces are listed in section 3.5. Material-to-context mapping:

| Material         | Where                                              |
| ---------------- | -------------------------------------------------- |
| `glass.chrome`   | menu bar, tab bar, status bar                      |
| `glass.thin`     | toolbars                                           |
| `glass.regular`  | sidebar, popover                                   |
| `glass.thick`    | sheet, drawer, command palette overlay             |

**Web fallback rules:**

- `@supports (backdrop-filter: blur(1px))` is required to enable glass; otherwise solid fallback.
- `prefers-reduced-transparency: reduce` always forces solid fallback.
- Optional refraction: web hero glass surfaces may opt in to an SVG `<feDisplacementMap>` filter on top of `backdrop-filter`. Costs roughly 5 ms on low-end mobile. Off by default; enabled per surface via `data-refract="true"`.

---

## 6. Accessibility

### 6.1 Contrast tiers

| Surface                                            | Minimum ratio | Token enforcement                        |
| -------------------------------------------------- | ------------- | ---------------------------------------- |
| Body text (`text.default`)                         | 4.5:1 (AA)    | enforced in semantic light/dark bundles  |
| Large text (≥ 18 pt or ≥ 14 pt bold)              | 3:1 (AA)      | enforced                                 |
| UI controls (icons, borders, focus rings)          | 3:1 (AA)      | enforced                                 |
| Critical surfaces (errors, balances, headlines)    | 7:1 (AAA)     | use `text.highlighted` token only        |
| Disabled state                                     | exempt        | no enforcement                           |

A pre-release script verifies contrast across every semantic pair. Failures block release.

Critical surfaces are marked at app level with a platform-appropriate marker (a data attribute on web, a view modifier on Apple, a Compose modifier on Android), which automatically swaps the underlying token to `text.highlighted`.

### 6.2 Focus rings

Style: 2 px outset ring, 2 px offset, color `ring.default`. Never rely on color alone; the ring is also a shape change. Never disable focus visibility without replacement. A "Skip to main content" link at the top of every web page is mandatory and visible only on focus.

### 6.3 Keyboard navigation

Every interactive component has explicit keyboard contracts documented in its component file. Universal rules: Tab follows DOM order which equals visual order; no positive `tabindex` values; modals trap focus and restore on close; Esc closes overlays unless destructive confirmation is required; no keyboard-only flows that have no visible affordance.

### 6.4 Screen-reader

Every interactive component requires an accessible name. Icon-only buttons require `aria-label`. Decorative icons require `aria-hidden="true"`. Toasts use `role="status"` (info/success) or `role="alert"` (error). Loading regions use `aria-busy="true"` plus `aria-live="polite"` to announce completion. Forms always associate `<label>` and field; errors are announced via `aria-describedby`. Headings never skip levels. Landmark roles (`<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`) are mandatory on page-level layouts.

### 6.5 Motion and vestibular

Transitions ≥ 200 ms must respect `prefers-reduced-motion`. Parallax and scroll-jacking are forbidden. Auto-play media is opt-in only and must show controls. Spring-bouncy motion always has a non-spring fallback when reduced motion is active.

### 6.6 Touch targets

Minimum 44 × 44 pt on touch surfaces (Apple HIG). Web/desktop pointer interfaces may go to 32 × 32 px. Compact density never shrinks the hit area below 28 × 28 px; smaller visual elements expand the hit area through padding or pseudo-elements.

### 6.7 Color is never alone

Status, required fields, charts, and active states all carry a non-color signal: icon, weight, border, label, or pattern.

### 6.8 Translucency

Glass surfaces never carry critical text (errors, balances, headlines). Body text on glass requires a minimum 5.5:1 contrast (margin over AA 4.5 to compensate for blur-induced contrast loss). `prefers-reduced-transparency` is always honored.

### 6.9 Verification

`axe-core` runs in dev and CI for web. VoiceOver and TalkBack smoke tests run for native components per release. Color-blindness simulation runs against all semantic token pairs. A keyboard-only walkthrough is recorded per release for component PRs.

---

## 7. Per-Platform Mapping

Rules of thumb only. Implementation code lives in app repos.

### 7.1 Web

Stack pin: Nuxt 4 (≥ 4.1), Nuxt UI v4, Tailwind CSS v4 (≥ 4.2), @nuxt/fonts, @nuxt/icon (Lucide collection).

Rules:

- Tokens are consumed via Tailwind v4 `@theme` block. No hand-written hex in app code.
- Nuxt UI components are themed by editing `app.config.ts` and applying DS slot/variant overrides. Never fork Nuxt UI source.
- Glass surfaces use the DS `<DSGlass material="...">` wrapper, which carries solid fallback for `prefers-reduced-transparency`.
- Icons use `<Icon name="lucide:..." />` only. SF Symbols name is the cross-reference key, not a runtime value.
- Fonts are variable-only and self-hosted via `@nuxt/fonts`. No external CDN at runtime.
- Browser support: evergreen last 2 years. No legacy polyfills.
- Reaching for arbitrary Tailwind values (`bg-[#ff0]`) signals a token-coverage gap. File the gap.

### 7.2 Apple

Stack pin: Swift 6, SwiftUI 6 (iOS 26 / macOS Tahoe 26 / iPadOS 26 minimum). SF Pro and SF Mono from system. SF Symbols 6 from system. Newsreader Variable bundled as Resource.

Rules:

- Use native Liquid Glass materials on allowed surfaces (`.regularMaterial`, `.thickMaterial`, etc.). Do not reinvent.
- Tokens land as enums (`DSColor`, `DSFont`, `DSSpacing`, `DSRadius`). Components consume enums, never literals.
- Density default: `.compact` on macOS, `.comfortable` on iOS. The DS theme provides the right default per platform.
- Components inherit Apple HIG defaults (Dynamic Type, accessibility opt-ins, etc.). The DS does not bypass HIG.
- Sheet/popover/full-screen-cover decisions follow HIG. The DS only styles them.
- Do not target iOS or macOS earlier than 26. Liquid Glass APIs require 26.

### 7.3 Android

Stack pin: Kotlin 2.x, Compose Material3 1.4.x stable. Public Sans Variable, Newsreader Variable, JetBrains Mono Variable bundled as Resource fonts. Lucide Compose icons.

Rules:

- DS owns color via a custom `colorScheme`. Material You / Monet dynamic color is disabled by default. Apps may opt in but accept cross-platform divergence.
- Glass approximation is allowed only on API ≥ 31 via `Modifier.blur`. API < 31 always falls back to solid. Document the gap; never hide it.
- DS extends Material3, never replaces. `MaterialTheme` is provided via the `DSTheme` wrapper.
- Tokens reach components through `CompositionLocal`s; recompose on theme/density change only.
- Minimum SDK aligns with what consuming apps support. The DS itself targets API 26+ to access modern blur and typography APIs.

### 7.4 Per-platform doc structure

Each `docs/platform-notes/{web,apple,android}.md` contains: stack pins and version requirements, a short paragraph on how tokens land, a paragraph on how components are themed, glass implementation rules, iconography binding, known divergences, and explicit out-of-scope items.

### 7.5 Cross-platform consistency contract

| Must match across platforms        | Allowed to diverge                             |
| ---------------------------------- | ---------------------------------------------- |
| Color hex values                   | Glass blur fidelity (best-effort per platform) |
| Spacing values                     | Font hinting and metrics                       |
| Radius values                      | Shadow rendering by compositor                 |
| Type sizes                         | Platform-idiomatic gestures                    |
| Motion durations within ±10 ms    | Native overflow / scroll behavior              |
| Accessibility behaviors            | Animation easing curve fidelity (close enough) |

Undocumented divergence is a bug. Documented divergence lives in `docs/platform-notes/<platform>.md` under a "Known divergences" heading.

---

## 8. Governance and Evolution

### 8.1 Versioning

Semantic versioning on the DS repo. MAJOR for token rename, removal, or visual breaking change. MINOR for additive (new tokens, components, variants). PATCH for value tweaks within the same role, doc clarifications, bug fixes. Every release is tagged in git as `vMAJOR.MINOR.PATCH` and entered in `CHANGELOG.md`. Apps pin a version: caret for minor auto-bumps, exact for production stability.

### 8.2 Naming rules

Lowercase, dot-namespaced. No verbs in names. Sizes use the fixed scale. Themes use file split, not key suffixes. Names describe role, not appearance.

### 8.3 Authoring discipline

Token additions follow primitive → semantic → component. Skipping tiers is a review failure. New components require a doc file from `_template.md`, tokens at the right tier, and at least one example. Visual decisions are authored once in `tokens/`. The `dist/` folder is never edited by hand. Each doc file ends with a "Last updated" date and version.

### 8.4 Contribution rules

Token PRs include before/after contrast checks and visual diffs. Component PRs update the doc, tokens, and at least one example. Naming changes require a major version bump. Breaking changes carry a migration note in `CHANGELOG.md`.

### 8.5 Evolution path

| Phase | Trigger                                  | Output                                              |
| ----- | ---------------------------------------- | --------------------------------------------------- |
| 0     | now                                      | `docs/`, `tokens/`, no build pipeline yet           |
| 1     | first Nuxt 4 app integrates              | add Style Dictionary, emit `dist/web/`              |
| 2     | first SwiftUI app integrates             | add `dist/apple/` emitter                           |
| 3     | first Compose app integrates             | add `dist/android/` emitter                         |
| 4     | Figma design begins                      | emit `dist/figma/tokens.studio.json`; install plugin |
| 5     | DS used in 2+ shipped apps               | tag `v1.0.0`; freeze breaking changes 6 months      |
| 6     | accumulated breaking-change requests     | batch into a single major bump                      |

### 8.6 Decision log (ADRs)

`docs/decisions/` holds one ADR per major choice. Format: title, date, decision, rationale, trade-offs accepted, reversible (yes/no).

ADRs to seed at v0:

1. Why three-tier token hierarchy
2. Why W3C DTCG over Tokens Studio JSON as source
3. Why Newsreader as primary serif
4. Why glass on chrome only
5. Why disable Material You by default on Android
6. Why iOS/macOS 26 minimum
7. Why density toggle (vs fixed compact)

### 8.7 Scope guards (what DS does NOT do)

The DS does not own copywriting, animation choreography, data viz styling beyond color tokens, custom icon assets (uses SF Symbols + Lucide upstream), illustrations, brand identity (logos, marks, voice), or reference component implementations beyond the small Tier B set.

### 8.8 Health checks per release

Token contrast passes AA on all pairs and AAA on `text.highlighted` pairs. Doc cross-references resolve. Token names pass the naming-rule lint. Per-platform docs carry a "Known divergences" entry where relevant. All v1 components have an entry in `docs/components/`.

---

## 9. Open Items

These are decisions the user has explicitly deferred to first-app feedback:

- Specific values for color saturation and lightness on accent ramps (currently mirroring macOS defaults; may shift after first design pass in Figma).
- Final Newsreader optical-size axis usage thresholds.
- Concrete spring stiffness/damping values for `motion.spring.standard` and `motion.spring.bouncy`.
- Whether accent picker UI is exposed by default or always app-level opt-in.

These items do not block v0. Each carries a TODO in the corresponding doc file and is revisited after the first app ships.

---

## 10. References

- [Apple — Liquid Glass Documentation](https://developer.apple.com/documentation/TechnologyOverviews/liquid-glass)
- [Apple — New software design announcement](https://www.apple.com/newsroom/2025/06/apple-introduces-a-delightful-and-elegant-new-software-design/)
- [WWDC 2025 — Meet Liquid Glass](https://developer.apple.com/videos/play/wwdc2025/219/)
- [Liquid Glass — Wikipedia overview](https://en.wikipedia.org/wiki/Liquid_Glass)
- [Nuxt 4 release announcement](https://nuxt.com/blog/v4)
- [Nuxt 4.4 release notes](https://nuxt.com/blog/v4-4)
- [Nuxt UI v4 announcement](https://nuxt.com/blog/nuxt-ui-v4)
- [Nuxt UI Design System docs](https://ui.nuxt.com/docs/getting-started/theme/design-system)
- [Nuxt UI CSS Variables](https://ui.nuxt.com/docs/getting-started/theme/css-variables)
- [Tailwind CSS v4 stable release](https://tailwindcss.com/blog/tailwindcss-v4)
- [Compose Material3 release notes](https://developer.android.com/jetpack/androidx/releases/compose-material3)
- [W3C Design Tokens Community Group format](https://design-tokens.github.io/community-group/format/)
- [Style Dictionary](https://styledictionary.com/)
- [Tokens Studio for Figma](https://tokens.studio/)
- [Public Sans (Apache 2.0)](https://github.com/uswds/public-sans)
- [Newsreader (OFL)](https://fonts.google.com/specimen/Newsreader)
- [JetBrains Mono (OFL)](https://www.jetbrains.com/lp/mono/)
- [Lucide Icons (ISC)](https://lucide.dev/)
- [SF Symbols 6](https://developer.apple.com/sf-symbols/)
- [Reka UI primitives (used by Nuxt UI v4)](https://reka-ui.com/)

---

**Last updated:** 2026-05-01
**Spec version:** 0.1 (pre-implementation)
