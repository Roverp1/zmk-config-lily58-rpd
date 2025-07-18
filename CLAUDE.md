# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration for a Lily58 split keyboard with custom "RPD" (Real Programmer's Dvorak) layouts. The configuration includes sophisticated home row mods, mouse emulation, and multiple keyboard layouts optimized for both typing and gaming.

## Architecture

### Key Components

**Main Keymap** (`config/lily58.keymap`): 8-layer configuration with QWERTY, RPD (Real Programmer's Dvorak), gaming variants, and function layers. Uses advanced ZMK features including timeless home row mods and mod morphs.

**Custom Behaviors** (`config/behaviors/`):

- `timeless-hrm.dtsi`: Home row modifier timing configurations for different modifier types
- `rpd-mod-morphs.dtsi`: Dynamic key transformations (e.g., shift+numbers produce programming symbols)

**Hardware Configuration** (`config/lily58.conf`): Bluetooth settings, mouse emulation, and ZMK Studio integration.

**Dependencies** (`config/west.yml`): ZMK core firmware and zmk-helpers library.

### Layer Architecture

- **Layers 0-1**: QWERTY (normal and gaming)
- **Layers 2-3**: RPD/Dvorak (normal and gaming)
- **Layer 4**: LOWER - Function keys and symbols
- **Layer 5**: RAISE - Navigation, mouse control, bluetooth
- **Layer 6**: LowerLower - Reserved
- **Layer 7**: RaiseRaise - Bluetooth management

## Common Development Commands

### Building Firmware

The repository uses GitHub Actions for automatic builds. Push changes to trigger builds that generate firmware binaries for both keyboard halves.

### Testing Changes

Use ZMK Studio for real-time keymap editing and testing without flashing firmware. The configuration includes `studio-rpc-usb-uart` snippet for Studio compatibility.

### Resolving Merge Conflicts

The repository currently has a merge conflict in `config/lily58.keymap` that needs resolution before further development.

## Development Notes

**Home Row Mods**: Uses "timeless" home row mods with sophisticated timing configurations. Different modifiers have different hold-tap timings optimized for typing flow.

**Gaming Modes**: Gaming layers disable home row mods to prevent accidental modifier activation during gaming.

**Mouse Integration**: Built-in mouse movement and scrolling controls in the RAISE layer.

**Bluetooth Management**: Multi-device pairing and switching controls in RaiseRaise layer.

**Mod Morphs**: Number row transforms into programming symbols when shifted, following RPD layout conventions.

## File Organization

- `config/lily58.keymap` - Main keymap definition
- `config/behaviors/` - Custom ZMK behaviors
- `config/lily58.conf` - Hardware configuration
- `build.yaml` - GitHub Actions build matrix
- `config/west.yml` - Project dependencies

## ZMK Documentation Reference

### Behavior Overview

---
title: Behavior Overview
sidebar_label: Overview
---

# Behaviors Overview

"Behaviors" are bindings that are assigned to and triggered by key positions on keymap layers, sensors (like an encoder) or combos. They describe what happens e.g. when a certain key position is pressed or released, or an encoder triggers a rotation event. They can also be recursively invoked by other behaviors, such as macros.

Below is a summary of pre-defined behavior bindings and user-definable behaviors available in ZMK, with references to documentation pages describing them.

## Key Press Behaviors

| Binding       | Behavior                                      | Description                                                                                                                                                                                                                                       |
| ------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `&kp`         | [Key Press](key-press.md)                     | Send keycodes to the connected host when a key is pressed                                                                                                                                                                                         |
| `&mt`         | [Mod Tap](hold-tap.mdx#mod-tap)               | Sends a different key press depending on whether a key is held or tapped                                                                                                                                                                          |
| `&kt`         | [Key Toggle](key-toggle.md)                   | Toggles the press of a key. If the key is not currently pressed, key toggle will press it, holding it until the key toggle is pressed again or the key is released in some other way. If the key is currently pressed, key toggle will release it |
| `&sk`         | [Sticky Key](sticky-key.md)                   | Stays pressed until another key is pressed, then is released. It is often used for modifier keys like shift, which allows typing capital letters without holding it down                                                                          |
| `&gresc`      | [Grave Escape](mod-morph.md#behavior-binding) | Sends Grave Accent `` ` `` keycode if shift or GUI is held, sends Escape keycode otherwise                                                                                                                                                        |
| `&caps_word`  | [Caps Word](caps-word.md)                     | Behaves similar to caps lock, but automatically deactivates when any key not in a continue list is pressed, or if the caps word key is pressed again                                                                                              |
| `&key_repeat` | [Key Repeat](key-repeat.md)                   | Sends again whatever keycode was last sent                                                                                                                                                                                                        |

## Miscellaneous Behaviors

| Binding  | Behavior                           | Description                                                                                                                           |
| -------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `&trans` | [Transparent](misc.md#transparent) | Passes the key press down to the next active layer in the stack for processing                                                        |
| `&none`  | [None](misc.md#none)               | Swallows and stops the key press, no keycode will be sent nor will the key press be passed down to the next active layer in the stack |

## Layer Navigation Behaviors

| Binding | Behavior                                     | Description                                                                                              |
| ------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `&mo`   | [Momentary Layer](layers.md#momentary-layer) | Enables a layer while a key is pressed                                                                   |
| `&lt`   | [Layer-tap](layers.md#layer-tap)             | Enables a layer when a key is held, and outputs a key press when the key is only tapped for a short time |
| `&to`   | [To Layer](layers.md#to-layer)               | Enables a layer and disables all other layers except the default layer                                   |
| `&tog`  | [Toggle Layer](layers.md#toggle-layer)       | Enables a layer until the layer is manually disabled                                                     |
| `&sl`   | [Sticky Layer](sticky-layer.md)              | Activates a layer until another key is pressed, then deactivates it                                      |

## Mouse Emulation Behaviors

| Binding | Behavior                                                    | Description                     |
| ------- | ----------------------------------------------------------- | ------------------------------- |
| `&mkp`  | [Mouse Button Press](mouse-emulation.md#mouse-button-press) | Emulates pressing mouse buttons |
| `&mmv`  | [Mouse Move](mouse-emulation.md#mouse-move)                 | Emulates mouse movement         |
| `&msc`  | [Mouse Scroll](mouse-emulation.md#mouse-scroll)             | Emulates mouse scrolling        |

## Reset Behaviors

| Binding       | Behavior                                | Description                                                                              |
| ------------- | --------------------------------------- | ---------------------------------------------------------------------------------------- |
| `&sys_reset`  | [Reset](reset.md#reset)                 | Resets the keyboard and re-runs the firmware flashed to the device                       |
| `&bootloader` | [Bootloader](reset.md#bootloader-reset) | Resets the keyboard and puts it into bootloader mode, allowing you to flash new firmware |

## Output Selection Behaviors

| Binding | Behavior                                                 | Description                                                                                        |
| ------- | -------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `&bt`   | [Bluetooth](bluetooth.md#bluetooth-behavior)             | Completes a bluetooth action given on press, for example switching between devices                 |
| `&out`  | [Output Selection](outputs.md#output-selection-behavior) | Allows selecting whether output is sent to the USB or bluetooth connection when both are connected |

## Lighting Behaviors

| Binding   | Behavior                                       | Description                                                                  |
| --------- | ---------------------------------------------- | ---------------------------------------------------------------------------- |
| `&rgb_ug` | [RGB Underglow](underglow.md#behavior-binding) | Controls the RGB underglow, usually placed underneath the keyboard           |
| `&bl`     | [Backlight](backlight.md#behavior-binding)     | Controls the keyboard backlighting, usually placed through or under switches |

## Power Management Behaviors

| Binding      | Behavior                                      | Description                                                     |
| ------------ | --------------------------------------------- | --------------------------------------------------------------- |
| `&ext_power` | [Power management](power.md#behavior-binding) | Allows enabling or disabling the VCC power output to save power |
| `&soft_off`  | [Soft off](soft-off.md#behavior-binding)      | Turns the keyboard off.                                         |

## ZMK Studio Behaviors

| Binding          | Behavior                                               | Description                                               |
| ---------------- | ------------------------------------------------------ | --------------------------------------------------------- |
| `&studio_unlock` | [ZMK Studio Unlock](studio-unlock.md#behavior-binding) | Unlocks the device so that ZMK Studio UI can make changes |

## User-Defined Behaviors

| Behavior                            | Description                                                                                         |
| ----------------------------------- | --------------------------------------------------------------------------------------------------- |
| [Macros](macros.md)                 | Allows configuring a list of other behaviors to invoke when the key is pressed and/or released      |
| [Hold-Tap](hold-tap.mdx)            | Invokes different behaviors depending on key press duration or interrupting keys.                   |
| [Tap Dance](tap-dance.mdx)          | Invokes different behaviors corresponding to how many times a key is pressed                        |
| [Mod-Morph](mod-morph.md)           | Invokes different behaviors depending on whether a specified modifier is held during a key press    |
| [Sensor Rotation](sensor-rotate.md) | Invokes different behaviors depending on whether a sensor is rotated clockwise or counter-clockwise |

### Keymap Structure

## <details>

title: Keymaps & Behaviors
sidebar_label: Keymaps

---

import KeymapExample from "../keymap-example.md";

ZMK uses a declarative approach to keymaps, using devicetree syntax to configure them in a `<keyboard>.keymap` file.
This declarative configuration defines the key mappings, behaviors used within them and the configuration of certain features.
These keymaps can then be updated dynamically/at runtime using the [ZMK Studio](../features/studio.md) feature,
which allows keymap modifications over USB or BLE.

## Stock and User Keymaps

All keyboard definitions (complete boards or shields) include the _default_ keymap for that keyboard,
so ZMK can produce a "stock" firmware for that keyboard without any further modifications.
When users complete the [user setup](../user-setup.mdx), the stock `.keymap` file is copied to the
user config directory, which can be used to [customize](../customization.md) the keymap to each user's liking.

## Behaviors

ZMK implements the concept of "behaviors", which can be bound to a certain key position, sensor (encoder),
or layer, to perform certain actions when events occur for that binding (e.g. when a certain key position
is pressed or released, or an encoder triggers a rotation event).

For example, the simplest behavior in ZMK is the "key press" behavior, which responds to key position
(a certain spot on the keyboard), and when that position is pressed, send a keycode to the host, and
when the key position is released, updates the host to notify of the keycode being released.

For the full set of possible behaviors, see the [overview page for behaviors](behaviors/index.mdx).

## Layers

Like many mechanical keyboard firmwares, ZMK keymaps are composed of a collection of layers, with a
minimum of at least one layer that is the default, usually near the bottom of the "layer stack". Each layer
in ZMK contains a set of bindings that bind a certain behavior to a certain key position in that layer.

|                                                        ![Diagram of three layers](../assets/keymaps/layer-diagram.png)                                                        |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| _A simplified diagram showing three layers. The layout of each layer is the same (they all contain four keys), but the behavior bindings within each layer can be different._ |

All layers are assigned and referred to by a natural number, with the base layer being layer `0`. It is common to [use the C preprocessor to "name" layers](behaviors/layers.md#defines-to-refer-to-layers), making them more legible.

The default layer (the base layer with index 0) is always enabled. Certain bound behaviors may enable/disable additional layers.

When a key location is pressed/released, the _highest-valued currently active_ layer is used. The press/release event is sent to the behavior bound at that position in said layer, for it to perform whatever actions it wants to in reaction to the event. The behavior can choose to "consume" the event, or "pass it along" and let the next highest-valued active layer _also_ get the event (whose behavior may continue "passing it along").

Note that the _activation_ order isn't relevant for determining the priority of active layers, it is determined _only_ by the definition order.

:::tip
If you wish to use multiple base layers (with a [toggle](behaviors/layers.md#toggle-layer)), e.g. one for QWERTY and another for Colemak layouts, you will want these layers to have the lowest value possible. In other words, one should be layer `0`, and the other should be layer `1`. This allows other momentary layers activated on top of them to work with both.
:::

## Behavior Bindings

Binding a behavior at a certain key position may include up to two extra parameters that are used to
alter the behavior when that specific key position is activated/deactivated. For example, when binding
the "key press" (`kp`) behavior at a certain key position, you must specify _which_ keycode should
be used for that key position.

```dts
&kp A
```

In this case, the `A` is actually a define for the raw HID keycode, to make keymaps easier to read and write.

For example of a binding that uses two parameters, you can see how "mod-tap" (`mt`) is bound:

```dts
&mt LSHIFT D
```

Here, the first parameter is the set of modifiers that should be used for the "hold" behavior, and the second
parameter is the keycode that should be sent when triggering the "tap" behavior.

## Keymap File

A keymap file is composed of several sections, that together make up a valid devicetree file for describing the keymap and its layers.

### Includes

The devicetree files are actually preprocessed before being finally leveraged by Zephyr. This allows using standard C defines to create meaningful placeholders
for what would otherwise be cryptic integer keycodes, etc. This also allows bringing in _other_ devicetree nodes from separate files.

The top two lines of most keymaps should include:

```dts
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
```

The first defines the nodes for all the available behaviors in ZMK, which will be referenced in the behavior bindings. This is how bindings like `&kp` can reference the key press behavior defined with an anchor name of `kp`.

The second include brings in the defines for all the keycodes (e.g. `A`, `N1`, `C_PLAY`) and the modifiers (e.g. `LSHIFT`) used for various behavior bindings.

### Root Devicetree Node

All the remaining keymap nodes will be nested inside of the root devicetree node, like so:

```dts
/ {
    // Everything else goes here!
};
```

### Keymap Node

Nested under the devicetree root, is the keymap node. The node _name_ itself is not critical, but the node **MUST** have a property
`compatible = "zmk,keymap"` in order to be used by ZMK.

```dts
    keymap {
        compatible = "zmk,keymap";

        // Layer nodes go here!
    };
```

### Layers

Each layer of your keymap will be nested under the keymap node. Here is an example of a layer in a 6-key macropad.

```dts
    keymap {
        compatible = "zmk,keymap";

        default_layer { // Layer 0
            display-name = "Base";
// ----------------------------------------------
// |     Z     |     M     |     K     |
// |     A     |     B     |     C     |
            bindings = <
                &kp Z    &kp M    &kp K
                &kp A    &kp B    &kp C
            >;
        };
    };
```

Each layer should have:

1. A `bindings` property that will be a list of [behavior bindings](behaviors/index.mdx), one for each key position for the keyboard.
1. (Optional) A `sensor-bindings` property that will be a list of behavior bindings for each sensor on the keyboard. (Currently, only encoders are supported as sensor hardware, but in the future devices like trackpoints would be supported the same way)
1. (Optional) A `display-name` property that is a string used by certain features, such as ZMK Studio and the layer status display widget.

### Multiple Layers

Layers are numbered in the order that they appear in keymap node - the first layer is `0`, the second layer is `1`, etc.

Here is an example of a trio of layers for a simple 6-key macropad:

<KeymapExample />

:::note
Even if layer `1` was to be activated after `2`, layer `2` would still have priority as it is higher valued. Behaviors such as [To Layer (`&to`)](behaviors/layers.md#to-layer) can be used to enable one layer _and disable all other non-default layers_, though.
:::

### Complete Example

Some examples of complete keymaps for a keyboard are:

- [`corne.keymap`](https://github.com/zmkfirmware/zmk/blob/main/app/boards/shields/corne/corne.keymap)
- [`kyria.keymap`](https://github.com/zmkfirmware/zmk/blob/main/app/boards/shields/kyria/kyria.keymap)
- [`lily58.keymap`](https://github.com/zmkfirmware/zmk/blob/main/app/boards/shields/lily58/lily58.keymap)

:::tip
Every keyboard comes with a "default keymap". For additional examples, the [ZMK tree's `app/boards` folder](https://github.com/zmkfirmware/zmk/blob/main/app/boards) can be browsed.
:::

</details>

### Layer Management

<!-- TODO: Add layer behaviors documentation (&mo, &lt, &to, &tog) -->

### Hold-Tap Behavior

<!-- TODO: Add hold-tap documentation (mod-tap, layer-tap, timing configs) -->

### Mod-Morph Behavior

<!-- TODO: Add mod-morph documentation (syntax, modifiers, examples) -->

### Combos

<!-- TODO: Add combos documentation (syntax, timing, key positions) -->

### Tap-Dance Behavior

<!-- TODO: Add tap-dance documentation when needed -->

### Conditional Layers

<!-- TODO: Add conditional layers documentation when needed -->

### Sticky Keys

<!-- TODO: Add sticky keys documentation when needed -->

### Encoders

<!-- TODO: Add encoder documentation when needed -->

### Bluetooth Management

<!-- TODO: Add bluetooth behavior documentation when needed -->

### Mouse Emulation

<!-- TODO: Add mouse emulation documentation when needed -->

### Key Codes Reference

<!-- TODO: Add key codes reference when needed -->

### Low Power States

---
title: Low Power States
sidebar_label: Low Power States
---

## Idle

In the idle state, peripherals such as displays and lighting are disabled, but the keyboard remains connected to Bluetooth so it can immediately respond when you press a key. Idle state is entered automatically after a timeout period that is [30 seconds by default](../config/power.md#low-power-states).

## Deep Sleep

In the deep sleep state, the keyboard enters a software power-off state. Among others, this:

- Disconnects the keyboard from all Bluetooth connections
- Disables any peripherals such as displays and lighting
- If possible, disables external power output
- Clears the contents of RAM, including any unsaved [Studio](studio.md) changes

This state uses very little power, but it may take a few seconds to reconnect after waking. A [wakeup source](#wakeup-sources) is required to wake from deep sleep.

### Config

Deep sleep must be enabled via its corresponding [config](../config/power.md#low-power-states).

### Wakeup Sources

Using deep sleep requires `kscan` nodes to have the `wakeup-source` property to enable them to wake the keyboard, e.g.:

```dts
/ {
    kscan: kscan {
        compatible = "zmk,kscan-gpio-matrix";
        diode-direction = "col2row";
        wakeup-source;

        ...
    };
};
```

It is recommended to add the `wakeup-source` property to `kscan` devices even if the deep sleep feature is not used -- there is no downside to it.

## Soft Off

The soft off feature is used to turn the keyboard on and off explicitly, rather than through a timeout like the deep sleep feature. Depending on the keyboard, this may be through a dedicated on/off push button defined in hardware, or merely through an additional binding in the keymap to turn the device off and an existing reset button to turn the device back on.

The feature is intended as an alternative to using a hardware switch to physically cut power from the battery to the keyboard. This can be useful for existing PCBs not designed for wireless that don't have a power switch, or for new designs that favor a push button on/off like found on other devices. It yields power savings comparable to the deep sleep state.

:::note

The device enters the same software power-off state as in deep sleep, but is significantly more restrictive in the sources which can wake it. Power is _not_ technically removed from the entire system, unlike a hardware switch.

:::

A device can be put in the soft off state by:

- Triggering a hardware-defined dedicated GPIO pin, if one exists;
- Triggering the [soft off behavior](../keymaps/behaviors/soft-off.md) from the keymap.

Once in the soft off state, the device can only be woken up by:

- Triggering any GPIO pin specified to enable waking from sleep, if one exists;
- Pressing a reset button found on the device.

The GPIO pin used to wake from sleep can be a hardware-defined one, such as for a dedicated on-off push button, or it can be a single specific key switch reused for waking up (which may be accidentally pressed, e.g. while the device is being carried in a bag). To allow the simultaneous pressing of multiple key switches to trigger and exit soft off, some keyboards make use of additional hardware to integrate the dedicated GPIO pin into the keyboard matrix.

### Config

Soft off must be enabled via [its corresponding config](../config/power.md#low-power-states) before it can be used.

### Using Soft Off

If your keyboard has hardware designed to take advantage of soft off, refer to your keyboard's documentation.

For keyboards which do not have such hardware, using soft off is as simple as placing the [soft off behavior](../keymaps/behaviors/soft-off.md) in your keymap and then invoking it.

You can then wake up the keyboard by pressing the reset button once, and repeating this for each side for split keyboards.

### Adding Soft Off to a Keyboard

Please refer to the [corresponding page under hardware integration](../development/hardware-integration/soft-off-setup.mdx) for details.

### Power Management Configuration

---
title: Power Management Configuration
sidebar_label: Power Management
---

See [Configuration Overview](index.md) for instructions on how to
change these settings.

## Low Power States

Configuration for entering [low power states](../features/low-power-states.md) when the keyboard is idle.

### Kconfig

Definition file: [zmk/app/Kconfig](https://github.com/zmkfirmware/zmk/blob/main/app/Kconfig)

| Config                          | Type | Description                                                         | Default |
| ------------------------------- | ---- | ------------------------------------------------------------------- | ------- |
| `CONFIG_ZMK_IDLE_TIMEOUT`       | int  | Milliseconds of inactivity before entering idle state               | 30000   |
| `CONFIG_ZMK_SLEEP`              | bool | Enable deep sleep support                                           | n       |
| `CONFIG_ZMK_IDLE_SLEEP_TIMEOUT` | int  | Milliseconds of inactivity before entering deep sleep               | 900000  |
| `CONFIG_ZMK_PM_SOFT_OFF`        | bool | Enable soft off functionality from the keymap or dedicated hardware | n       |

## External Power Control

Driver for enabling or disabling power to peripherals such as displays and lighting. This driver must be configured to use [power management behaviors](../keymaps/behaviors/power.md).

### Kconfig

Definition file: [zmk/app/Kconfig](https://github.com/zmkfirmware/zmk/blob/main/app/Kconfig)

| Config                 | Type | Description                                     | Default |
| ---------------------- | ---- | ----------------------------------------------- | ------- |
| `CONFIG_ZMK_EXT_POWER` | bool | Enable support to control external power output | y       |

### Devicetree

Applies to: `compatible = "zmk,ext-power-generic"`

| Property        | Type       | Description                                                   |
| --------------- | ---------- | ------------------------------------------------------------- |
| `control-gpios` | GPIO array | List of GPIOs which should be active to enable external power |
| `init-delay-ms` | int        | number of milliseconds to delay after initializing the driver |

## GPIO Key Wakeup Trigger

A device similar to a [kscan](./kscan.md) which will be enabled only when the keyboard is entering [soft off](../features/low-power-states.md#soft-off) state. This is used to configure a GPIO key to wake the keyboard from [soft off](../features/low-power-states.md#soft-off) once it is pressed.

### Devicetree

Applies to: `compatible = "zmk,gpio-key-wakeup-trigger"`

Definition file: [zmk/app/dts/bindings/zmk,gpio-key-wakeup-trigger.yaml](https://github.com/zmkfirmware/zmk/blob/main/app/dts/bindings/zmk%2Cgpio-key-wakeup-trigger.yaml)

| Property        | Type       | Description                                                                                   |
| --------------- | ---------- | --------------------------------------------------------------------------------------------- |
| `trigger`       | phandle    | Phandle to a GPIO key to be used to wake from soft off                                        |
| `wakeup-source` | bool       | Mark this device as able to wake the keyboard                                                 |
| `extra-gpios`   | GPIO array | list of GPIO pins (including the appropriate flags) to set active before going into power off |

The `wakeup-source` property should always be present for this node to be useful. The `extra-gpios` property should be used to ensure the GPIO pin will trigger properly to wake the keyboard. For example, for a `col2row` matrix kscan, these are the column pins relevant for soft off.

## Soft Off Wakeup Sources

Selects a list of devices to enable during [soft off](../features/low-power-states.md#soft-off), allowing those with `wakeup-source` as a property to wake the keyboard.

### Devicetree

Applies to: `compatible = "zmk,soft-off-wakeup-sources"`

