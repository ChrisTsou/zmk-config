# Claude Instructions for ZMK Glove80 Configuration

**AUTO-UPDATE TRIGGER**: When you say "update claude.md" or make significant project changes, Claude should automatically update this file to reflect the current project state and structure.

This is a minimal ZMK firmware configuration specifically for the Glove80 keyboard, derived from urob's comprehensive ZMK setup.

## Project Structure

```
min-urob-zmk/
├── config/
│   ├── glove80.keymap       # Personal minimal Glove80 configuration with 6 layers
│   ├── glove80.conf         # Glove80-specific hardware configuration  
│   ├── bluetooth.dtsi       # Modular bluetooth behaviors (tap-dance profiles)
│   └── west.yml             # ZMK dependencies using Moergo fork + urob modules
├── config-urob/             # Original urob reference configuration (preserved)
│   ├── base.keymap          # urob's main keymap with layers and behaviors
│   ├── glove80.keymap       # urob's Glove80 layout wrapper
│   ├── combos.dtsi          # urob's combo key definitions
│   ├── leader.dtsi          # urob's leader key sequences  
│   ├── mouse.dtsi           # urob's smart mouse layer configuration
│   └── west.yml             # urob's original dependencies
├── build.yaml               # GitHub Actions build matrix (glove80_lh/glove80_rh only)
├── Justfile                 # Build automation recipes (WSL-compatible)
├── flake.nix               # Nix development environment with XDG_RUNTIME_DIR fix
├── glove80_ascii_gen.py     # Python ASCII art generator for Glove80 layouts
├── glove80_ascii_gen.js     # JavaScript ASCII art generator for Glove80 layouts  
└── draw/                   # Keymap visualization assets
```

## Key Project Semantics

### Architecture
- **Glove80-only**: All non-Glove80 keyboards (Corneish Zen, Planck) have been removed
- **Personal minimal config**: New streamlined configuration in `config/` with 6 layers (BASE/SYM/NUM/NAV/FUN/MGC)
- **Reference preservation**: Original urob config preserved in `config-urob/` for reference
- **Modular bluetooth**: Bluetooth behaviors separated into `bluetooth.dtsi` for reusability  
- **Module-driven**: Uses urob's zmk-helpers for key position definitions and clean syntax
- **Moergo ZMK fork**: Uses Moergo's optimized ZMK fork (v25.08) instead of upstream

### Current Configuration Features
- **6-layer layout**: BASE, SYM, NUM, NAV, FUN, MGC layers with clear purposes
- **Homerow mods**: Left/right HRM behaviors with balanced flavor and positional hold-tap
- **Magic Shift**: Custom tap-dance shift behavior (single=one-shot, double=caps-word, shift+tap=caps-lock)
- **Cancel key**: K_CANCEL for deactivating caps-word and sticky layers
- **Bluetooth management**: 4 BT profiles with tap-dance behaviors (tap=connect, hold=disconnect)
- **Magic button**: RGB status indicator for battery/bluetooth/caps lock (working)
- **Layer-tap behaviors**: Dedicated left/right layer-tap behaviors with proper trigger positions
- **zmk-helpers integration**: Uses urob's key position definitions (LT0, LM0, RT0, etc.)
- **Optimized sticky keys**: Quick-release behavior for single-letter capitalization

### Available urob's ZMK Modules (Reference)

The following modules are available from urob's ecosystem but not currently used in the minimal config:

**1. zmk-helpers** - ✅ **ACTIVE** - Simplifies configuration syntax
- Key position labels (LC5, LT0, LM0, RT0, etc.) for Glove80
- Provides clean syntax for keymap definitions

**2. zmk-auto-layer** - ⚪ Available - Smart layer management  
- Powers "Smart-Num" (Numword) functionality in urob's config
- Auto-deactivates layers when non-specified keys are pressed

**3. zmk-adaptive-key** - ⚪ Available - Context-aware key transformations
- Powers "Magic Shift" repeat/sticky-shift behavior in urob's config
- Adapts behavior based on previous keypress context

**4. zmk-tri-state** - ⚪ Available - Three-stage interaction patterns
- Powers window switcher and smart-mouse functionality in urob's config
- Stage 1: activate, Stage 2: continue, Stage 3: interrupt

**5. zmk-leader-key** - ⚪ Available - Vim-style leader sequences
- Provides leader key functionality for Unicode input
- Can be activated by combos for special character sequences

**6. zmk-unicode** - ⚪ Available - Advanced Unicode input
- Cross-platform Unicode input (macOS/Linux/Windows)
- German, Greek, and other character definitions

### Build System
- Uses `build.yaml` to define build targets (only `glove80_lh` and `glove80_rh`)
- **ZMK Source**: Uses Moergo fork (v25.08) instead of official ZMK for Glove80 optimizations
- Justfile provides convenient build recipes (`just build glove80`, `just list`)
- Nix + direnv development environment for reproducible builds

## Current Project State

**Status**: ✅ **WORKING** - Full Glove80 configuration with Magic Shift successfully built and tested

### Key Accomplishments
- ✅ **Build System**: Moergo ZMK fork integration complete (v25.08)
- ✅ **WSL Compatibility**: Just recipes fixed (XDG_RUNTIME_DIR=/tmp in flake.nix)
- ✅ **Key Position Mapping**: zmk-helpers integration working (LT0, LM0, RT0, etc.)
- ✅ **Magic Button Fix**: RGB status indicators working correctly
- ✅ **Modular Design**: Bluetooth behaviors extracted to reusable .dtsi file
- ✅ **ASCII Art Generators**: Python and JavaScript tools for layout visualization (fixed alignment issues)
- ✅ **Magic Shift Implementation**: Custom tap-dance shift behavior fully working
- ✅ **Cancel Key**: K_CANCEL key for caps-word/sticky layer cancellation
- ✅ **Optimized Sticky Keys**: Configured for single-letter capitalization with quick-release

### Current Configuration
- **Personal config**: `config/` contains working 6-layer Glove80 setup with Magic Shift
- **Reference config**: `config-urob/` preserved for advanced features reference
- **Magic Shift behaviors**: Single tap=one-shot shift, Double tap=caps-word, Shift+tap=caps-lock
- **Timing optimizations**: Sticky keys (500ms timeout, quick-release), tap-dance (500ms window)
- **Working features**: HRM, bluetooth profiles, layer-tap, magic status button, cancel key
- **Ready for expansion**: Can easily add urob modules (combos, leader keys, etc.)

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
- `config/glove80.keymap` - Personal minimal Glove80 configuration
- `config/bluetooth.dtsi` - Modular bluetooth behaviors  
- `config/west.yml` - Dependencies with Moergo ZMK fork + urob modules
- `config-urob/base.keymap` - Reference: urob's main layout logic
- `config-urob/*.dtsi` - Reference: urob's feature modules (combos, leader, mouse)

### 4. Before Making Changes
- Read relevant config files to understand current implementation
- Check if changes align with existing patterns and conventions
- Verify changes don't break the modular structure

### 5. Testing and Validation
- After changes, check build targets: `cat build.yaml`
- Ensure no non-Glove80 references: `grep -r "corneish\|planck" config/`
- Verify keymap syntax if modified

### 6. Common Tasks & Configuration Patterns

#### Current Configuration Pattern:
1. **Main keymap**: Edit `config/glove80.keymap` 
2. **Key positions**: Use zmk-helpers labels (LT0, LM0, RT0, RM0, etc.)
3. **Homerow mods**: Defined in behaviors section with `balanced` flavor
   - `tapping-term-ms = <280>`, `require-prior-idle-ms = <150>`
   - Separate left/right behaviors with positional hold-tap
4. **Layer structure**: 6 layers (BASE=0, SYM=1, NUM=2, NAV=3, FUN=4, MGC=5)
5. **Magic Shift**: Custom tap-dance implementation using ZMK helper macros
   - Hold-tap → Mod-morph → Tap-dance structure
   - Timing: 500ms tap-dance window, 500ms sticky key timeout with quick-release
6. **Cancel functionality**: K_CANCEL key for caps-word and sticky layer deactivation
7. **Bluetooth**: Modular behaviors in `bluetooth.dtsi` with tap-dance patterns

#### Adding urob's Advanced Features (Reference):
1. **Combos**: Copy patterns from `config-urob/combos.dtsi`
2. **Leader sequences**: Copy patterns from `config-urob/leader.dtsi` 
3. **Smart behaviors**: Copy from `config-urob/base.keymap`
4. **Auto-layer/Tri-state**: Reference urob's module usage patterns
5. **Unicode**: Reference `config-urob/` for character definitions

#### ASCII Art Generation:
1. **Python**: `python3 glove80_ascii_gen.py` for templates
2. **JavaScript**: `node glove80_ascii_gen.js` for templates  
3. **Usage**: `substituteBindings(template, your_80_bindings_array)`
4. **Key mapping**: Uses urob's position labels (LC5, LT0, LM0, etc.)

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

**Last Updated**: 2025-09-03 - Project enhanced: Working Glove80 config with Magic Shift, optimized ASCII generators, and K_CANCEL functionality