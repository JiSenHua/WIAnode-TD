# WIAnode_JISEN

**Connect WIAnode to TouchDesigner and bring sensor data into your interactive projects.**

[简体中文](README.md) | English

WIAnode_JISEN is a third-party TouchDesigner component created by 基森 (JiSenHua). It communicates with DFRobot WIAnode over MQTT to receive sensor data in TouchDesigner and send control data to devices connected to WIAnode. Use it for interactive installations, real-time visuals, lighting experiments, and creative prototypes.

## Features

- **Real-time sensor input**: Bring data from buttons, knobs, distance sensors, and other configured devices into your CHOP workflow.
- **Device output control**: Control LED strips, servos, and other supported devices through WIAnode, depending on the hardware and port configuration.
- **Connection status and reconnect**: Enter the device IP address, check the connection status, and reconnect when needed.
- **Dynamic port configuration**: Controls reflect the device configuration. Ports configured as inputs do not expose editable output controls.
- **Built-in reference links**: Use the sensor reference guide (「自查手册」) to check ports and configuration tags, or open the official documentation (「官方文档」) for details.
- **TouchDesigner 2023 / 2025**: The author's tutorial states support for both release families. Compatibility with individual builds should be verified in your environment.


## Requirements and preparation

- Install TouchDesigner and prepare a WIAnode device with the sensors or output devices you need.
- Configure Wi-Fi and ports according to the [official WIAnode Wiki (Chinese)](https://wiki.dfrobot.com.cn/WIAnode). Power-cycle the device after editing `config.txt`.
- Connect the computer and WIAnode to the same local network and ensure they can communicate. The computer may use a wired connection to the same router.
- Read the current IP address from the device display.

> The wireless connection is between WIAnode and the computer. Sensors, LED strips, and servos still require physical connections to WIAnode and suitable power.

## Quick start

1. Drag `WIAnode_JISEN.tox` into TouchDesigner.
2. Enter the actual IP address of your WIAnode in the component.
3. Click 「重新连接」 (Reconnect) and confirm that the status reads 「已连接」 (Connected).
4. Connect a **Null CHOP** to the component's CHOP output.
5. Press a connected button or move an object in front of a distance sensor and check that the corresponding channel values change.
6. Use a **Select CHOP** to isolate the channels you need. Process them with operators such as **Math CHOP** or **Limit CHOP**, then use them in your visuals or interaction logic.

```text
Sensors → WIAnode → LAN / MQTT → WIAnode_JISEN → CHOPs → Interactive content
TouchDesigner control data → WIAnode_JISEN → WIAnode → LED strip / Servo
```

Channel names, channel counts, and value ranges depend on the connected devices. Check the actual output before mapping values.

## Example: control a WS2812 LED strip

This workflow follows the tutorial's seven-pixel example. Configure the physical port used by the strip as `WS2812` before proceeding.

1. Create a **Constant TOP** with a resolution of `7 × 1`. For other strip lengths, adjust the resolution to match the pixel count.
2. Use **TOP to CHOP** to read the colors, keeping RGB and removing Alpha.
3. Map color values from `0–1` to `0–255`.
4. Use **Shuffle CHOP** with `Split All Samples` to split the data.
5. Use **Reorder CHOP** with `Merge N Groups` and set N to `3`, arranging the values as `R, G, B, R, G, B…`.
6. Add a **Null CHOP** and assign it to the component's control field for the LED strip port. The tutorial uses P4; your selection must match your wiring and device configuration.

```text
Constant TOP → TOP to CHOP → Shuffle CHOP → Reorder CHOP → Null CHOP
                                                              ↓
                                  Component's LED strip port control
```

Change the TOP's color to drive the strip, or use sensor data to control its color and brightness.

## Ports and device configuration

Open 「自查手册」 in the component to check the physical ports and configuration tags for your devices. For the full hardware documentation, visit the [official DFRobot WIAnode Wiki (Chinese)](https://wiki.dfrobot.com.cn/WIAnode), or use the 「官方文档」 button in the component.

- Physical wiring must match the device configuration.
- After changing a port's function, save the configuration, power-cycle WIAnode, and reconnect the component.
- If an output control is disabled, check whether that port is configured as an input.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| Cannot connect | Confirm that WIAnode is on the network, the computer can reach it, and its IP address has not changed. Then reconnect. |
| Connected, but no sensor data | Check sensor power, wiring, port configuration, and device detection, then inspect the component's output channels. |
| A port control is disabled | Check whether the port is configured as an input. After updating the device configuration and restarting it, reconnect the component. |
| Incorrect LED colors or brightness | Check the pixel count, RGB order, `0–255` value range, port configuration, and power supply. |

## Feedback

Report problems or suggest improvements through [GitHub Issues](https://github.com/JiSenHua/WIAnode-TD/issues). Include your full TouchDesigner version, component version, WIAnode port configuration, steps to reproduce, and relevant screenshots or error messages.

## Author and license

Created by **基森 / [JiSenHua](https://github.com/JiSenHua)**.

This is a third-party TouchDesigner component. WIAnode hardware and its official documentation are provided by DFRobot.