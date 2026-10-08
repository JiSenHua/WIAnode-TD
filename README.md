# WIAnode_JISEN

**连接 WIAnode 与 TouchDesigner，让传感器数据进入你的交互创作。**

简体中文 | [English](README_EN.md)

WIAnode_JISEN 是由基森（JiSenHua）制作的 TouchDesigner 第三方组件，通过 MQTT 与 DFRobot WIAnode 通信，将传感器输入接入 TouchDesigner，并将控制数据发送至连接在 WIAnode 上的设备。适用于交互装置、实时视觉、灯光实验和创意原型。

## 功能特点

- **实时传感器输入**：接收按钮、旋钮、距离等传感器的数据，接入 CHOP 工作流。
- **设备输出控制**：通过 WIAnode 控制灯带、舵机等设备，具体能力取决于硬件及接口配置。
- **连接状态与重新连接**：填写设备 IP，查看连接状态，并按需重新连接。
- **动态接口配置**：根据设备的接口配置更新控制项；配置为输入的接口不会开放对应的输出编辑。
- **内置参考入口**：通过「自查手册」查询传感器接口与配置标签，通过「官方文档」查看详细说明。
- **TouchDesigner 2023 / 2025**：按作者教程说明支持这两个版本系列，具体 Build 的兼容性以实际使用为准。


## 使用前准备

- 安装 TouchDesigner，并准备 WIAnode 主机及所需的传感器或输出设备。
- 按 [WIAnode 官方 Wiki](https://wiki.dfrobot.com.cn/WIAnode) 完成 Wi-Fi 和接口配置，修改 `config.txt` 后重新给设备上电。
- 确保电脑与 WIAnode 处于可互相访问的同一局域网。电脑可以使用该路由器的有线网络。
- 通过设备屏幕获取当前 IP 地址。

> 无线连接发生在 WIAnode 与电脑之间；传感器、灯带和舵机仍需通过线缆连接 WIAnode，并满足各自的供电要求。

## 快速开始

1. 将 `WIAnode_JISEN.tox` 拖入 TouchDesigner。
2. 在组件中填写 WIAnode 的实际 IP 地址。
3. 点击「重新连接」，确认状态显示「已连接」。
4. 在组件的 CHOP 输出后连接一个 **Null CHOP**。
5. 按下按钮或改变距离传感器前的物体位置，观察对应通道数值是否变化。
6. 使用 **Select CHOP** 选出所需通道，再通过 **Math CHOP**、**Limit CHOP** 等处理数据，连接到你的视觉或交互逻辑。

```text
传感器 → WIAnode → 局域网 / MQTT → WIAnode_JISEN → CHOP → 交互内容
TouchDesigner 控制数据 → WIAnode_JISEN → WIAnode → 灯带 / 舵机
```

通道名称、数量和数值范围取决于所接设备，请以实际输出为准。

## 示例：控制 WS2812 灯带

以下流程对应教程中的 7 颗灯珠示例，使用前需将实际连接的接口配置为 `WS2812`。

1. 创建 **Constant TOP**，分辨率设为 `7 × 1`；其他长度的灯带按实际灯珠数量调整。
2. 使用 **TOP to CHOP** 读取颜色，仅保留 RGB，去掉 Alpha。
3. 将颜色值从 `0–1` 映射到 `0–255`。
4. 使用 **Shuffle CHOP** 的 `Split All Samples` 拆分数据。
5. 使用 **Reorder CHOP** 的 `Merge N Groups`，将 N 设为 `3`，按 `R、G、B、R、G、B…` 排列。
6. 在末尾连接 **Null CHOP**，将其指定到组件中对应的灯带接口控制项。教程使用 P4，实际应与接线及设备配置一致。

```text
Constant TOP → TOP to CHOP → Shuffle CHOP → Reorder CHOP → Null CHOP
                                                                        ↓
                                                     组件对应的灯带接口控制项
```

改变 TOP 的颜色，即可驱动灯带；也可以用传感器数据控制颜色或亮度。

## 接口与设备配置

使用组件中的「自查手册」确认设备对应的物理接口与配置标签。完整硬件说明可直接查看 [DFRobot WIAnode 官方 Wiki](https://wiki.dfrobot.com.cn/WIAnode)，也可通过组件中的「官方文档」入口打开。

- 传感器的实际接线必须与设备配置一致。
- 修改接口用途后，保存配置并重新给 WIAnode 上电，再在组件中重新连接。
- 如果某个输出控制项呈灰色，先检查该接口是否被配置为输入。

## 常见问题

| 问题 | 排查方法 |
| --- | --- |
| 无法连接 | 确认 WIAnode 已联网、电脑与设备可互相访问，并检查 IP 是否发生变化，然后重新连接。 |
| 显示已连接，但没有数据 | 检查传感器供电、接线、接口配置和设备识别状态，再查看组件的实际输出通道。 |
| 接口控制项无法编辑 | 检查接口是否被配置为输入；修改设备配置并重启后，重新连接组件。 |
| 灯带颜色或亮度异常 | 检查灯珠数量、RGB 顺序、`0–255` 数值范围，以及接口配置与供电。 |

## 反馈

欢迎通过 [GitHub Issues](https://github.com/JiSenHua/WIAnode-TD/issues) 提交问题或建议。反馈时请附上 TouchDesigner 完整版本号、插件版本、WIAnode 接口配置、复现步骤，以及相关截图或错误信息。

## 作者与许可

作者：**基森 / [JiSenHua](https://github.com/JiSenHua)**。

本项目为第三方 TouchDesigner 组件。WIAnode 硬件及其官方文档由 DFRobot 提供。