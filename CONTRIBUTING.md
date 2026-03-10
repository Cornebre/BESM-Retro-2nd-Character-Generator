# Contributing to BESM 2nd Edition Character Creator

Thank you for your interest in contributing! This project welcomes two types of contributions: **translation files (TOML)** and **code improvements (Python)**. Both follow the same fork & pull request workflow.

---

## How to Contribute (Quick Summary)

1. **Fork** this repository on GitHub
2. Make your changes on your fork
3. Open a **Pull Request** — I'll review and merge it

You don't need to be added as a collaborator. Anyone can propose changes.

---

## Contributing a Translation

### 1. Copy an existing TOML

Start from an existing file (e.g. `besm2nd_config_eng.toml`) and rename it:

```
besm2nd_config_XXX.toml
```

Replace `XXX` with a short language code (e.g. `deu` for German, `esp` for Spanish, `jpn` for Japanese).

### 2. Edit the `[lang]` section

At the top of the file, update the language metadata:

```toml
[lang]
label   = "Deutsch"     # Display name shown in the language selector
default = false         # Set to true only if you want this as the default language
```

### 3. Translate the `[ui]` section

This section contains all the interface strings. Translate the **values** (right side), never the **keys** (left side):

```toml
[ui]
# ✅ Correct
label_body = "Corps"

# ❌ Wrong — don't translate the key
corps = "Body"
```

### 4. Translate game data entries

Each attribute, defect, skill, weapon advantage/defect, and mecha entry has a `name` field and optionally a `key` field.

- The `key` field (if present) must **never be translated** — it is used internally to match saved characters across languages.
- The `name` field is the **display name** that players will see — translate this one.

```toml
# Example: translate "name", leave "key" unchanged
[[attributes]]
key   = "Highly Skilled"   # ← DO NOT TRANSLATE
name  = "Très compétent"   # ← translate this
costs = [2]
```

If there is no `key` field, the `name` field is used as the internal key — **do not translate it** in that case, as it would break compatibility with save files from other languages.

### 5. Translate settings names

The `[settings_names]` table maps English campaign setting names to their display equivalent. Translate the values:

```toml
[settings_names]
"Action Adventure" = "Action-Aventure"
"Dark Fantasy"     = "Heroic Fantasy sombre"
```

### 6. Leave formulas and costs untouched

The `expression` fields in `[stat_formulas]` and `[attr_formulas]` are arithmetic expressions — do not modify them.

### 7. Test your translation

Run the app from source with your new TOML in the `BESM2nd Config/` folder:

```bash
python BESM2nd_CharGen.py
```

Switch to your language using the language selector in the top right and check:
- All UI labels appear correctly
- Attributes, Defects, Skills, and Weapons display translated names
- Creating a character and saving/loading works without errors

---

## Contributing Code

### Setting up

```bash
git clone https://github.com/YOUR_USERNAME/BESM2nd-CharGen.git
cd BESM2nd-CharGen
python BESM2nd_CharGen.py
```

Python 3.11+ is required. No external dependencies are needed to run the app (only `reportlab` for PDF export, which is optional).

### Guidelines

- Keep the code in a **single file** (`BESM2nd_CharGen.py`) — this is intentional to simplify distribution.
- New UI strings should be added to the `[ui]` section of the TOML files and referenced with the `t()` function, so they are translatable.
- If you add new game data entries (attributes, defects, skills, etc.), add a `key` field that is distinct from the `name` so translators can provide translated names without breaking save compatibility.
- Do not commit `.pyc` files or the `dist/` / `build/` folders — they are in `.gitignore`.

### Submitting a Pull Request

Please describe in your PR:
- What you changed and why
- For translations: which language and what was added or corrected
- For code: which bug was fixed or feature was added

---

## TOML File Structure Reference

| Section | Description |
|---|---|
| `[lang]` | Language metadata (`label`, `default`) |
| `[ui]` | All interface strings |
| `[stat_formulas]` | Stat cost formulas (Body/Mind/Soul) |
| `[attr_formulas]` | Attribute cost formulas |
| `[[attributes]]` | Character attributes list |
| `[attr_modifies]` | Which stats attributes affect |
| `[[defects]]` | Character defects list |
| `[defect_modifies]` | Which stats defects affect |
| `[[weapon_advantages]]` | Weapon advantage definitions |
| `[[weapon_defects]]` | Weapon defect definitions |
| `[[mecha_attributes]]` | Mecha-only attributes |
| `[[mecha_defects]]` | Mecha-only defects |
| `[mecha]` | Mecha configuration |
| `[skills_config]` | Available campaign settings |
| `[skills]` | Regular skills with costs per setting |
| `[combat_skills]` | Combat skills |
| `[settings_names]` | Display names for campaign settings |

---

## Questions?

Open a [GitHub Issue](../../issues) if you have a question, spot a bug, or want to discuss a new feature before working on it.
