# Claude Instructions for ZMK Glove80 Configuration

**AUTO-UPDATE TRIGGER**: When you say "update claude.md" or make significant project changes, Claude should automatically update this file to reflect the current project state and structure.

This is a minimal ZMK firmware configuration specifically for the Glove80 keyboard, derived from urob's comprehensive ZMK setup.

## Project Structure

```
min-urob-zmk/
├── config/
│   ├── base.keymap          # Main keymap with layers and behaviors
│   ├── glove80.conf         # Glove80-specific hardware configuration
│   ├── glove80.keymap       # Glove80 layout wrapper (includes base.keymap)
│   ├── combos.dtsi          # Combo key definitions
│   ├── leader.dtsi          # Leader key sequences
│   ├── mouse.dtsi           # Smart mouse layer configuration
│   └── west.yml             # ZMK dependencies and module versions
├── build.yaml               # GitHub Actions build matrix (glove80_lh/glove80_rh only)
├── Justfile                 # Build automation recipes
├── flake.nix               # Nix development environment
└── draw/                   # Keymap visualization assets
```

## Key Project Semantics

### Architecture
- **Glove80-only**: All non-Glove80 keyboards (Corneish Zen, Planck) have been removed
- **Modular design**: Core layout in `base.keymap`, hardware-specific config in `glove80.keymap`
- **Feature modules**: Combos, leader keys, and mouse functionality are separated into `.dtsi` files
- **Module-driven**: Uses 6 of urob's ZMK modules for advanced functionality

### Core Features
- **Timeless homerow mods**: Advanced HRM setup with balanced flavor, positional hold-tap
- **Combo-based symbols**: No dedicated symbol layer, uses combos instead
- **Smart layers**: Auto-deactivating Numword and Smart-mouse layers
- **Magic thumb**: Repeat/Sticky-shift/Capsword on right thumb
- **Leader sequences**: Unicode input and system commands via S+T combo

### urob's ZMK Modules Integration

**1. zmk-helpers** - Simplifies configuration syntax
- `ZMK_COMBO()` macro for easy combo definitions
- `ZMK_HOLD_TAP()` macro for behavior creation
- `MAKE_HRM()` custom macro for homerow mod generation
- Key position labels (LT0-LT4, RT0-RT4, etc.)

**2. zmk-auto-layer** - Smart layer management
- Powers the "Smart-Num" (Numword) functionality
- Auto-deactivates layers when non-specified keys are pressed
- `num_word NUM` behavior keeps NUM layer active while typing numbers

**3. zmk-adaptive-key** - Context-aware key transformations
- Powers the "Magic Shift" repeat/sticky-shift behavior
- `shift_repeat` adapts based on previous keypress context
- Transforms tap after alpha to repeat, otherwise sticky-shift

**4. zmk-tri-state** - Three-stage interaction patterns
- Powers window switcher (`swapper`) behavior
- Powers smart-mouse auto-toggle functionality
- Stage 1: activate, Stage 2: continue, Stage 3: interrupt

**5. zmk-leader-key** - Vim-style leader sequences
- Activated by S+T combo (`&leader`)
- German umlauts (A→ä, O→ö, U→ü, S→ß)
- Greek letters for math (E A→α, E B→β, etc.)
- System commands (USB, BLE, RESET, BOOT)

**6. zmk-unicode** - Advanced Unicode input
- German and Greek character definitions
- Cross-platform Unicode input (macOS/Linux/Windows)
- `&uc UC_DE_AE` syntax for character codes

### Build System
- Uses `build.yaml` to define build targets (only `glove80_lh` and `glove80_rh`)
- **ZMK Source**: Uses Moergo fork (v25.08) instead of official ZMK for Glove80 optimizations
- Justfile provides convenient build recipes (`just build glove80`, `just list`)
- Nix + direnv development environment for reproducible builds

## Current Project State

**Personal Configuration Status**: Building minimal Glove80-only config from scratch
- Original urob config preserved in `config-urob/` for reference  
- New personal config in `config/` with modular bluetooth behaviors
- Using `zmk-helpers` key position definitions (LT0, LM0, etc.) for portability

**Known Issues**: Just recipes fixed for WSL (XDG_RUNTIME_DIR=/tmp in flake.nix)

## Auto-Update Instructions

### Automatic CLAUDE.md Updates
When the user says "update claude.md" or when making significant changes to the project, Claude should:

1. **Scan current project state**:
   ```bash
   find config/ -name "*.keymap" -o -name "*.conf" -o -name "*.dtsi" -o -name "*.yml"
   ls -la
   cat build.yaml
   git log --oneline -5
   ```

2. **Update this file sections**:
   - Project Structure: Reflect any new/removed files
   - Key Project Semantics: Update if architecture changes
   - File descriptions: Update if file purposes change
   - Build targets: Sync with current build.yaml
   - Version info: Update if dependencies change

3. **Preserve user customizations**: Never overwrite user-added sections or notes

### Regular Project Work
When working on this project, Claude should:

### 1. Always Check Project State First
```bash
# Understand current structure
ls -la config/
cat build.yaml
git status
```

### 2. Maintain Glove80-Only Focus
- Never add configurations for other keyboards
- Keep build.yaml limited to glove80_lh and glove80_rh
- Preserve the modular .dtsi structure

### 3. Key Files to Monitor
- `config/base.keymap` - Main layout logic
- `config/glove80.keymap` - Hardware wrapper
- `config/*.dtsi` - Feature modules (combos, leader, mouse)
- `config/west.yml` - Dependencies (check for version updates)

### 4. Before Making Changes
- Read relevant config files to understand current implementation
- Check if changes align with existing patterns and conventions
- Verify changes don't break the modular structure

### 5. Testing and Validation
- After changes, check build targets: `cat build.yaml`
- Ensure no non-Glove80 references: `grep -r "corneish\|planck" config/`
- Verify keymap syntax if modified

### 6. Common Tasks & Configuration Patterns

#### Adding new combos:
1. Edit `config/combos.dtsi`
2. Follow existing ZMK_COMBO pattern:
   ```c
   ZMK_COMBO(name, &kp KEY, LT1 LT2, DEF NAV NUM, COMBO_TERM_FAST, COMBO_IDLE_FAST)
   ```
3. Use `COMBO_TERM_FAST/SLOW` and `COMBO_IDLE_FAST/SLOW` constants
4. For HRM-overlapping combos, use 8-argument ZMK_COMBO version

#### Adding leader sequences:
1. Edit `config/leader.dtsi`
2. Use ZMK_LEADER_SEQUENCE macro:
   ```c
   ZMK_LEADER_SEQUENCE(name, &behavior KEY, KEY_SEQUENCE)
   ```
3. Access via `&leader` (S+T combo)

#### Modifying homerow mods:
1. Edit `config/base.keymap` MAKE_HRM section
2. Key parameters: `tapping-term-ms = <280>`, `require-prior-idle-ms = <150>`
3. Uses `balanced` flavor with `hold-trigger-on-release`
4. Positional hold-tap prevents same-hand false triggers

#### Creating new behaviors:
1. Use zmk-helpers macros: `ZMK_HOLD_TAP()`, `ZMK_MOD_MORPH()`, etc.
2. Follow existing patterns in `base.keymap`
3. Define aliases with `#define` for readability

#### Smart layer configuration:
1. Numword: Uses `zmk-auto-layer` module, triggered by `SMART_NUM`
2. Smart-mouse: Uses `zmk-tri-state`, triggered by combo W+P
3. Both auto-deactivate on non-specified keypresses

#### Updating dependencies:
1. Check `config/west.yml` for module versions:
   - urob modules: pinned to v0.3  
   - ZMK: uses Moergo fork v25.08 (based on upstream v0.3.0)
2. Update remote refs if needed
3. Run `just init` (or `west update && west zephyr-export`) after west.yml changes
4. Test build after changes

#### Development environment setup:
1. Requires nix package manager and direnv
2. Run `direnv allow` in project directory
3. Environment auto-loads when entering directory (first time is slow)
4. Provides `just`, `west`, and full ZMK build tools

## Development Notes

- Uses urob's zmk-helpers for cleaner syntax
- Requires `require-prior-idle-ms` for combo reliability
- Timeless HRM config is in base.keymap (280ms tapping-term, balanced flavor)
- Build environment is fully containerized with Nix

## Troubleshooting

If builds fail:
1. Check `config/west.yml` for correct module references
2. Verify no syntax errors in `.keymap` files
3. Ensure all `.dtsi` includes are valid
4. Check that only Glove80 boards are in `build.yaml`

Remember: This is a minimal, Glove80-focused configuration. Maintain that focus and preserve the advanced features that make this setup unique.

---

## Auto-Update Trigger Commands

Tell Claude any of these to trigger an automatic update of this file:
- "update claude.md"
- "refresh claude instructions" 
- "sync claude.md with project"
- Or after making significant project changes, Claude should proactively ask: "Should I update CLAUDE.md to reflect these changes?"

**Last Updated**: 2025-09-03 - Added urob ZMK module semantics, Moergo ZMK fork integration, and nix/direnv setup