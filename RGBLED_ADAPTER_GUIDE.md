# RGB LED Adapter Shield - Keymap Integration Guide

## Overview

The `rgbled_adapter` shield works with the `zmk-rgbled-widget` module to provide RGB LED control on your keyboard. The shield uses the built-in RGB LED on the XIAO BLE board.

## Available Behaviors

When using the `rgbled_adapter` shield, you can use these behaviors in your keymap:

### 1. Battery Indicator: `&ind_bat`
Shows the current battery level by blinking the LED with a color based on battery percentage:
- **Green**: Battery > 80%
- **Yellow**: Battery 20-80%
- **Red**: Battery < 20%
- **Red (blinking)**: Battery < 10% (critical)

### 2. Connection Indicator: `&ind_con`
Shows the Bluetooth connection status:
- **Blue**: Connected
- **Red**: Disconnected
- **Cyan**: Pairing mode

### 3. Layer Indicator: `&ind_layer`
Shows the currently active layer by blinking a color corresponding to the layer number.

## How to Use in Your Keymap

### Basic Usage

Simply replace any `&trans` (transparent) or add to any key position in your keymap:

```dts
bindings = <
    &kp Q    &kp W    &kp E    &ind_bat    // Battery indicator on 4th key
    &kp A    &kp S    &kp D    &ind_con    // Connection indicator on 4th key
    // ... rest of your keys
>;
```

### Example: Adding to ADJ Layer

Here's how you could add LED indicators to your adjust layer:

```dts
adjust_layer {
    bindings = <
        &sys_reset   &bt BT_CLR  &out OUT_TOG  &ind_bat  &ind_con    // Battery & connection indicators
        &bootloader  &bt BT_NXT  &ind_layer   &trans    &trans      // Layer indicator
        // ... rest of your keys
    >;
};
```

### Example: Adding to Base Layer

You could add a battery check key to your base layer:

```dts
base_layer {
    bindings = <
        &kp Q    &kp W    &kp E    &kp R    &kp T    &kp Y    &kp U    &kp I    &kp O    &kp P
        &kp A    &kp S    &kp D    &kp F    &kp G    &kp H    &kp J    &kp K    &kp L    &ind_bat  // Battery on semicolon key
        // ... rest of your keys
    >;
};
```

## Configuration Options

In your `config/totem.conf`, you can configure:

```ini
CONFIG_RGBLED_WIDGET=y
CONFIG_RGBLED_WIDGET_BATTERY_LEVEL_HIGH=80
CONFIG_RGBLED_WIDGET_BATTERY_LEVEL_LOW=20
CONFIG_RGBLED_WIDGET_BATTERY_LEVEL_CRITICAL=10
CONFIG_RGBLED_WIDGET_BATTERY_SHOW_PERIPHERALS=y  # For split keyboards
```

## Automatic vs Manual Indication

- **Automatic**: The LED will automatically blink periodically to show battery/connection status (configured via `CONFIG_RGBLED_WIDGET`)
- **Manual**: Using `&ind_bat`, `&ind_con`, or `&ind_layer` in your keymap allows you to trigger the indicator on-demand with a keypress

## Notes

- The behaviors are available once you've added the `rgbled_adapter` shield to your `build.yaml`
- The `zmk-rgbled-widget` module must be included in your `west.yml` (already done)
- Each behavior will cause the LED to blink/show the appropriate color when the key is pressed
- The LED uses the built-in RGB LED on the XIAO BLE board (no external hardware needed)

