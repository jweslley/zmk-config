# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration repository for custom split keyboards, specifically the "eyelash_sofle" keyboard. ZMK is a modern keyboard firmware built on the Zephyr RTOS that supports wireless (Bluetooth) and wired (USB) connectivity.

## Architecture

### Directory Structure

- `boards/arm/eyelash_sofle/` - Custom board definition for the eyelash_sofle split keyboard
  - `.dtsi` files: Device tree includes defining hardware (GPIO pins, SPI, encoders, RGB LEDs)
  - `.dts` files: Device tree sources for left/right halves
  - `.keymap`: Default keymap included with the board definition
  - `Kconfig.*`: Kernel configuration for the board

- `config/` - User keymap configurations
  - `eyelash_sofle.keymap`: Main keymap file (this is what users typically edit)
  - `eyelash_sofle.conf`: Configuration options (sleep timeout, RGB, mouse support, etc.)
  - `west.yml`: West manifest defining ZMK version and module dependencies

- `keymap-drawer/` - Visual representations of keymaps
  - `.yaml` files: Keymap descriptions for visualization
  - `.svg` files: Generated keymap diagrams

- `keymap_drawer.config.yaml` - Configuration for keymap-drawer tool (SVG styling, key legends)

### Hardware Features

The eyelash_sofle keyboard supports:
- **Split keyboard**: Left/right halves with separate builds
- **Matrix**: 5 rows × 14 columns (6 or 7 columns per side)
- **MCU**: nRF52840 (Nordic Semiconductor - Bluetooth LE capable)
- **Rotary encoder**: On left half (used for volume/scrolling)
- **RGB underglow**: WS2812 LEDs via SPI3
- **Backlight**: PWM-controlled LEDs
- **Nice!View display**: SPI0-connected e-ink display support
- **Mouse emulation**: Pointer and scroll wheel support via ZMK pointing
- **Soft off**: Low-power shutdown mode
- **ZMK Studio**: Runtime keymap editing support (studio build variant)

### Keymap Structure

Keymaps are defined in Device Tree format:
- `config/eyelash_sofle.keymap` - Primary keymap edited by users
- `boards/arm/eyelash_sofle/eyelash_sofle.keymap` - Board default keymap

**6 Layers:**
- **Layer 0 (Default)**: QWERTY base with home row mods, media controls, tmux prefix, key repeat
- **Layer 1 (Symbol)**: Symbols optimized for Ruby, tmux, vim, and Markdown — `^@<>=|` left, `,/; ( ) [ ] { } _ : . / # $ %` right, arrows on center column
- **Layer 2 (Navigation)**: Function keys (F1-F12), arrows, page navigation, macOS shortcuts
- **Layer 3 (System)**: Bluetooth profiles, RGB controls, USB/BLE switching, soft-off, bootloader
- **Layer 4 (Number)**: Numpad layout right hand (7-8-9 top, 1-2-3 home, 4-5-6 bottom, 0 thumb), operators left (*/÷, +/-, ./,), `( = )` and `%` left
- **Layer 5 (Mouse)**: Mouse movement/scroll, mouse buttons (L/M/R + MB4/MB5)

**Custom Behaviors:**
- Home row mods (`&hml`/`&hmr`) with balanced flavor, 280ms tapping term, require-prior-idle
- Key repeat behavior on right thumb
- Tmux prefix macro (Ctrl+A) on left thumb
- Mouse movement (`&mmv`) and scrolling (`&msc`) with acceleration
- Soft-off with 2-second hold time
- Tap-dances: `td_comma_semi` (,/;), `td_excl_qmark` (!/?), `td_mo1_num` (sym/num), `td_dot_comma` (./,), `td_plus_minus` (+/-), `td_mul_div` (*/÷)

## Build Configuration

The repository uses GitHub Actions for automated building:

### Build Targets (build.yaml)

The firmware builds three variants:
1. **eyelash_sofle_right** - Right half with nice_view display
2. **eyelash_sofle_left** - Left half with nice_view display and normal firmware
3. **eyelash_sofle_studio_left** - Left half with ZMK Studio enabled for live editing
   - Includes USB-UART snippet for Studio RPC
   - Studio locking disabled for convenience

A settings reset firmware is also built for `nice_nano_v2`.

### Building Locally

ZMK uses the West build system. Standard workflow:

```bash
# Initialize workspace (first time only)
west init -l config/
west update

# Build for left half
west build -d build/left -b eyelash_sofle_left -- -DZMK_CONFIG="$(pwd)/config"

# Build for right half
west build -d build/right -b eyelash_sofle_right -- -DZMK_CONFIG="$(pwd)/config"

# Build with ZMK Studio support
west build -d build/studio -b eyelash_sofle_left -- -DZMK_CONFIG="$(pwd)/config" -DCONFIG_ZMK_STUDIO=y -DCONFIG_ZMK_STUDIO_LOCKING=n
```

### Flashing Firmware

```bash
# Put keyboard in bootloader mode, then:
west flash -d build/left
```

Or manually copy the `.uf2` file from `build/left/zephyr/zmk.uf2` to the mounted bootloader drive.

## Keymap Visualization

The repository auto-generates keymap SVG diagrams using keymap-drawer:

```bash
# Generate keymap visualization (done automatically via GitHub Actions)
keymap parse -c keymap_drawer.config.yaml -z config/eyelash_sofle.keymap > keymap-drawer/eyelash_sofle.yaml
keymap draw keymap-drawer/eyelash_sofle.yaml > keymap-drawer/eyelash_sofle.svg
```

GitHub workflow (`.github/workflows/draw.yml`) automatically updates SVG files when keymap changes are pushed.

## Configuration Files

### eyelash_sofle.conf

Key configuration options:
- `CONFIG_ZMK_SLEEP=y` - Enable sleep after 1 hour idle
- `CONFIG_ZMK_RGB_UNDERGLOW=y` - RGB LED support
- `CONFIG_ZMK_POINTING=y` - Mouse pointer emulation
- `CONFIG_ZMK_BACKLIGHT=y` - Backlight support
- `CONFIG_ZMK_PM_SOFT_OFF=y` - Soft power-off capability
- `CONFIG_EC11=y` - Rotary encoder support
- `CONFIG_BT_CTLR_TX_PWR_PLUS_8=y` - Increased Bluetooth transmit power

### Mouse/Pointing Configuration

Mouse behavior is customized in keymap headers:
- `ZMK_POINTING_DEFAULT_MOVE_VAL` - Mouse movement speed
- `ZMK_POINTING_DEFAULT_SCRL_VAL` - Scroll wheel speed
- `time-to-max-speed-ms`, `acceleration-exponent` - Acceleration curves
- Input processors: `zip_xy_scaler` for scaling movement/scroll

## Development Workflow

1. **Edit keymap**: Modify `config/eyelash_sofle.keymap`
2. **Test locally**: Build and flash to keyboard for testing
3. **Push changes**: Commit and push to trigger automated builds
4. **Download firmware**: Get `.uf2` files from GitHub Actions artifacts
5. **Keymap visuals**: Updated automatically by draw.yml workflow

## Layer Access Keys

- **Layer 1 (Symbol)**: Hold LH1/56 (left thumb); double-tap LH1/56 toggles Layer 4
- **Layer 2 (Navigation)**: Hold RH1/60 (right thumb)
- **Layer 3 (System)**: Hold RH1/60 while in Layer 1
- **Layer 4 (Number)**: Double-tap LH1/56 to toggle on; press LH1/56 again to exit
- **Layer 5 (Mouse)**: Hold LH4/53 (left thumb)

## Position Reference

The keyboard has 64 positions (0-63) across 5 rows:
- **Rows 1-4**: 13 keys each (6 left + 1 center + 6 right)
- **Row 5 (thumb)**: 12 keys (6 left + 1 center + 5 right)
- **Position 52**: Encoder (hardwired, must be `&none`)
- **Position 58**: Center thumb key (accessible by both thumbs)

## Important Notes

- Device tree files use `.dtsi` (include) and `.dts` (source) conventions
- The keymap uses ZMK's behavior system (`&kp`, `&mo`, `&mkp`, `&mmv`, `&msc`, etc.)
- Split keyboard requires building separate firmware for each half
- The left half is typically the "central" side (USB connection)
- Bluetooth profiles (BT1-BT5) are accessed via Layer 3
- Soft-off is triggered via Layer 3, position LH4/53 (hold for 2 seconds)
- Home row mods: Left `A=⌘, S=⌥, D=⇧, F=⌃` | Right `J=⌃, K=⇧, L=⌥, ;=⌘`
- Mouse layer uses hjkl pattern for movement and scrolling on right hand
- Number layer has `( = )` on LM2/LM1/LM0 and `%` on LB0 for expression entry
