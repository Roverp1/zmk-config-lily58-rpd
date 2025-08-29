# AGENTS.md

## Build/Test Commands
- **Build firmware**: Push to GitHub (auto-builds via Actions) or use local ZMK build environment
- **Test keymap**: Use ZMK Studio for real-time testing without flashing firmware
- **Validate syntax**: ZMK build will catch devicetree syntax errors

## Code Style Guidelines
- **File structure**: Main keymap in `config/lily58.keymap`, behaviors in `config/behaviors/`
- **Includes**: Always include `<behaviors.dtsi>` and `<dt-bindings/zmk/keys.h>` at top
- **Naming**: Use SCREAMING_SNAKE_CASE for layer defines, snake_case for behavior nodes
- **Comments**: Use C-style comments (`//` or `/* */`). Include ASCII art for visual layouts
- **Constants**: Define timing constants in `constants.dtsi`, reference via `#define`
- **Behaviors**: Place custom behaviors in separate `.dtsi` files, use semantic node names
- **Layout**: Align bindings in visual keyboard layout format with proper indentation
- **Parameters**: Use semantic key labels (LT0, RM1) instead of numeric positions
- **Timing**: Use "balanced" flavor for hold-taps, configure per-modifier timing
- **Configuration**: Hardware settings in `.conf`, behavior definitions in `.dtsi`
- **Dependencies**: External libraries via `west.yml`, internal includes via relative paths

## ZMK-Specific Notes
- **Architecture**: 8-layer config with QWERTY/RPD layouts, timeless home row mods
- **Studio integration**: Real-time keymap editing enabled via `studio-rpc-usb-uart`
- **Mouse support**: Built-in mouse emulation in RAISE layer using `&mkp`, `&mmv`, `&msc`
- **Gaming modes**: Dedicated gaming layers disable home row mods for clean inputs