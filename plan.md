# ZMK Keyboard Layout Redesign Plan

**Goal**: Redesign keyboard layers for Ruby development workflow (nvim, tmux, Chrome on macOS)

**Design Principles**:
- Most-used keys on home row, rare keys toward corners
- Home row mods with Ctrl on index finger
- Avoid pinky double-tapping (use repeat key)
- Optimize for Ruby bigrams as inward rolls (!=, <=, +=, ->, =>)
- Keep QWERTY base for easy learning

---

## Keyboard Layout Reference

Your eyelash_sofle has **65 keys total** arranged as:
- **Rows 1-4**: 13 keys each (6 left + 1 center + 6 right)
- **Row 5 (thumb)**: 12 keys (6 left + 1 center + 5 right)

### Position Mapping

```
Row 1 (0-12):    [0-5]     [6]      [7-12]      (left 6, center 1, right 6)
Row 2 (13-25):   [13-18]   [19]     [20-25]     (left 6, center 1, right 6)
Row 3 (26-38):   [26-31]   [32]     [33-38]     (left 6, center 1, right 6)
Row 4 (39-51):   [39-44]   [45]     [46-51]     (left 6, center 1, right 6)
Row 5 (52-63):   [52-57]   [58]     [59-63]     (left 6, center 1, right 5)
                  ↑
                 MUTE (hardwired, must always be C_MUTE)
```

**Current Thumb Row (Row 5)**:
- **Position 52**: C_MUTE (hardwired to encoder, cannot be changed)
- **Left thumb (53-57)**: LCTRL, LGUI, LALT, mo 1, SPACE (5 usable keys)
- **Center (58)**: ENTER (1 key, accessible by both thumbs)
- **Right thumb (59-63)**: ENTER, mo 2, RCTRL, RSHFT, DELETE (5 keys)

---

## Target Layer Designs

### Layer 0: Base (QWERTY + Home Row Mods)

```
┌─────┬─────┬─────┬─────┬─────┬─────┐                    ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ ESC │  1  │  2  │  3  │  4  │  5  │                    │  6  │  7  │  8  │  9  │  0  │ BSPC│
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ TAB │  Q  │  W  │  E  │  R  │  T  │                    │  Y  │  U  │  I  │  O  │  P  │  -  │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│  '  │ A/⌘ │ S/⌥ │ D/⇧ │ F/⌃ │  G  │                    │  H  │ J/⌃ │ K/⇧ │ L/⌥ │ ;/⌘ │  =  │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ⇧   │  Z  │  X  │  C  │  V  │  B  │                    │  N  │  M  │  ,  │  .  │  /  │  \  │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐        ┌─────┼─────┼─────┼─────┼─────┼─────┼─────┘
      │MUTE*│ ⌃   │ ⌘   │ ⌥   │ L1  │ SPC │        │ RET │ RET │ L2  │REPT │ ⇧   │ DEL │
      └─────┴─────┴─────┴─────┴─────┴─────┘        └─────┴─────┴─────┴─────┴─────┴─────┘
       52    53    54    55    56    57             58    59    60    61    62    63
       *hardwired to encoder, cannot change
```

**Key Features**:
- Home row mods: Left `A=⌘, S=⌥, D=⇧, F=⌃` | Right `J=⌃, K=⇧, L=⌥, ;=⌘`
- **Hyphen `-` and Equals `=`** directly on Layer 0 (right column) for easy access!
- **Single quote `'`** moved to left column for better ergonomics
- Repeat key on right thumb (position 61) - avoid ==, ++, //
- **Left thumb** (positions 53-57): 5 usable keys (position 52 is hardwired to MUTE)
- **Center** (position 58): 1 key accessible by both thumbs
- **Right thumb** (positions 59-63): 5 keys
- Encoder: Volume Up/Down

**Position changes from standard QWERTY**:
- Position 26: ~~CAPS~~ → `'` (single quote) - CAPS moved to Layer 2
- Position 25: ~~`\`~~ → `-` (hyphen/minus)
- Position 38: ~~`'`~~ → `=` (equals)
- Position 51: ~~ENTER~~ → `\` (backslash) - ENTER on thumbs (58, 59)

**Proposed thumb layout** (keeping current layout, only adding repeat key):
- 52: **C_MUTE** (hardwired to encoder, cannot be changed)
- 53: LCTRL (quick Ctrl access for terminal/tmux)
- 54: LGUI (⌘ Cmd key for macOS shortcuts)
- 55: LALT (⌥ Option/Alt key)
- 56: mo 1 (hold for Layer 1 - symbols/numbers)
- 57: SPACE (main space key)
- 58: ENTER (center key, main enter)
- 59: ENTER (duplicate on right half - can change to &trans)
- 60: mo 2 (hold for Layer 2 - navigation/function keys)
- 61: **key_repeat** ← CHANGED from RCTRL (repeat last key!)
- 62: RSHFT (right shift for convenience)
- 63: DELETE (backspace alternative)

---

### Layer 1: Symbols + Numbers (Ruby Optimized)

```
┌─────┬─────┬─────┬─────┬─────┬─────┐                    ┌─────┬─────┬─────┬─────┬─────┬─────┐
│  `  │  !  │  @  │  #  │  $  │  %  │                    │  ^  │  &  │  *  │  (  │  )  │ DEL │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│  ~  │  1  │  2  │  3  │  4  │  5  │                    │  6  │  7  │  8  │  9  │  0  │  |  │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │  =  │  -  │  +  │  *  │  <  │                    │  >  │  :  │  [  │  ]  │  "  │  '  │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │  _  │  /  │  &  │  |  │  ?  │                    │  !  │  {  │  }  │  (  │  )  │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐        ┌─────┼─────┼─────┼─────┼─────┼─────┼─────┘
      │MUTE │ ─── │ ─── │ ─── │ ─── │ ─── │        │ ─── │ ─── │ L3  │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘        └─────┴─────┴─────┴─────┴─────┴─────┘
```

**Ruby Bigrams (Inward Rolls)**:
- `<=`: < (pinky) → = (index) ✓
- `>=`: Similar pattern ✓
- `->`: - (middle) → > (right hand)
- `=>`: = (index) → > (right hand)
- `::`: : (index) + repeat key ✓
- `==`: = (Layer 0) + repeat key ✓
- `--`: - (Layer 0) + repeat key ✓
- `!=`: ! + = comfortable reach

**Key Features**:
- Numbers 1-0 on second row (easier than top row)
- `=` and `-` **also** on Layer 1 home row for symbol combos while holding layer
- `:` on right home index (Ruby hashes)
- Brackets/parens on right side
- Math operators on left home row

**Note**: `=` and `-` appear on **both Layer 0 and Layer 1** for convenience:
- **Layer 0**: Right column (positions 25, 38) - easy access for writing text
- **Layer 1**: Left home row (positions 27, 28) - for symbol combinations without releasing layer

---

### Layer 2: Navigation + Function Keys

```
┌─────┬─────┬─────┬─────┬─────┬─────┐                    ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ F11 │ F1  │ F2  │ F3  │ F4  │ F5  │                    │ F6  │ F7  │ F8  │ F9  │ F10 │ F12 │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │Tmux1│Tmux2│Tmux3│Tmux4│Tmux5│                    │ HOME│ PGDN│ PGUP│ END │ INS │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ CAPS│ ⌘   │  ⌥  │  ⇧  │  ⌃  │ ─── │                    │  ←  │  ↓  │  ↑  │  →  │ ─── │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ ⌘Z  │ ⌘X  │ ⌘C  │ ⌘V  │Tmux0│                    │ ⌃←  │ ⌃↓  │ ⌃↑  │ ⌃→  │ ─── │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐        ┌─────┼─────┼─────┼─────┼─────┼─────┼─────┘
      │MUTE │ ─── │ ─── │ L4  │ ─── │ ─── │        │ ─── │ ─── │ ─── │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘        └─────┴─────┴─────┴─────┴─────┴─────┘
```

**Key Features**:
- Function keys F1-F12 on top row
- Arrow keys on right home row
- Ctrl+Arrow for word jumping
- Tmux macros (⌃-A [0-5]) for window switching
- macOS shortcuts (⌘Z/X/C/V) on left bottom row
- Encoder: Tmux window switching (⌃-A n / ⌃-A p)

---

### Layer 3: System + Mouse

```
┌─────┬─────┬─────┬─────┬─────┬─────┐                    ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ ─── │ BT1 │ BT2 │ BT3 │ BT4 │ BT5 │                    │ RGB+│ RGB-│ EFF+│ EFF-│ TOG │BOOT │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │BTCLR│BTCLA│ ─── │ ─── │ ─── │                    │ M↑  │ MW← │ MW→ │ ─── │ ─── │RESET│
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ USB │ BLE │ ─── │ ─── │ ─── │                    │ M←  │ M↓  │ M↑  │ M→  │ ─── │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤                    ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ ─── │ ─── │ ─── │ ─── │ ─── │                    │ MW↓ │ MW↑ │ ─── │ ─── │ ─── │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐        ┌─────┼─────┼─────┼─────┼─────┼─────┼─────┘
      │MUTE │ ─── │ ─── │ ─── │ ─── │ LCLK│        │ RCLK│ MCLK│ ─── │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘        └─────┴─────┴─────┴─────┴─────┴─────┘
```

**Key Features**:
- Bluetooth profiles 1-5, clear functions
- USB/BLE output selection
- Mouse movement (M←/↓/↑/→) on right home area
- Mouse wheel (MW←/→/↓/↑) for scrolling
- Mouse clicks (L/R/M) on right thumb
- RGB controls (brightness, effects, toggle)
- System controls (bootloader, reset) in corners
- Encoder: Vertical scrolling

---

### Layer 4: Reserved/Future Macros

Currently empty - available for:
- Ruby code snippets (`def`, `end`, `do |x|`)
- IDE shortcuts
- Additional custom macros

---

## Implementation Tasks

### Phase 1: Foundation (Tasks 1-3)

#### Task 1: Add Key Repeat Behavior
**File**: `config/eyelash_sofle.keymap`

**Changes**:
```c
/ {
    behaviors {
        key_repeat: key_repeat {
            compatible = "zmk,behavior-key-repeat";
            #binding-cells = <0>;
        };
    };
};
```

**Update Layer 0, Row 5** (thumb row positions 52-63):

Current row 5:
```
&kp C_MUTE  &kp LCTRL  &kp LEFT_GUI  &kp LEFT_ALT  &mo 1  &kp SPACE  &kp ENTER  &kp ENTER  &mo 2  &kp RCTRL  &kp RIGHT_SHIFT  &kp DELETE
```

Change to (update position 61):
```
&kp C_MUTE  &kp LCTRL  &kp LEFT_GUI  &kp LEFT_ALT  &mo 1  &kp SPACE  &kp ENTER  &kp ENTER  &mo 2  &key_repeat  &kp RIGHT_SHIFT  &kp DELETE
```

**What changed**:
- Position 61: `&kp RCTRL` → `&key_repeat`
- Note: RCTRL is removed (you already have Ctrl on home row via HRM)

**Testing**:
- Build and flash firmware
- Press `=` once, then press the repeat key → should type `==`
- Press `:` once, then press the repeat key → should type `::`
- Type `for` then press repeat → should type another `r`
- Verify the repeat key works for any character

**Rollback**: Change position 61 back to `&kp RCTRL`

---

#### Task 2: Add Home Row Mod - Single Key Test
**File**: `config/eyelash_sofle.keymap`

**Changes**:
```c
/ {
    behaviors {
        hrm: homerow_mods {
            compatible = "zmk,behavior-hold-tap";
            #binding-cells = <2>;
            flavor = "balanced";
            tapping-term-ms = <200>;
            quick-tap-ms = <125>;
            require-prior-idle-ms = <125>;
            bindings = <&kp>, <&kp>;
        };
    };
};
```

**Update Layer 0** (position 30 - F key):
- Change from: `&kp F`
- Change to: `&hrm LCTRL F`

**Testing**:
- Tap F quickly → should type 'f'
- Hold F for 200ms → should act as Ctrl
- Try Ctrl-C (hold F, tap C)
- Type "ффф" quickly to test quick-tap
- Type "for" to ensure normal typing works

**Rollback**: Revert position 30 to `&kp F`

---

#### Task 3: Complete Left Hand Home Row Mods
**File**: `config/eyelash_sofle.keymap`

**Update Layer 0** (positions 27-30):
- Position 27 (A): `&hrm LGUI A`
- Position 28 (S): `&hrm LALT S`
- Position 29 (D): `&hrm LSHFT D`
- Position 30 (F): `&hrm LCTRL F` (already done)

**Testing**:
- Test each modifier independently
- Try combos: ⌘-C, ⌘-V, ⌘-Tab, ⌃-A
- Type common words: "asdf", "fast", "sad", "add"
- Verify no false triggers during normal typing

**Rollback**: Revert positions 27-29 to plain `&kp` versions

---

### Phase 2: Complete Home Row Mods (Tasks 4-5)

#### Task 4: Complete Right Hand Home Row Mods
**File**: `config/eyelash_sofle.keymap`

**Update Layer 0** (positions 33-37):
- Position 33 (H): Keep `&kp H` (no mod)
- Position 34 (J): `&hrm RCTRL J`
- Position 35 (K): `&hrm RSHFT K`
- Position 36 (L): `&hrm RALT L`
- Position 37 (;): `&hrm RGUI SEMI`

**Testing**:
- Test right hand modifiers
- Try vim navigation with Ctrl (⌃-F, ⌃-B, ⌃-D, ⌃-U)
- Type common words: "jkl;", "jump", "kill", "look"
- Test modifier combos from both hands

**Rollback**: Revert positions 34-37 to plain `&kp` versions

---

#### Task 5: Remove Position 26 (Old CAPS)
**File**: `config/eyelash_sofle.keymap`

**Update Layer 0** (position 26):
- Change from: `&kp CAPS`
- Change to: `&none` or `&trans`

**Testing**:
- Verify CAPS moved to Layer 2 (position 26)
- Confirm no accidental CAPS activation

**Rollback**: Restore `&kp CAPS` if needed

---

### Phase 3: Symbol Layer - Left Hand (Tasks 6-7)

#### Task 6: Symbol Layer - Left Top/Number Row
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 0-5, 13-18):
```
Row 1 (0-5):   `    !    @    #    $    %
Row 2 (13-18): ~    1    2    3    4    5
```

**Testing**:
- Hold Layer 1, type each symbol
- Verify backtick, tilde work
- Test numbers 1-5
- Test symbols !, @, #, $, %

**Rollback**: Restore original Layer 1 values

---

#### Task 7: Symbol Layer - Left Home/Bottom Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 26-31, 39-44):
```
Home (26-31):   trans  =  -  +  *  <
Bottom (39-44): trans  _  /  &  |  ?
```

**Testing**:
- Test `=` for typing `==` (with repeat)
- Test `-`, `+` for math
- Test `<`, `>` combo
- Test `&`, `|` for && and ||
- Try Ruby bigrams: `<=`, `>=`, `!=`

**Rollback**: Restore original Layer 1 values

---

### Phase 4: Symbol Layer - Right Hand (Tasks 8-9)

#### Task 8: Symbol Layer - Right Top/Number Row
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 7-12, 20-25):
```
Row 1 (7-12):  ^    &    *    (    )    DEL
Row 2 (20-25): 6    7    8    9    0    |
```

**Testing**:
- Test numbers 6-0
- Test ^, &, *, (, )
- Test pipe |
- Verify DEL works

**Rollback**: Restore original Layer 1 values

---

#### Task 9: Symbol Layer - Right Home/Bottom Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 33-38, 46-51):
```
Home (33-38):   >  :  [  ]  "  '
Bottom (46-51): !  {  }  (  )  trans
```

**Testing**:
- Test `:` for Ruby hashes (`key: value`)
- Test brackets: `[]`, `{}`
- Test quotes: `"`, `'`
- Try bigrams: `=>`, `->`, `::`
- Test parentheses

**Rollback**: Restore original Layer 1 values

---

### Phase 5: Navigation Layer (Tasks 10-12)

#### Task 10: Navigation - Function Keys
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 0-12):
```
Row 1: F11  F1  F2  F3  F4  F5  (center)  F6  F7  F8  F9  F10  F12
```

**Testing**:
- Test each F-key (F1-F12)
- Try in IDE (F5 for debug, etc.)
- Verify F11, F12 work

**Rollback**: Restore original Layer 2 row 1

---

#### Task 11: Navigation - Arrow Keys & Page Navigation
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 20-25, 33-38):
```
Row 2 (20-25): HOME PGDN PGUP END INS trans
Row 3 (33-38): LEFT DOWN UP   RIGHT trans trans
```

**Testing**:
- Test arrow keys in editor
- Test Page Up/Down for scrolling
- Test Home/End for line navigation
- Verify Insert key works

**Rollback**: Restore original Layer 2 values

---

#### Task 12: Navigation - Ctrl+Arrow & macOS Shortcuts
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 46-50):
```
Bottom row (46-50): LC(LEFT) LC(DOWN) LC(UP) LC(RIGHT) trans
```

**Update Layer 2** (positions 40-44):
```
Bottom row (40-44): LG(Z) LG(X) LG(C) LG(V) trans
```

**Testing**:
- Test Ctrl+Arrow for word jumping
- Test ⌘Z (undo), ⌘X (cut), ⌘C (copy), ⌘V (paste)
- Verify in text editor and terminal

**Rollback**: Restore original Layer 2 values

---

### Phase 6: Tmux Integration (Tasks 13-14)

#### Task 13: Add Tmux Macro Behavior
**File**: `config/eyelash_sofle.keymap`

**Changes**:
```c
/ {
    macros {
        tmux_0: tmux_0 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LC(A) &kp N0>;
        };
        tmux_1: tmux_1 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LC(A) &kp N1>;
        };
        // ... repeat for tmux_2 through tmux_5
    };
};
```

**Testing**:
- Test macro compiles
- Don't test functionality yet

**Rollback**: Remove macro definitions

---

#### Task 14: Add Tmux Macros to Layer 2
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 14-18, 44):
```
Row 2 (14-18): &tmux_1 &tmux_2 &tmux_3 &tmux_4 &tmux_5
Row 4 (44):    &tmux_0
```

**Testing**:
- Open tmux session with multiple windows
- Test each tmux macro (0-5) switches to correct window
- Verify Ctrl-A prefix works correctly

**Rollback**: Restore original Layer 2 values

---

### Phase 7: System & Mouse Layer (Tasks 15-17)

#### Task 15: System Layer - Bluetooth & Output
**File**: `config/eyelash_sofle.keymap`

**Update Layer 3** (positions 1-5, 14-15, 27-28):
```
Row 1 (1-5):   BT_SEL 0  BT_SEL 1  BT_SEL 2  BT_SEL 3  BT_SEL 4
Row 2 (14-15): BT_CLR    BT_CLR_ALL
Row 3 (27-28): OUT_USB   OUT_BLE
```

**Testing**:
- Test BT profile switching
- Test BT clear functions
- Test USB/BLE output toggle
- Verify connections work

**Rollback**: Keep original Layer 2 BT functions if needed

---

#### Task 16: System Layer - Mouse Movement & Clicks
**File**: `config/eyelash_sofle.keymap`

**Update Layer 3** (positions 20, 33-36, 46-47, 58-60):
```
Row 2 (20):       M↑ (mouse up)
Row 3 (33-36):    M← M↓ M↑ M→
Row 4 (46-47):    MW↓ MW↑ (mouse wheel)
Thumb (58-60):    LCLK RCLK MCLK
```

**Testing**:
- Test mouse movement in all directions
- Test mouse clicks (left/right/middle)
- Test mouse wheel scrolling
- Verify smooth movement

**Rollback**: Restore original mouse positions

---

#### Task 17: System Layer - RGB & System Controls
**File**: `config/eyelash_sofle.keymap`

**Update Layer 3** (positions 7-12, 25):
```
Row 1 (7-12): RGB_BRI RGB_BRD RGB_EFF RGB_EFR RGB_TOG BOOTLOADER
Row 2 (25):   SYS_RESET
```

**Testing**:
- Test RGB brightness up/down
- Test RGB effects
- Test RGB toggle
- **CAREFUL**: Test bootloader (puts keyboard in flash mode)
- Test system reset

**Rollback**: Keep original RGB positions if needed

---

### Phase 8: Encoder Configuration (Tasks 18-19)

#### Task 18: Configure Encoder for Layer 0 & 2
**File**: `config/eyelash_sofle.keymap`

**Update encoder bindings**:
```
Layer 0: C_VOL_UP / C_VOL_DN
Layer 2: &tmux_next / &tmux_prev (needs macros for ⌃-A n / ⌃-A p)
```

**Testing**:
- Test volume control on Layer 0
- Test tmux window switching on Layer 2
- Verify smooth rotation

**Rollback**: Restore original encoder configs

---

#### Task 19: Configure Encoder for Layer 3
**File**: `config/eyelash_sofle.keymap`

**Update encoder bindings**:
```
Layer 3: MW_UP / MW_DOWN (mouse wheel)
```

**Testing**:
- Test mouse wheel scrolling in browser
- Test in editor for page scrolling
- Verify direction is correct

**Rollback**: Restore original encoder configs

---

### Phase 9: Polish & Optimization (Tasks 20-21)

#### Task 20: Fine-tune Home Row Mod Timings
**File**: `config/eyelash_sofle.keymap`

**Adjust `hrm` behavior**:
```c
tapping-term-ms = <200>;      // Adjust between 175-250
quick-tap-ms = <125>;          // Adjust between 100-150
require-prior-idle-ms = <125>; // Adjust between 100-150
```

**Testing**:
- Type fast: "asdf", "jkl;", "the quick brown fox"
- Hold for modifiers: ⌃-C, ⌘-V, ⌘-Tab
- Find sweet spot between responsiveness and false triggers
- Test common words from your Ruby code

**Rollback**: Restore original timings

---

#### Task 21: Update Layer 4 (Optional Macros)
**File**: `config/eyelash_sofle.keymap`

**Add Ruby snippet macros** (optional):
```c
ruby_def: macro for "def <cursor> end"
ruby_do: macro for "do |x| <cursor> end"
ruby_puts: macro for "puts "
```

**Testing**:
- Test each macro in editor
- Verify cursor positioning (if possible)
- Test in actual Ruby files

**Rollback**: Remove macro layer if not useful

---

## Testing Checklist

After each task, verify:
- [ ] Firmware builds without errors
- [ ] Keyboard connects (USB/BT)
- [ ] Modified keys work as expected
- [ ] No regression in other keys/layers
- [ ] Typing feels natural (no false triggers)
- [ ] Can still perform daily work effectively

---

## Complete Testing Script

After all tasks, test complete workflow:

### Ruby Development Test
```ruby
# Test symbol access
def hello(name)
  puts "Hello, #{name}!"
  result = { name: name, greeting: "Hello!" }

  # Test comparisons and operators
  if name.length >= 5
    result[:long_name] = true
  end

  # Test arrows and blocks
  [1, 2, 3].map { |x| x * 2 }
    .select { |x| x != 4 }
    .each do |x|
      puts x
    end

  result
end
```

**Test while typing**:
- Home row mods don't false trigger
- `==`, `!=`, `<=`, `>=` comfortable to type
- `=>`, `->`, `::` feel like inward rolls
- Brackets/parens easy to access
- Repeat key works for `==`, `::`

### Vim Navigation Test
- Test hjkl (normal letters, but also ⌃-F, ⌃-B, ⌃-D, ⌃-U with HRM)
- Test Layer 2 arrow keys
- Test Ctrl+Arrow for word jumping
- Test Page Up/Down for scrolling
- Test ⌘Z, ⌘C, ⌘V in editor

### Tmux Workflow Test
- Test ⌃-A prefix (from thumb or HRM)
- Test tmux window switching (macros and encoder)
- Test creating/closing windows
- Test pane navigation

### Chrome/macOS Test
- Test ⌘T (new tab), ⌘W (close tab)
- Test ⌘L (address bar)
- Test ⌘Tab (app switching)
- Test volume control (encoder)
- Test mouse movement/clicks (Layer 3)

---

## Rollback Instructions

### Complete Rollback
If something breaks badly:
```bash
git checkout config/eyelash_sofle.keymap
git checkout config/eyelash_sofle.conf
west build -p -d build/left -b eyelash_sofle_left
west flash -d build/left
```

### Partial Rollback
Use git to restore specific sections:
```bash
git diff config/eyelash_sofle.keymap  # See what changed
git checkout config/eyelash_sofle.keymap -- <specific-section>
```

### Emergency: Flash Original Firmware
If keyboard becomes unusable:
1. Download artifacts from GitHub Actions (last working build)
2. Put keyboard in bootloader mode (double-tap reset)
3. Copy `.uf2` file to mounted drive
4. Wait for keyboard to reboot

---

## Configuration Notes

### Home Row Mod Tuning

**If false triggers occur** (modifiers activate when typing):
- Increase `tapping-term-ms` (try 220-250)
- Increase `require-prior-idle-ms` (try 150)

**If mods feel sluggish**:
- Decrease `tapping-term-ms` (try 175-190)
- Decrease `require-prior-idle-ms` (try 100-115)

**If repeated letters (like 'ff') trigger mod**:
- Adjust `quick-tap-ms` (try 100-150)

### Symbol Layer Optimization

Track which symbols you actually use and adjust positions:
- Most used → home row
- Medium → top/bottom rows
- Rare → corners or Layer 4

### Encoder Preferences

Choose per-layer based on workflow:
- Layer 0: Volume (general use)
- Layer 1: RGB effects (if you use RGB)
- Layer 2: Tmux windows (development)
- Layer 3: Mouse wheel (browsing)

---

## Next Steps

1. Start with **Task 1** (Repeat Key)
2. Test thoroughly before moving to next task
3. Take notes on what feels good/bad
4. Adjust timings based on your typing style
5. Don't hesitate to modify the plan based on experience

**Remember**: This is YOUR keyboard. Adjust anything that doesn't feel right!

---

## Build Commands Reference

```bash
# Build left half
west build -d build/left -b eyelash_sofle_left -- -DZMK_CONFIG="$(pwd)/config"

# Build right half
west build -d build/right -b eyelash_sofle_right -- -DZMK_CONFIG="$(pwd)/config"

# Flash (keyboard in bootloader mode)
west flash -d build/left

# Or copy manually
cp build/left/zephyr/zmk.uf2 /Volumes/NICENANO/

# Clean build (if needed)
west build -d build/left -b eyelash_sofle_left --pristine
```

## Resources

- [ZMK Behaviors Documentation](https://zmk.dev/docs/behaviors)
- [Home Row Mods Guide](https://precondition.github.io/home-row-mods)
- [ZMK Discord](https://discord.gg/zmk) - Great for troubleshooting
