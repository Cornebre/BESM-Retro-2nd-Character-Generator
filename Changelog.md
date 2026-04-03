# Changelog

All notable changes to BESM 2nd Edition Character Creator will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [V01.01.00] — 2026-04-02

### Added

- **Unique Attribute / Unique Defect prompt** — when adding either item, a dialog now asks for a custom name. Cancelling aborts the add. The name is stored as `custom_name` on the item dict, serialised to JSON, and displayed everywhere (list, summary, PDF). A new `item_display_name()` helper returns `custom_name` if set, otherwise the translated game-data name.
- **Horizontally scrollable character tab bar** — `◀` / `▶` arrow buttons scroll through tabs by 60 px per click. Arrows disable automatically when all tabs fit. Switching to or opening a tab scrolls it into view.
- **Auto-resizing description boxes** — all desc fields (attributes, defects, weapons, mecha — in both add panels and existing-item rows) now grow vertically as content is typed, starting at 1 line. Consistent with the existing Character Details / Equipment Notes behaviour.
- **Settings** — All language and colours scheme are no present in this new menu.

### Changed

#### UI / Layout
- Add panels (Attributes, Defects, Weapons, Arsenal): Name field and Desc field now stretch to fill the full column width.
- Level, Modifier and their spinboxes are grouped in a left-anchored sub-frame, keeping them flush-left regardless of panel width. Applies to all three add panels.
- **Cost/Lv** selector moved from a separate row into the Level/Modifier sub-frame, appearing to the right of the Modifier spinbox. Colour changed to `TEXT_DIM` to match surrounding labels.
- Added vertical gap between the Name row and Level row, and between the Level row and Desc row, in the Attributes/Defects add panel — matching the spacing already present in the Weapons panel.
- A visible BG-coloured separator gap now appears between the add panel and the "Added" list for both Attributes/Defects and Weapons.

#### Summary page
- Descriptions for attributes, defects, weapons, skills, combat skills, and mecha sub-sections now appear **to the right of the CP/MP cost** instead of being embedded in the name label.
- Cost labels use a fixed width so the description column starts at a consistent horizontal position regardless of digit count.
- Description labels use `justify="left"` for correct multi-line alignment.
- Same layout applied to the clipboard plain-text summary, with a fixed-width info column for clean alignment.

#### PDF export
- Equipment & Adventuring Notes box now **grows dynamically** to fit its content (was fixed at 22 mm).
- Attribute/Defect table: Notes column now **word-wraps** across multiple lines; row height grows accordingly.
- Weapon table: Desc column now **word-wraps** across multiple lines. Column proportions adjusted (Name 28 % → 24 %, Lv 6 % → 5 %, CP 9 % → 8 %) to give more space to the description. Per-row height estimate updated (9 mm → 11 mm) for pagination.

### Fixed

- **New character starts clean** — `CharacterTab.__init__` now calls `state.mark_clean()` after `_build()` completes, so a brand-new character never shows the unsaved `•` indicator before any data has been changed.
- **Unbound `name` variable** in `Page6Summary.refresh()` — `name = s.char_name.get()` was missing from the method body and has been restored.

### Internal

- Added `from collections.abc import Callable` import (required for `cost_unit: str | Callable` type hint in `WeaponList`).
- Added `simpledialog` to the `tkinter` import (required for the Unique Attribute/Defect name prompt).
- `WeaponList` outer frame changed from `bg=PANEL` to `bg=BG` so padding gaps between its PANEL-coloured children render as visible separators, consistent with `LevelledList`.
- `_available_langs()` now also scans `CONFIG_DIR / "machine_translated"` in dev mode and deduplicates by language code.
- First-run installer dialog: updated descriptive text; saves chosen default language code to prefs directly instead of patching TOML files.

---

## [V01.00.00] — initial release

- Initial public release.