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
┌─────┬─────┬─────┬─────┬─────┬─────┐       ┌─────┐       ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ ESC │  1  │  2  │  3  │  4  │  5  │       │  ↑  │       │  6  │  7  │  8  │  9  │  0  │ BSPC│
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ TAB │  Q  │  W  │  E  │  R  │  T  │       │  ↓  │       │  Y  │  U  │  I  │  O  │  P  │  -  │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│  '  │ A/⌘ │ S/⌥ │ D/⇧ │ F/⌃ │  G  │       │  ←  │       │  H  │ J/⌃ │ K/⇧ │ L/⌥ │ ;/⌘ │  =  │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ⇧   │  Z  │  X  │  C  │  V  │  B  │       │  →  │       │  N  │  M  │  ,  │  .  │  /  │  \  │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐ ├─────┤ ┌─────┼─────┼─────┼─────┼─────┼─────┴─────┘
      │none*│ ⌃   │ ⌘   │⌃-A**│ L1  │ SPC │ │ RET │ │ RET │ L2  │REPT │ ⇧   │ DEL │
      └─────┴─────┴─────┴─────┴─────┴─────┘ └─────┘ └─────┴─────┴─────┴─────┴─────┘
       52    53    54    55    56    57      58      59    60    61    62    63
       *hardwired to encoder, set to &none
       **⌃-A = tmux prefix macro
```

**Key Features**:
- Home row mods: Left `A=⌘, S=⌥, D=⇧, F=⌃` | Right `J=⌃, K=⇧, L=⌥, ;=⌘`
- **Hyphen `-` and Equals `=`** directly on Layer 0 (right column) for easy access!
- **Single quote `'`** moved to left column for better ergonomics
- **Tmux prefix (⌃-A) on left thumb (position 55)** - one-key access to all tmux commands!
- **Repeat key on right thumb (position 61)** - avoid double-tapping for ==, ::, ||, etc.
- **Duplicate ENTER keys** (positions 58, 59) - accessible from both thumbs
- **Left thumb** (positions 53-57): 5 usable keys (position 52 is encoder, set to &none)
- **Center** (position 58): 1 key accessible by both thumbs
- **Right thumb** (positions 59-63): 5 keys (ENTER, L2, Repeat, Shift, Delete)
- **Center column** (positions 6, 19, 32, 45): Arrow keys (↑, ↓, ←, →) preserved
- Encoder: Volume Up/Down

**Position changes from standard QWERTY**:
- Position 26: ~~CAPS~~ → `'` (single quote) - CAPS moved to Layer 2
- Position 25: ~~`\`~~ → `-` (hyphen/minus)
- Position 38: ~~`'`~~ → `=` (equals)
- Position 51: ~~ENTER~~ → `\` (backslash) - ENTER on thumbs (58, 59)

**Actual thumb layout** (as implemented):
- 52: **&none** (encoder hardwired position)
- 53: LCTRL (quick Ctrl access for terminal/tmux)
- 54: LGUI (⌘ Cmd key for macOS shortcuts)
- 55: **TMUX_PREFIX** ← NEW! Sends ⌃-A for tmux commands
- 56: mo 1 (hold for Layer 1 - symbols/numbers)
- 57: SPACE (main space key)
- 58: ENTER (center key, accessible by both thumbs)
- 59: ENTER (duplicate for convenience - right thumb)
- 60: mo 2 (hold for Layer 2 - navigation)
- 61: **key_repeat** ← NEW! Repeat last key for ==, ::, etc.
- 62: RSHFT (right shift for convenience)
- 63: DELETE (backspace alternative)

---

### Layer 1: Symbols (Ruby Optimized)

```
┌─────┬─────┬─────┬─────┬─────┬─────┐       ┌─────┐       ┌─────┬─────┬─────┬─────┬─────┬─────┐
│  `  │  ~  │  @  │  #  │  $  │  ^  │       │ ─── │       │ @@  │  (  │  )  │  {  │  }  │ DEL │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│  _  │  !  │  ?  │  *  │  /  │  %  │       │ ─── │       │  |  │  &  │  "  │  '  │  \  │  |  │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │  -  │  +  │  <  │  =  │  >  │       │ ─── │       │  :  │  [  │  ]  │  (  │  )  │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │  _  │  *  │  /  │  %  │  &  │       │ ─── │       │  |  │  {  │  }  │  [  │  ]  │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐ ├─────┤ ┌─────┼─────┼─────┼─────┼─────┼─────┴─────┘
      │MUTE │ ─── │ ─── │ ─── │ ─── │ ─── │ │ ─── │ │ ─── │ L3  │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘ └─────┘ └─────┴─────┴─────┴─────┴─────┘
```

**Ruby Bigrams (Optimized as Inward Rolls)**:

Left hand home row enables comfortable inward rolls:
- `<=`: **< (middle, 29) → = (INDEX, 30)** ✓ Inward roll
- `>=`: **> (INDEX, 31) → = (INDEX, 30)** ⚠️ Same finger (adjacent keys, very natural)
- `->`: **- (pinky, 27) → > (INDEX, 31)** ✓ Full inward roll
- `+=`: **+ (ring, 28) → = (INDEX, 30)** ✓ Inward roll
- `-=`: **- (pinky, 27) → = (INDEX, 30)** ✓ Full inward roll
- `*=`: **\* (row 2/4) → = (INDEX, 30)** ✓ Available
- `/=`: **/ (row 2/4) → = (INDEX, 30)** ✓ Available
- `=>`: **= (INDEX, 30) → > (INDEX, 31)** ⚠️ Same finger (adjacent keys, very natural)

Right hand home row for delimiters:
- `::`: **: (INDEX, 33) + repeat** ✓ Symbol syntax
- `[]`: **[ (INDEX, 34) ] (middle, 35)** ✓ Adjacent keys

With repeat key:
- `==`: **= + repeat** ✓
- `::`: **: + repeat** ✓ (colon on right index)
- `||`: **| + repeat** ✓ (pipe on right side)
- `&&`: **& + repeat** ✓ (ampersand on right side)

Other common Ruby patterns:
- `!=`: **! (row 2) → = (home)** - comfortable reach
- `[]`: **[ ] next to each other** (positions 34-35) ✓
- `{}`: **{ } grouped** (positions 10-11, 47-48) ✓
- `()`: **( ) grouped** (positions 8-9, 36-37, 49-50) ✓
- `|x|`: **| easy access** on right side ✓

**Key Features**:
- **No numbers** - already on Layer 0 without modifiers
- **Left home row optimized for operators** (pinky → INDEX):
  - `< + > = -` (positions 27-31)
  - Most common `=` on strongest finger (INDEX, position 30)
  - Enables 5+ inward rolls for Ruby bigrams
- **Right home row for delimiters** (INDEX → pinky):
  - `: [ ] ( )` (positions 33-37)
  - Colon `:` on INDEX for hash syntax
  - Brackets `[]` adjacent for easy array access
- **Symbols grouped by type**:
  - Row 1: Special chars (`, ~, @, #, $, ^, @@)
  - Row 2: Logic/string (_, !, ?, *, /, %, |, &, ", ', \)
  - Row 3 (HOME): Operators & delimiters optimized by finger strength
  - Row 4: Duplicate brackets for convenience ({ }, [], |)
- **Pinkies avoid double-taps** - use repeat key instead!
- **Block parameters** `|x|` very accessible with pipe on right side

**Finger assignments on home row**:
- Left: pinky `<`, ring `+`, middle `>`, INDEX `=`, INDEX `-`
- Right: INDEX `:`, INDEX `[`, middle `]`, ring `(`, pinky `)`

---

### Layer 2: Navigation + Function Keys

```
┌─────┬─────┬─────┬─────┬─────┬─────┐       ┌─────┐       ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ F11 │ F1  │ F2  │ F3  │ F4  │ F5  │       │ ─── │       │ F6  │ F7  │ F8  │ F9  │ F10 │ F12 │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │Tmux1│Tmux2│Tmux3│Tmux4│Tmux5│       │ ─── │       │ HOME│ PGDN│ PGUP│ END │ INS │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ CAPS│ ⌘   │  ⌥  │  ⇧  │  ⌃  │ ─── │       │ ─── │       │  ←  │  ↓  │  ↑  │  →  │ ─── │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ ⌘Z  │ ⌘X  │ ⌘C  │ ⌘V  │Tmux0│       │ ─── │       │ ⌃←  │ ⌃↓  │ ⌃↑  │ ⌃→  │ ─── │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐ ├─────┤ ┌─────┼─────┼─────┼─────┼─────┼─────┴─────┘
      │MUTE │ ─── │ ─── │ L4  │ ─── │ ─── │ │ ─── │ │ ─── │ ─── │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘ └─────┘ └─────┴─────┴─────┴─────┴─────┘
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
┌─────┬─────┬─────┬─────┬─────┬─────┐       ┌─────┐       ┌─────┬─────┬─────┬─────┬─────┬─────┐
│ ─── │ BT1 │ BT2 │ BT3 │ BT4 │ BT5 │       │ ─── │       │ RGB+│ RGB-│ EFF+│ EFF-│ TOG │BOOT │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │BTCLR│BTCLA│ ─── │ ─── │ ─── │       │ ─── │       │ M↑  │ MW← │ MW→ │ ─── │ ─── │RESET│
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ USB │ BLE │ ─── │ ─── │ ─── │       │ ─── │       │ M←  │ M↓  │ M↑  │ M→  │ ─── │ ─── │
├─────┼─────┼─────┼─────┼─────┼─────┤       ├─────┤       ├─────┼─────┼─────┼─────┼─────┼─────┤
│ ─── │ ─── │ ─── │ ─── │ ─── │ ─── │       │ ─── │       │ MW↓ │ MW↑ │ ─── │ ─── │ ─── │ ─── │
└─────┼─────┼─────┼─────┼─────┼─────┼─────┐ ├─────┤ ┌─────┼─────┼─────┼─────┼─────┼─────┴─────┘
      │MUTE │ ─── │ ─── │ ─── │ ─── │ LCLK│ │ RCLK│ │ MCLK│ ─── │ ─── │ ─── │ ─── │
      └─────┴─────┴─────┴─────┴─────┴─────┘ └─────┘ └─────┴─────┴─────┴─────┴─────┘
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

#### Task 1: Add Key Repeat & Tmux Prefix Behaviors ✅ COMPLETED
**File**: `config/eyelash_sofle.keymap`

**Changes**:
```c
/ {
    macros {
        tmux_prefix: tmux_prefix {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&macro_press &kp LCTRL>, <&macro_tap &kp A>, <&macro_release &kp LCTRL>;
        };
    };

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

Changed to:
```
&none  &kp LCTRL  &kp LEFT_GUI  &tmux_prefix  &mo 1  &kp SPACE  &kp ENTER  &kp ENTER  &mo 2  &key_repeat  &kp RIGHT_SHIFT  &kp DELETE
```

**What changed**:
- Position 52: `&kp C_MUTE` → `&none` (encoder hardwired position)
- Position 55: `&kp LEFT_ALT` → `&tmux_prefix` (⌃-A macro for tmux!)
- Position 61: `&kp RCTRL` → `&key_repeat` (repeat key for ==, ::, ||, etc.)
- Note: Kept duplicate ENTER at positions 58 and 59 for convenience

**Testing**:
- Build and flash firmware
- **Repeat key tests**:
  - Press `=` once, then press the repeat key → should type `==`
  - Press `:` once, then press the repeat key → should type `::`
  - Type `for` then press repeat → should type another `r`
  - Verify the repeat key works for any character
- **Tmux prefix tests**:
  - In tmux session, press tmux prefix key then `c` → should create new window
  - Press tmux prefix then `n` → should switch to next window
  - Press tmux prefix then `0` → should switch to window 0
  - Verify it works like Ctrl-A in tmux

**Rollback**: Restore positions 55, 59, 60, 61 to original values

---

#### Task 2: Add Home Row Mod - Single Key Test ✅ COMPLETED
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

#### Task 3: Complete Left Hand Home Row Mods ✅ COMPLETED
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

#### Task 4: Complete Right Hand Home Row Mods ✅ COMPLETED
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

#### Task 5: Update Layer 0 Symbol Positions ✅ COMPLETED
**File**: `config/eyelash_sofle.keymap`

**Update Layer 0** (positions 25, 26, 38, 51):
- Position 25: `&kp BSLH` → `&kp MINUS` (hyphen `-`)
- Position 26: `&kp CAPS` → `&kp APOS` (single quote `'`)
- Position 38: `&kp APOS` → `&kp EQUAL` (equals `=`)
- Position 51: `&kp ENTER` → `&kp BSLH` (backslash `\`)

**Testing**:
- Test typing `-` and `=` for text (hyphens, math)
- Test `'` for contractions, string quotes
- Test `\` for escape sequences
- Verify ENTER still accessible on thumbs (positions 58, 59)

**Rollback**: Restore original positions if needed

---

### Phase 3: Symbol Layer - Left Hand (Tasks 6-7)

#### Task 6: Symbol Layer - Left Top Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 0-5, 13-18):
```
Row 1 (0-5):   GRAVE  TILDE  AT     HASH   DLLR   CARET
Row 2 (13-18): UNDER  EXCL   QMARK  ASTRK  FSLH   PRCNT
```
Note: Keep position 6 (center) and 19 (center) unchanged (preserve arrow keys)

**Testing**:
- Hold Layer 1, type each symbol
- Test ` (backtick), ~ (tilde)
- Test @, #, $, ^ for Ruby symbols
- Test _, !, ?, *, /, % for operators

**Rollback**: Restore original Layer 1 values

---

#### Task 7: Symbol Layer - Left Home/Bottom Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 26-31, 39-44):
```
Home (26-31):   &trans  MINUS  PLUS  LT  EQUAL  GT
Bottom (39-44): &trans  UNDER  ASTRK  FSLH  PRCNT  AMPS
```
Note: Keep positions 32 and 45 (center) unchanged (preserve arrow keys)

**Testing**:
- Test `=` for typing `==` (with repeat)
- Test `-`, `+` for math and operators
- Test Ruby bigrams as inward rolls:
  - `<=`: < (middle) → = (INDEX) ✓
  - `>=`: > (INDEX) → = (INDEX) ✓
  - `->`: - (pinky) → > (INDEX) ✓
  - `+=`: + (ring) → = (INDEX) ✓
  - `-=`: - (pinky) → = (INDEX) ✓
- Test `&`, `%`, `*`, `/` operators

**Rollback**: Restore original Layer 1 values

---

### Phase 4: Symbol Layer - Right Hand (Tasks 8-9)

#### Task 8: Symbol Layer - Right Top Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 7-12, 20-25):
```
Row 1 (7-12):  AT  AT  LPAR  RPAR  LBRC  RBRC  DEL
Row 2 (20-25): PIPE  AMPS  DQT  SQT  BSLH  PIPE
```
Note: Position 7 is @@ (double at for Ruby instance variables)
Note: Keep positions 6, 19 (center) unchanged (preserve arrow keys)

**Testing**:
- Test @@ for Ruby instance variables (`@@variable`)
- Test (), {}, [] for delimiters
- Test |, &, ", ', \ for Ruby syntax
- Verify DEL works (position 12)

**Rollback**: Restore original Layer 1 values

---

#### Task 9: Symbol Layer - Right Home/Bottom Rows
**File**: `config/eyelash_sofle.keymap`

**Update Layer 1** (positions 33-38, 46-51):
```
Home (33-38):   COLON  LBKT  RBKT  LPAR  RPAR  &trans
Bottom (46-51): PIPE  LBRC  RBRC  LBKT  RBKT  &trans
```
Note: Keep positions 32, 45, 58 (center) unchanged (preserve arrow keys + ENTER)

**Testing**:
- Test `:` for Ruby hashes (`key: value`)
- Test `::` (double colon) using `:` + repeat key
- Test brackets: `[]` adjacent on home row
- Test `{}`, `()` for blocks and grouping
- Try Ruby bigrams:
  - `=>`: = (left INDEX) → > (left INDEX)
  - `->`: - (left pinky) → > (left INDEX)
  - `::`: : (right INDEX) + repeat key

**Rollback**: Restore original Layer 1 values

---

### Phase 5: Navigation Layer (Tasks 10-12)

#### Task 10: Navigation - Function Keys
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 0-5, 7-12):
```
Row 1 left (0-5):   F11  F1  F2  F3  F4  F5
Row 1 right (7-12): F6  F7  F8  F9  F10  F12
```
Note: Keep position 6 (center UP_ARROW) unchanged

**Testing**:
- Test each F-key (F1-F12)
- Try in IDE (F5 for debug, etc.)
- Verify F11, F12 work
- Confirm center arrow key still works

**Rollback**: Restore original Layer 2 row 1

---

#### Task 11: Navigation - Arrow Keys & Page Navigation
**File**: `config/eyelash_sofle.keymap`

**Update Layer 2** (positions 20-25, 33-38):
```
Row 2 left (14-18): &tmux_1 &tmux_2 &tmux_3 &tmux_4 &tmux_5
Row 2 right (20-25): HOME PGDN PGUP END INS &trans
Row 3 right (33-38): LEFT DOWN UP RIGHT &trans &trans
```
Note: Keep positions 19, 32, 45, 58 (center column) unchanged

**Testing**:
- Test arrow keys in editor
- Test Page Up/Down for scrolling
- Test Home/End for line navigation
- Verify Insert key works
- Center arrow keys should still work

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

#### Task 13: Add Tmux Window Switching Macros
**File**: `config/eyelash_sofle.keymap`

**Note**: This complements the `tmux_prefix` macro from Task 1. While `tmux_prefix` lets you do ANY tmux command (prefix + key), these macros provide one-key shortcuts to switch to specific windows.

**Changes** (add to macros section after tmux_prefix):
```c
/ {
    macros {
        // tmux_prefix already added in Task 1

        tmux_0: tmux_0 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N0>;
        };
        tmux_1: tmux_1 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N1>;
        };
        tmux_2: tmux_2 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N2>;
        };
        tmux_3: tmux_3 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N3>;
        };
        tmux_4: tmux_4 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N4>;
        };
        tmux_5: tmux_5 {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings = <&kp LCTRL &kp A &kp N5>;
        };
    };
};
```

**Testing**:
- Firmware compiles successfully
- Don't test functionality yet (wait for Task 14)

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
